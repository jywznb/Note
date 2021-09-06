# SortKey

## Background

​    当前Arctic的 Copy-On-Write 功能存在 实时性不够 的问题。实时性不够主要是针对“近实时”的要求，要做到秒级以下的响应，单纯靠COW是难以做到的，因此需要引入 Merge-On-Read 功能。Merge-On-Read 可以实时的合并 Change 数据和 Base 数据，进行过滤、去重等操作后将正确结果返回给用户，对用户来说感知不到 Change 数据和 Base 数据的区别。另外，有了 MOR 功能后，我们可以在元数据层支持更细粒度的提交，从而实现可控的Compaction plan，例如：针对部分分区只有少量 Change 数据的情况，我们可以推迟 COW 操作的执行，等到 Change Data 的大小到达了一定阈值（或在业务低峰期的某个固定时刻），我们再启动 COW。

​    MOR的性能高度依赖于 Base 数据映射下推和谓词下推的效果，前者能充分利用Parquet列式存储的优势，在获取表中原始数据时只需要扫描查询中需要的列，由于每一列的所有值都是连续存储的，所以分区取出每一列的所有值就可以实现 TableScan 算子，而避免扫描整个表文件内容；后者在Parquet中原生就支持映射下推，执行查询的时候可以传递谓词查询条件到Parquet的Column Chunk中，Column Chunk元信息记录了Column Chunk的最大值、最小值和空值个数，通过这些统计值和该列的过滤条件可以判断该Row Group是否需要扫描。

​    Parquet谓词下推功能能大幅度的提高Parquet文件搜索的性能，但是如果数据原来就是乱序或者存储的顺序和当前谓词排序不符，则谓词下推的性能会大幅度降低。因此，SortKey是具体业务场景下提升MOR的关键点。

## Architecture

### Sort in Compaction

   SortKey涉及到以下四种场景：

- 无主键无sortKey
- 无主键有sortKey
- 有主键无sortKey
- 有主键有sortKey
  - 有主键有sortKey，且主键和sortKey相同
  - 有主键有sortKey，且主键和sortKey不同

  对于无主键的情况，数据只会单纯的append，而不存在upsert。因此在COW时，不需要对Change数据进行去重，不需要对Base数据求Range相交，只需要通过装箱算法进行小文件合并即可。在此场景下增加sortKey，需要在COW时，对合并的文件进行排序后落盘。

  对于有主键的情况，情况较为复杂，Change数据之间需要依靠主键去重，Change数据和Base数据之间需要依靠主键去重。因此，主键的Range相交情况大幅度影响COW和MOR的性能，并且由于没有合适的拆分Range的方式，当文件大小增大时，目前没有一个合适的方案去分割COW产生的大文件。在当前场景下引入SortKey：

- 有主键有sortKey，且主键和sortKey相同。如果主键和sortKey相同，文件的PrimaryKey Range和SortKey Range是相同的，则在COW时，我们需要在对Change数据去重后进一步按主键排序。然后将排序后的Change数据按顺序分配到相交Base数据的Primary Key Range内，依次写入落盘。主键和sortKey相同带来的最大好处是：因为COW生成的Base数据是有序的，因此能自然的按大小拆分数据（128M），并且文件的Range排列整齐，能有效减少下一次COW的损耗。
- 有主键有sortKey，且主键和sortKey不同。如果主键和sortKey不同，文件的PrimaryKey Range和SortKey Range是不同的。如何将去重并排序后的Change数据，写入Base数据是一个难点（存在多个按SortKey排序好的Base文件）。能想到的解决方式是：让Change数据拆分成Insert（保证是Insert）和Delete数据，然后将Insert数据按顺序分配到相交Base数据的SortKey Range内，然后边写入边delete数据。主键和sortKey不同最终能提升MOR性能，但对于COW会进一步打乱Base文件的PrimaryKey Range，从而增加COW负担。而在内存中手动将Change数据拆分为Insert和Delete数据难度也是比较大的

  除此之外，增加SortKey还会带来以下问题：

- 不支持原地升级，需要对历史数据进行初次排序
- 需要强大的外排功能支持，无论是排序Change数据还是已有的Base数据
- sortKey用户修改后，需要重新排序已有base文件
- 增加Change数据排序步骤，copy on write性能下降
- COW场景变得复杂，需要为不同的场景定制COW代码

  造成当前Arctic的COW如此复杂的原因：当前COW依赖于Range相交，导致每个MergeTask中存在多个Base文件和Change文件，形成了复杂的多对多的关系，不仅增加代码复杂度还降低了性能。是否考虑后续参考hudi和iceberg，让Base数据和Change数据绑定起来。

  例如hudi方式，hudi在MOR和COW时，会扫描元数据中的每个FileSlice，hudi的FileSlice中只包含单个BaseFile和其绑定的多个Change数据，因此在MOR时，Spark只要按FileSlice做partition，返回去重后的Base数据的Iterator即可，COW也是相同的，只做单个Base文件和Change文件的合并

  或者使用iceberg当前的方案，在数据摄取时将update划分为Insert和Delete，最终落盘下来只有Delete数据和Insert数据，在此基础上做 COW和MOR。简化成Delete数据和Insert数据，以上场景可以简化为：

- 无主键无sortKey。在无主键的场景下，不存在Delete数据，只有Insert数据，直接小文件合并。
- 无主键有sortKey。需要Sortkey，则只需要对Insert数据进行排序，然后按SorkKey range写入落盘即可
- 有主键有sortKey，且主键和sortKey相同。同样只对Insert数据进行排序，然后按SorkKey range写入落盘，Delete数据保存在Map中，写入时进行删除
- 有主键有sortKey，且主键和sortKey不同。对Insert数据按Sortkey进行排序，然后按SorkKey range写入落盘，Delete数据保存在Map中，写入时进行删除

### Independent Sort

  以上是将SortKey放在Compaction中操作的方式，还可以考虑将Sort Base数据单独做一个批任务，该任务的工作就是去Sort当前Snapshot中的Base文件（控制一次Sort的文件数量）。该方案有以下好处：

- 将COW从上面复杂的逻辑中解耦出来
- 支持原地升级，用户能感知到的只是MOR性能的逐步提升

  该方案也存在如何保证提交一致性问题，sort任务和COW任务最终都需要进行提交，如何保证两者提交不会冲突需要复杂的调度规则来支持。