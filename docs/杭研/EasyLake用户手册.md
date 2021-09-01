EasyLake 用户手册

# 产品简介

简单介绍产品的功能和亮点

## 产品原理简介

简单介绍arctic的实现原理

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=MjcyNmEzNmY4YWVhMTJjMGM2YWNlMWExN2E0NjliYjlfVlA2dkNHUllDWHpveVBRV0M2S01Ga29Hb3dtdEFhcVlfVG9rZW46Ym94Y25IcWhHelVpa1BWM1ZaRHJKUloyNkloXzE2MzA0NzIwMzU6MTYzMDQ3NTYzNV9WNA)

# 建表流程

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=OGJhNjdiZTc3ZDFjOTcxYWY2ZTEzMWQ3YjJhZGVhZmFfaTU1bkpPZ3JQTlh5RjhMblZaWHNTUDRHWUZQUlNvcXpfVG9rZW46Ym94Y25zTWZwU0I3RWpSOGExdnRpeHcwajhHXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)



通过新建表流程，可以将一张 Hive 表升级为一张 Arctic 表，或者直接新建一张 Arctic 表。在建表过程中，最多可以配置 5 个任务，分别用于将数据导入 Arctic 表，以及将数据在表内进行同步。

## 新建表流程：

入口：实时数据湖列表页右上方

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=NGU1NmFhMGRmMmFhYzFhMDU2NzNiMDM1MDg1NDY4YzRfYnh3WURjclZJc1V2anR3alc4MFU5RUVCUnRpMk1HcWxfVG9rZW46Ym94Y254Y1FqY3VwekVjRVlsSlZVV1hRUXliXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

### 新建表-基本信息

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=YWEyYTU0YTJjYjgwZDkxMGIwMDQ0Y2NlNGVkZTg4YjJfcHJhTVd4a3EwMUQ3dEdQdmQ5QVByNkZ1djJxc0E4Z1lfVG9rZW46Ym94Y25vYUxNdDFwd2lGa1pNYnlxTjQ1eWFjXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

建表方式：新建表第一步可以选择建表方式，包括Hive原地升级和新建表两种。

集群、数据库：集群和库为新建的表存储的HDFS集群和库，默认选择页面左侧选中的集群和库，用户可自行更改。

表名称：如建表方式为Hive原地升级，则此处下拉选择要升级的表的名称；如建表方式为新建表，则此处需要手动输入新建的表的名称。

原生读取：此处开关如开启，则 Arctic 表内容会定时写入 Hive，之后用户可以使用 Hive 的原生 connector 进行读取，而无需使用 Arctic 的 connector 进行读取。如果关闭此项，则 Acrtic 表新增的数据不会写入 Hive 中。

数据刷新方式：当原生读取为开启时有此选项，用户可选择 Arctic 表内容写入到 Hive 的时间区间，或选择在某个固定时刻写入。

数据刷新范围：当原生读取为开启时有此选项，用户可选择将 Arctic 表的全部内容，或最新的任意数量分区的内容写入 Hive。

属性：

### 新建表-字段配置

当建表方式为 Hive 原地升级时，此页面展示原 Hive 表的字段信息，用户可在此设置主键，未设置主键时，不支持 Update 操作，同时不支持配置索引。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=ODRlOTExZjE0OTRlMjg0ZDY3OGNjZTZkYWM5MDVhYmVfUUZCRXBWcGR3SmxDWFdBU21ZRVdsdWJiaGdkQlVpdFlfVG9rZW46Ym94Y255RWw1amlWNlJtdnFzVnZEMEFrVEd2XzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

当建表方式为新建表时，用户需在此页面设置 Arctic 表的字段。字段的配置方式可选择从已有表复制或自定义字段。

**从已有表复制：**

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmFjZDk2YzdlMGYzY2Y4ZWQzY2YwOTI3MDI0ZDMyNTJfcUNidXVKdzVxSjc4NzAwVDB2SW93WFhjMFVtRk1COEJfVG9rZW46Ym94Y25OY2FyaFhBcU94UUhoelpGMTRubnVoXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

数据源类型、数据源、表：通过下拉选择要复制的表的位置和表名，目前仅支持复制 MySQL 表。

字段：列表显示了选中的表的字段信息，用户可以新增、删除、修改或设置主键。

