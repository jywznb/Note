# Hudi 的一些设计

### 场景

https://hudi.apache.org/docs/use_cases.html

- 近实时写入

​    \+ 减少碎片化工具的使用

​    \+ CDC 增量导入 RDBMS 数据

​    \+ 限制小文件的大小和数量

- 近实时分析

​    \+ 相对于秒级存储 (Druid, OpenTSDB) ，节省资源

​    \+ 提供分钟级别时效性，支撑更高效的查询

​    \+ Hudi 作为 lib，非常轻量

- 增量 pipeline

​    \+ 区分 arrivetime 和 event time 处理延迟数据

​    \+ 更短的调度 interval 减少端到端延迟 (小时 -> 分钟) => Incremental Processing

- 增量导出

​    \+ 替代部分 Kafka 的场景，数据导出到在线服务存储 e.g. ES

### 概念/术语

https://hudi.apache.org/docs/concepts.html

#### Timeline

![img](https://cdn.nlark.com/yuque/0/2021/png/22051639/1630805179782-2f2ffadf-20ec-4a98-a464-6a3940b4ef40.png)

Timeline 是 HUDI 用来管理提交（commit）的抽象，每个 commit 都绑定一个固定时间戳，分散到时间线上。在 Timeline 上，每个 commit 被抽象为一个 HoodieInstant，一个 instant 记录了一次提交 (commit) 的行为、时间戳、和状态。


HUDI 的读写 API 通过 Timeline 的接口可以方便的在 commits 上进行条件筛选，对 history 和 on-going 的 commits 应用各种策略，快速筛选出需要操作的目标 commit。

#### Time

Arrival time: 数据到达 Hudi 的时间，commit time

Event time: record 中记录的时间

![img](https://cdn.nlark.com/yuque/0/2021/png/22051639/1630805219358-d71fe557-8c6c-4657-a73a-08fc7caefefa.png)

上图中采用时间（小时）作为分区字段，从 10:00 开始陆续产生各种 commits，10:20 来了一条 9:00 的数据，该数据仍然可以落到 9:00 对应的分区，通过 timeline 直接消费 10:00 之后的增量更新（只消费有新 commits 的 group），那么这条延迟的数据仍然可以被消费到。



#### 文件管理

![img](https://cdn.nlark.com/yuque/0/2021/png/22051639/1630805127461-41aec9da-02fc-4636-8412-c9a1beb06acf.png)

#### 文件版本

一个新的 base commit time 对应一个新的 FileSlice，实际就是一个新的数据版本。HUDI 通过 TableFileSystemView 抽象来管理 table 对应的文件，比如找到所有最新版本 FileSlice 中的 base file （Copy On Write Snapshot 读）或者  base + log files（Merge On Read 读)。


通过 Timeline 和 TableFileSystemView 抽象，HUDI 实现了非常便捷和高效的表文件查找。

##### 文件格式

Hoodie 的每个 FileSlice 中包含一个 base file （merge on read 模式可能没有）和多个 log file （copy on write 模式没有）。


每个文件的文件名都带有其归属的 FileID（即 FileGroup Identifier）和 base commit time（即 InstanceTime）。通过文件名的 group id 组织 FileGroup 的 logical 关系；通过文件名的 base commit time 组织 FileSlice 的逻辑关系。


HUDI 的 base file (parquet 文件) 在 footer 的 meta 去记录了 record key 组成的 BloomFilter，用于在 file based index 的实现中实现高效率的 key contains 检测。只有不在 BloomFilter 的 key 才需要扫描整个文件消灭假阳。


HUDI 的 log （avro 文件）是自己编码的，通过积攒数据 buffer 以 LogBlock 为单位写出，每个 LogBlock 包含 magic number、size、content、footer 等信息，用于数据读、校验和过滤。

#### File Format

![img](https://cdn.nlark.com/yuque/0/2021/png/22051639/1630805277044-f99004bd-a5cb-4dd3-a43e-8222a51de958.png)

#### Index

Hoodie key (record key + partition path) 和 file id (FileGroup) 之间的映射关系，数据第一次写入文件后保持不变，所以，一个 FileGroup 包含了一批 record 的所有版本记录。Index 用于区分消息是 INSERT 还是 UPDATE。



##### Index 的创建过程

###### BloomFilter Index

- 新增 records 找到映射关系：record key => target partition
- 当前最新的数据 找到映射关系：partition => (fileID, minRecordKey, maxRecordKey) LIST （如果是 base files 可加速）

- 新增 records 找到需要搜索的映射关系：fileID => HoodieKey(record key + partition path) LIST，key 是候选的 fileID
- 通过 HoodieKeyLookupHandle 查找目标文件（通过 BloomFilter 加速）



###### Flink State-based Index

HUDI 在 0.8.0 版本中实现的 Flink witer，采用了 Flink 的 state 作为底层的 index 存储，每个 records 在写入之前都会先计算目标 bucket ID，不同于 BloomFilter Index，避免了每次重复的文件 index 查找。

#### Table 类型

| Table Type    | Supported Query types                                        |
| ------------- | ------------------------------------------------------------ |
| Copy On Write | Snapshot Queries + Incremental Queries                       |
| Merge On Read | Snapshot Queries + Incremental Queries + Read Optimized Queries |



##### Copy On Write


Copy On Write 类型表每次写入都会生成一个新的持有 base file

（对应写入的 instant time ） 的 FileSlice。


用户在 snapshot 读取的时候会扫描所有最新的  FileSlice 下的 base file。

![img](https://cdn.nlark.com/yuque/0/2021/png/22051639/1630805301744-6db809e9-ed3d-4b54-960a-8289f33f9af3.png)

##### Merge On Read

Merge On Read 表的写入行为，依据 index 的不同会有细微的差别：

- 对于 BloomFilter 这种无法对 log file 生成 index 的索引方案，对于  INSERT 消息仍然会写 base file （parquet format），只有 UPDATE 消息会 append log 文件（因为 base file 总已经记录了该 UPDATE 消息的 FileGroup ID）。
- 对于可以对 log file 生成 index 的索引方案，例如 Flink writer 中基于 state 的索引，每次写入都是 log format，并且会不断追加和 roll over。


Merge On Read 表的读在 READ OPTIMIZED 模式下，只会读最近的经过 compaction 的 commit。

![img](https://cdn.nlark.com/yuque/0/2021/png/22051639/1630805320402-69d051a5-4337-48db-95d4-ca547c765908.png)



### 数据写

#### 写操作

1. UPSERT：默认行为，数据先通过 index 打标(INSERT/UPDATE)，有一些启发式算法决定消息的组织以优化文件的大小 => CDC 导入
2. INSERT：跳过 index，写入效率更高 => Log Deduplication

1. BULK_INSERT：写排序，对大数据量的 Hudi 表初始化友好，对文件大小的限制 best effort（写 HFile）

#### 写流程(UPSERT)

##### Copy On Write

- 先对 records 按照 record key 去重
- 首先对这批数据创建索引 (HoodieKey => HoodieRecordLocation)；通过索引区分哪些 records 是 update，哪些 records 是 insert（key 第一次写入）

- 对于 update 消息，会直接找到对应 key 所在的最新 FileSlice 的 base 文件，并做 merge 后写新的 base file (新的 FileSlice)
- 对于 insert 消息，会扫描当前 partition 的所有 SmallFile（小于一定大小的 base file），然后 merge 写新的 FileSlice；如果没有 SmallFile，直接写新的 FileGroup + FileSlice

##### Merge On Read

- 先对 records 按照 record key 去重（可选）
- 首先对这批数据创建索引 (HoodieKey => HoodieRecordLocation)；通过索引区分哪些 records 是 update，哪些 records 是 insert（key 第一次写入）

- 如果是 insert 消息，如果 log file 不可建索引（默认），会尝试 merge 分区内最小的 base file （不包含 log file 的 FileSlice），生成新的 FileSlice；如果没有 base file 就新写一个 FileGroup + FileSlice + base file；如果  log file 可建索引，尝试 append 小的 log file，如果没有就新写一个  FileGroup + FileSlice + base file
- 如果是 update 消息，写对应的 file group + file slice，直接 append 最新的 log file（如果碰巧是当前最小的小文件，会 merge base file，生成新的 file slice）

- log file 大小达到阈值会 roll over 一个新的

#### 写流程(INSERT)

##### Copy On Write

- 先对 records 按照 record key 去重（可选）
- 不会创建 Index

- 如果有小的 base file 文件，merge base file，生成新的 FileSlice + base file，否则直接写新的 FileSlice + base file

##### Merge On Read

- 先对 records 按照 record key 去重（可选）
- 不会创建 Index

- 如果 log file 可索引，并且有小的 FileSlice，尝试追加或写最新的 log file；如果 log file 不可索引，写一个新的 FileSlice + base file

#### 工具

- DeltaStreamer
- Datasource Writer

- Flink SQL API

#### Key 生成策略

用来生成 HoodieKey（record key + partition path），目前支持以下策略：

- 支持多个字段组合 record keys
- 支持多个字段组合的 parition path （可定制时间格式，Hive style path name）

- 非分区表

#### 删除策略

- 逻辑删：将 value 字段全部标记为 null
- 物理删：

1. 通过 OPERATION_OPT_KEY  删除所有的输入记录
2. 配置 PAYLOAD_CLASS_OPT_KEY = org.apache.hudi.EmptyHoodieRecordPayload 删除所有的输入记录

1. 在输入记录添加字段：_hoodie_is_deleted

### 数据读

#### Snapshot 读

读取所有 partiiton 下每个 FileGroup 最新的 FileSlice 中的文件，Copy On Write 表读 parquet 文件，Merge On Read 表读 parquet + log 文件



#### Incremantal 读: 

https://hudi.apache.org/docs/querying_data.html#spark-incr-query，当前的 Spark data source 可以指定消费的起始和结束 commit 时间，读取 commit 增量的数据集。但是内部的实现不够高效：拉取每个 commit 的全部目标文件再按照系统字段 _hoodie_commit_time_  apply 过滤条件。

#### Streaming 读

0.8.0 版本的 HUDI Flink writer 支持实时的增量订阅，可用于同步 CDC 数据，日常的数据同步 ETL pipeline。Flink 的 streaming 读做到了真正的流式读取，source 定期监控新增的改动文件，将读取任务下派给读 task。

### Compaction

- 没有 base file：走 copy on write insert 流程，直接 merge 所有的 log file 并写 base file
- 有 base file：走 copy on write upsert 流程，先读 log file 建 index，再读 base file，最后读 log file 写新的 base file


Flink 和 Spark streaming 的 writer 都可以 apply 异步的 compaction 策略，按照间隔 commits 数或者时间来触发 compaction 任务，在独立的 pipeline 中执行。



### 总结

通过对写流程的梳理我们了解到 Apache Hudi 相对于其他数据湖方案的核心优势：

- 写入过程充分优化了文件存储的小文件问题，Copy On Write 写会一直将一个 bucket （FileGroup）的 base 文件写到设定的阈值大小才会划分新的 bucket；Merge On Read 写在同一个 bucket 中，log file 也是一直 append 直到大小超过设定的阈值 roll over。
- 对 UPDATE 和 DELETE 的支持非常高效，一条 record 的整个生命周期操作都发生在同一个 bucket，不仅减少小文件数量，也提升了数据读取的效率（不必要的 join 和 merge）。


0.8.0 的 HUDI Flink 支持了 streaming 消费 HUDI 表，在后续版本还会支持 watermark 机制，让 HUDI Flink 承担 streaming ETL pipeline 的中间层，成为数据湖/仓建设中流批一体的中间计算层。