分区字段：如需建分区表，则需要用户手动新增分区字段。

**自定义字段：**

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=MzJiMDU2ZWI2OGJmMTMyMjhkZmNmNTg5ZGJhZDE1MTNfWkJRQ1ZlSVIwc0tLVzljRTlYclpuV0lYbnlPemVndzVfVG9rZW46Ym94Y25Yc0tsZnZoUjRnZjhocldobmFXek5nXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

字段：用户手动输入新表的字段信息，并可设置某个字段为主键。

分区字段：如需建分区表，则需要用户手动新增分区字段。

### 新建表-数据入湖

新建表的第三步是数据入湖配置，可以配置需要写入到新建的 Arctic 表的全量数据和实时数据的数据源、映射关系、分区字段。如在新建表过程中未对数据入湖任务进行配置，还可以在表-运行信息-数据入湖页面进行入湖任务的创建。

#### 全量数据入湖

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=MWQwYjVkYjk3M2Q4Y2VmZmExYmIwNmNmYWU5NTQzMWNfbTVjRXRuYWlTSGgwMm9ZVkFnWGlKcFJ2ejUyTWJaYlpfVG9rZW46Ym94Y25CSjBNYTZ6Rm5GWE0ySzhzRXEyZjBmXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

打开全量数据入湖的开关，即可配置全量入湖任务。（建表流程图中任务a）

**源端选择**

源端数据源类型：选择要入湖的全量数据的来源类型，目前仅支持 MySQL。

源端数据源、库、表：选择要入湖的数据的存储位置，目前仅支持选择单张表。

并发度：设置入湖任务的并发度，默认值为1。

**字段映射**

目标端字段即为在第二步字段配置中配置的新建的表的字段。源端表字段与目标表字段通过字段名匹配，字段名完全相同的字段即可自动映射，未找到与目标端字段同名的则源端字段默认选中不导入，用户可以通过下拉选择要映射的字段，也可以使用自定义表达式自定义要写入的字段内容，自定义表达式需使用 SQL 语法。

**分区字段**

分区字段即为在第二步字段配置中配置的新建的表的分区字段。用户需要在分区字段的列表中为入湖数据定义分区表达式。

#### 实时数据入湖

打开全量数据入湖的开关，即可配置全量入湖任务。（建表流程图中任务b）

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=MWJhNDBhNjFmMDcxNjVmMTY3OGI1ODlkYzEyZmE4YTlfNHhKMGM3SzVtMUF0bFJ4Z1pTWXdNNzNST2RQem9aRjRfVG9rZW46Ym94Y25kQnk3S1lkcFpVTll3VHVhMUdqWFpiXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

**源端选择**

源端数据源类型：选择要入湖的实时数据的来源类型，目前仅支持 Kafka。

Kafka 集群、Topic：选择要入湖的 Kafka 的集群和 Topic。

起始消费位点：选择开始消费的 Kafka 位点，可选择最新，最早和指定时间戳。

序列化方式：选择 Kafka 消息的序列化方式，目前仅支持 canal-json。

并发度：设置入湖任务的并发度，默认值为1。

**字段映射&分区字段**

设置方式与全量入湖任务相同。

### 新建表-高级配置

在高级配置中，用户可以进行 Arctic 表 CDC 配置和索引配置。

#### CDC 配置

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=NGU0Njg1M2ZlMzVlMmNiZGI0YjZiYWVjMzc1N2I1NDlfQ200MURvMXFnMHl2N3ZoNE1jbkdnMW5hWE1VRWFRcnlfVG9rZW46Ym94Y25IMFhEMFBlZUQ1Y3pUR1RHRGdERndoXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

打开 CDC 配置的开关，即可开始配置 Arctic 表 CDC 数据要写入的 Kafka 集群和 Topic 信息。

Kafka 集群：选择 CDC 数据要写的集群。

Broker 地址：展示被选中的集群的地址，以便下游消费时使用。

Topic：默认填入 Arctic 表的集群.db.表名，用户可以自行修改。

副本数、分区数：设置要写入 Kafka Topic 的副本数和分区数。

打开 CDC 配置的开关后，会默认创建一个表内同步（Kafka → Hive）任务（建表流程图中任务c）。任务创建后默认为未启动状态，如有需要，在表-运行信息中的表内同步 tab 页可以对此任务进行启停。

#### 索引配置

注意：仅当 Arctic 表有主键时才可以进行索引配置。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=YzlkNjMxOTFlZTZjYmNiYTZlYjYzNDU5YjRhNjU4YWZfQ1RxR21INlRnRG1lSWY0eTdFSG52QWxSY3FuNnZ4ZWxfVG9rZW46Ym94Y25qcU9uckJXc0hQUVpqTEJUVUZudXhoXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

打开索引配置开关，即可进行索引配置。仅当 Arctic 表有主键时此开关才可打开。

索引字段：默认填入 Arctic 表主键，不可修改。

HBase 数据源：选择索引要写入的 HBase 集群。

地址：展示被选中的集群的地址，以便下游消费时使用。

表名：输入索引要写入的表名，如此表在 HBase 中不存在，则会自动新建此表。

数据导入：选择要写入 HBase 表内的数据类型，默认选中全量，可多选，不可不选。

选择全量时，会新建一个表内同步（Hive → HBase）任务（建表流程图中任务d）。任务创建后默认为未启动状态，如有需要，在表-运行信息中的表内同步 tab 页可以对此任务进行启停。

选择实时时，会新建一个表内同步（Kafka → HBase）任务（建表流程图中任务e）。任务创建后默认为未启动状态，如有需要，在表-运行信息中的表内同步 tab 页可以对此任务进行启停。

# 已有表管理

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjNiMzVjYTc1YjJiYWNhODc5MDZjNjc0NzA4NGU0N2Zfdk5NTnVBWE5JdVRzbnBZbzB2bmJRa0h0YVJZTVduYVpfVG9rZW46Ym94Y25hMnVERXF0R3JySWp1Sk9CYXpQZE5mXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

实时数据湖的首页左侧展示了项目内的 Hive 集群和数据库，选择想查看的集群和库之后，页面右侧展示在该数据库下的 Arctic 表列表。用户可对列表进行按负责人筛选或按表名搜索。

点击列表中的表名，会打开该表的详情页（数据地图中的表详情页面），用户可以查看表的基本信息、字段信息、血缘信息等表相关信息。

点击列表的操作栏中的运行信息，即可查看该表关联的所有任务的运行情况，包括表内同步任务、数据合并任务、数据入湖任务。

点击列表的操作栏中的配置信息，即可查看该表在建表时的配置信息，包括表的基本配置、字段配置、高级配置。

## 运行信息

### 基本信息

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=NTMzODUwOTViNGNhYzczYmVmOTliMWQ1NGU5MDExYWJfdjhFS09sc3hPTXZESWExbTByVFNGRU1OYWFtOVpVNkRfVG9rZW46Ym94Y25kUFJINVV0QURvMkgzUDVpVDFGZU1oXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

在运行信息-基本信息页面，展示了 Arctic 表中的 Change 表和 Base 表的数据量、文件个数、快照情况等基本信息。用户可以通过这些基本信息来判断表的情况是否正常。

### 表内同步

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=MTNiYzYzOTJiMWJjNDI2MzhmYTE1MzY5N2E4NmJkODlfSlpUa0ZHMG5ZRUNiYVZKQWdNN25xb1lKdE9xZDB5NzZfVG9rZW46Ym94Y25IVUViQjVzaTFIODVJTm1HaU12NG5iXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

在新建表-高级配置中进行的 CDC 配置和索引配置相关的任务可以在表内同步页面查看。在新建表过程中创建的任务默认为未启动状态，需要用户在此页面中手动启动。全量同步任务在完成同步后会自动停止，实时同步任务需用户在需要时手动停止。

点击列表的操作栏中的详情，可以跳转至 Flink UI 页面，查看该 Flink 任务的运行情况。

点击列表的操作栏中的配置信息，可以查看该任务的配置，并可以对配置项进行修改。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjA1NWY0NWUzYWU3YTYwNWI2NjgyNzcyMTFkNGY1YmNfcnlUWWVoQWFxQUNIRFhFcDVqWUVPclRpcEE2NmhZWWlfVG9rZW46Ym94Y25NenVqVkxPOGpOQ2NXbDl4M1RWYkRlXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTFlZjVmM2I0YTdkN2E0ZmRiYjEwNzgwN2FjNDc1NWRfRk03MFRLNjhFRmtVWG50N2JLSFNlZ1RsWWhDYlhUNVFfVG9rZW46Ym94Y25DeWc0RmZtMXNhYjJQQ0lRVEFkUlRoXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

全量任务的配置项包括批量数、拉取并发度、插入并发度。实时任务的配置项包括并发数。

### 数据合并

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWIxMDA5NWRkMmM3NmM3NTg0N2EwYzEwNjVjODI4NmVfN1lOTzdKaWx2VlcwdWxzWHBBaFR3Y2ZESGdZQzl5d2ZfVG9rZW46Ym94Y25jZjNhcnNVNFh0TW9pTkFTYmJzOWhCXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)



### 数据入湖

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=YjY1MDEyMTE4ZDYyMGFkOGRkYzI2NGRjM2UwYmRjMzFfQ2Z3akhocWNzZEJ5TTdDNzBDRjB4ZkFPQXZIZ0JBM1FfVG9rZW46Ym94Y25lS245ODlvbHBUZGNHNnRrTG9CSlpLXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

在新建表-数据入湖步骤或在此页面内创建的数据入湖任务显示在此页列表中。任务创建后为未启动状态，需要用户手动启停。为保证数据一致性，目前仅支持同时运行一个入湖任务。

在任务为停止或失败状态下，点击操作栏的配置信息，可对入湖任务的配置进行更改，重启任务后变更生效。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=NjAwNDUyMmYxZGJlODIyMjk0NzU2ZGU5ZGUzMmRkMDFfQ1pvVEN0eTF2SUlJRVVzRm1uUkV2M1k1Z0YwRGcwTk5fVG9rZW46Ym94Y25PbU1KSVUwYWswcXlJN3BWQ25uQVhjXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

## 配置信息

### 基本信息

展示建表时选择的集群、数据库、原生读取等信息。其中原生读取相关配置可以随时更改，其他信息不支持修改。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=MTdhOGJmOThkMGYwYjRkNTMxYjUzMjQ1Y2U2YjRjOWZfbWppYmx0OFVPYnhaMHUyZmtjR0FFdWwyY2x2UDJBTHBfVG9rZW46Ym94Y244NlhEUUxNWGNwRTJaemNKbUZISzFRXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

### 字段配置

展示 Arctic 表的字段列表、分区字段列表和主键。目前暂不支持在建表后对字段进行修改。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=OWE4ZjkxOGNlZjQ4Y2U4MmE4ZjVmZjNkMDE5OGNkNTJfR2FES2cwSjNUZGJrcDFpVDltcXI0TUg5MTlhM1JxVXRfVG9rZW46Ym94Y25mN3VvWTVZcmxsWmZzSVNGSHVKVkU2XzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

### 高级配置

展示建表时的 CDC 配置和索引配置。目前暂不支持在建表后对高级配置进行修改。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDM1Njk0Mjg4NmY0YzlkMDU3ZmQ4MzQ5M2NhMDk4NGFfaDhTTFBZV3k3N2thbU9SelRSQUgySm9wcGtwNnVDa01fVG9rZW46Ym94Y25MbUhDQVBDbkV3dHlrTEx0V1VMZHJnXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

# 索引关联（维表Join ）

本节将介绍索引相关功能的基本说明、实现原理以及使用方法。

## 功能简介

Arctic 表可以作为维表，对外提供关联（Join）实时数据的能力。

在hsit

## 实现原理

Arctic 底层维护了一份数据专门用于支撑快速高效的关联操作，这份数据会加载到内存或者第三方存储中，当前已支持 HBase。后台将 Arctic 表的主键字段作为 HBase 的 RowKey 字段，因此，若要使用 Arctic 维表 Join 的功能，必须是有主键表，且 Join 的条件字段必须等于主键字段（不限制多个主键字段的 join 顺序）。

下图为数据流向的简单示意图。 

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=NmVjMTRjNzg4ZDQzMjI4ZTM1YjRhZDE4ODkzZDVhNGVfWm14cDlDdXh3UFhzaEhTMk1ra1gyM0RyVHFnVnNka1VfVG9rZW46Ym94Y25JWnp5R0JhdFJSNFBqMHB1RFdDcDRhXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)



## 使用说明

### 使用限制

仅支持 HBase 作为索引存储，仅支持 Arctic 表主键字段作为索引条件

### 使用步骤

#### 主键设置

Arctic 索引功能必须声明主键，可选择多个字段组成，例如：

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTcxYWNlNTM5NjdhNmUyZDkxNzJlNDMxYjU3YjZkNGRfbFR0cTV1bHk2Vk1vSG9WTUZmQTN5VEc3Z2VHTXNBWG1fVG9rZW46Ym94Y25pSWR2RHZwcGphQVRZaHZENjVQamhlXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

#### 索引配置

开启索引配置，选择 HBase 集群，输入存储在 HBase 中对应的表名，该表名在平台使用过程中不常用，仅方便后期问题排查。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=NTYzMjY0MGU5NzE2MmU4Y2FkNTUxOTYxNjAxZjEzMDNfMksxWnNGd2JZSmxRMFFtelVpaGFRT2lGVXdSWUVQYU9fVG9rZW46Ym94Y25PV1plR3h5VkxwNmFqeTdWM2JTTndjXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

##### 索引同步方式一：入湖

数据导入功能是在 Arctic 表内部将数据从 Kafka 或 HDFS 上导入 HBase 的功能，该功能与表内同步功能配合使用（数据流向示意图黄色虚线部分），若用户直接通过平台 SQL 作业写入 HBase，则不需要使用导入功能。详见表内同步章节。

‘全量’ 数据导入以任务的形式将 HDFS 离线数据导入到 HBase ；

‘实时’ 数据导入以任务的形式将 Kafka 实时数据导入到 HBase 。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=MGJkYjVhZjdmZjEwYTdmYjgyNmFmZTdhM2EyODFlNGVfdDVib05WdEt0TGpBR2o0QVdtaElYZmswQWpqUFpNUmpfVG9rZW46Ym94Y25wRGNKdThMdEpaQ1RndUhjeHJZNThlXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

##### 索引同步方式二：SQL 作业

指定 Dynamic Table Options 参数 /*+ OPTIONS('arctic_emit_mode'='hbase') */  即可实现往 Arctic HBase 端中写入数据。

![img](https://as9e2kjei0.feishu.cn/space/api/box/stream/download/asynccode/?code=NWRjMzgwYjAzYTFhZGNkNWRhOWYwODE1OGJiNWFkNzZfMjl1dFI5OUg0eDJpUlNROTQ4RWVkczBKZmwzaXRGeFpfVG9rZW46Ym94Y251VkRBUnRpVkFGTHdKdGdWaGphMkFkXzE2MzA0NzE5OTM6MTYzMDQ3NTU5M19WNA)

#### 关联索引

在 Flink SQL 作业中可直接对 Arctic 进行 Join 关联数据，其中 Join 的字段必须与 Arctic 声明的主键字段相同，多个主键字段顺序不限制。

```
insert into `memory_catalog`.`default`.arctic_result_table 

select

 src.id as id,

 dim.`name` as `name`,

 dim.dt as dt

from

 `memory_catalog`.`default`.`source_table` as src

 left join `bdmstest-hive-catalog`.ndc_test_db.animal_dimension /*+ OPTIONS('index.read.properties.cache.strategy'='LRU','index.read.properties.cache.size'='108000') */

 -- 关联维度表中的数据，必须加上FOR SYSTEM_TIME AS OF ${souce_table_event_time_column}，表示JOIN维表当前时刻所看到的每条数据

 FOR SYSTEM_TIME AS OF src.join_time as dim 

 ON src.`name` = dim.`name` and src.id = dim.id;
```

通过以上的 SQL 可以从 Arctic 表中读取到维表的数据，实时数据来自于与表绑定的 HBase，可以通过 Flink SQL Hint 语法在 SQL 中注入读取表时的参数，支持的参数包括：

- index.read.properties.cache.strategy：读取HBase时的缓存策略，默认为None不开启，可选值为LRU

- index.read.properties.cache.size：当选择了LRU缓存策略，可设置缓存数量，默认为10000

- index.read.properties.cache.ttl：当选择了LRU缓存策略，可设置缓存过期时间，默认3小时，单位：秒