# BigData-Notes



<div align="center"> <img width="444px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/bigdata-notes-icon.png"/> </div>
<br/>

**大数据入门指南**



<table>
    <tr>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/hadoop.jpg"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/hive.jpg"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/spark.jpg"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/storm.png"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/flink.png"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/hbase.png"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/kafka.png"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/zookeeper.jpg"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/flume.png"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/sqoop.png"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/azkaban.png"></th>
      <th><img width="50px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/scala.jpg"></th>
    </tr>
    <tr>
      <td align="center"><a href="#一hadoop">Hadoop</a></td>
      <td align="center"><a href="#二hive">Hive</a></td>
      <td align="center"><a href="#三spark">Spark</a></td>
      <td align="center"><a href="#四storm">Storm</a></td>
      <td align="center"><a href="#五flink">Flink</a></td>
      <td align="center"><a href="#六hbase">HBase</a></td>
      <td align="center"><a href="#七kafka">Kafka</a></td>
      <td align="center"><a href="#八zookeeper">Zookeeper</a></td>
      <td align="center"><a href="#九flume">Flume</a></td>
      <td align="center"><a href="#十sqoop">Sqoop</a></td>
      <td align="center"><a href="#十一azkaban">Azkaban</a></td>
      <td align="center"><a href="#十二scala">Scala</a></td>
    </tr>
  </table>
<br/>

<div align="center">
	<a href = "https://github.com/heibaiying/Full-Stack-Notes"> 
	<img width="150px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/weixin.jpg"/> 
	</a> 
</div>
<div align="center"> <strong> 如果需要离线阅读，可以在公众号上发送 “bigdata” 获取《大数据入门指南》离线阅读版！ </strong> </div>

<br/>

## 前  言

1. [大数据学习路线](BigData-Notes/大数据学习路线.md)
2. [大数据技术栈思维导图](BigData-Notes/大数据技术栈思维导图.md)        
3. [大数据常用软件安装指南](BigData-Notes/大数据常用软件安装指南.md)

## 一、Hadoop

1. [分布式文件存储系统 —— HDFS](BigData-Notes/Hadoop-HDFS.md)
2. [分布式计算框架 —— MapReduce](BigData-Notes/Hadoop-MapReduce.md)
3. [集群资源管理器 —— YARN](BigData-Notes/Hadoop-YARN.md)
4. [Hadoop 单机伪集群环境搭建](BigData-Notes/installation/Hadoop单机环境搭建.md)
5. [Hadoop 集群环境搭建](BigData-Notes/installation/Hadoop集群环境搭建.md)
6. [HDFS 常用 Shell 命令](BigData-Notes/HDFS常用Shell命令.md)
7. [HDFS Java API 的使用](BigData-Notes/HDFS-Java-API.md)
8. [基于 Zookeeper 搭建 Hadoop 高可用集群](BigData-Notes/installation/基于Zookeeper搭建Hadoop高可用集群.md)

## 二、Hive

1. [Hive 简介及核心概念](BigData-Notes/Hive简介及核心概念.md)
2. [Linux 环境下 Hive 的安装部署](BigData-Notes/installation/Linux环境下Hive的安装部署.md)
3. [Hive CLI 和 Beeline 命令行的基本使用](BigData-Notes/HiveCLI和Beeline命令行的基本使用.md)
4. [Hive 常用 DDL 操作](BigData-Notes/Hive常用DDL操作.md)
5. [Hive 分区表和分桶表](BigData-Notes/Hive分区表和分桶表.md)
6. [Hive 视图和索引](BigData-Notes/Hive视图和索引.md)
7. [Hive 常用 DML 操作](BigData-Notes/Hive常用DML操作.md)
8. [Hive 数据查询详解](BigData-Notes/Hive数据查询详解.md)

## 三、Spark

**Spark Core :**

1. [Spark 简介](BigData-Notes/Spark简介.md)
2. [Spark 开发环境搭建](BigData-Notes/installation/Spark开发环境搭建.md)
3. [弹性式数据集 RDD](BigData-Notes/Spark_RDD.md)
4. [RDD 常用算子详解](BigData-Notes/Spark_Transformation和Action算子.md)
5. [Spark 运行模式与作业提交](BigData-Notes/Spark部署模式与作业提交.md)
6. [Spark 累加器与广播变量](BigData-Notes/Spark累加器与广播变量.md)
7. [基于 Zookeeper 搭建 Spark 高可用集群](BigData-Notes/installation/Spark集群环境搭建.md)

**Spark SQL :**

1. [DateFrame 和 DataSet ](BigData-Notes/SparkSQL_Dataset和DataFrame简介.md)
2. [Structured API 的基本使用](BigData-Notes/Spark_Structured_API的基本使用.md)
3. [Spark SQL 外部数据源](BigData-Notes/SparkSQL外部数据源.md)
4. [Spark SQL 常用聚合函数](BigData-Notes/SparkSQL常用聚合函数.md)
5. [Spark SQL JOIN 操作](BigData-Notes/SparkSQL联结操作.md)

**Spark Streaming ：**

1. [Spark Streaming 简介](BigData-Notes/Spark_Streaming与流处理.md)
2. [Spark Streaming 基本操作](BigData-Notes/Spark_Streaming基本操作.md)
3. [Spark Streaming 整合 Flume](BigData-Notes/Spark_Streaming整合Flume.md)
4. [Spark Streaming 整合 Kafka](BigData-Notes/Spark_Streaming整合Kafka.md)

## 四、Storm

1. [Storm 和流处理简介](BigData-Notes/Storm和流处理简介.md)
2. [Storm 核心概念详解](BigData-Notes/Storm核心概念详解.md)
3. [Storm 单机环境搭建](BigData-Notes/installation/Storm单机环境搭建.md)
4. [Storm 集群环境搭建](BigData-Notes/installation/Storm集群环境搭建.md)
5. [Storm 编程模型详解](BigData-Notes/Storm编程模型详解.md)
6. [Storm 项目三种打包方式对比分析](BigData-Notes/Storm三种打包方式对比分析.md)
7. [Storm 集成 Redis 详解](BigData-Notes/Storm集成Redis详解.md)
8. [Storm 集成 HDFS/HBase](BigData-Notes/Storm集成HBase和HDFS.md)
9. [Storm 集成 Kafka](BigData-Notes/Storm集成Kakfa.md)

## 五、Flink

1. [Flink 核心概念综述](BigData-Notes/Flink核心概念综述.md)
2. [Flink 开发环境搭建](BigData-Notes/Flink开发环境搭建.md)
3. [Flink Data Source](BigData-Notes/Flink_Data_Source.md)
4. [Flink Data Transformation](BigData-Notes/Flink_Data_Transformation.md)
5. [Flink Data Sink](BigData-Notes/Flink_Data_Sink.md)
6. [Flink 窗口模型](BigData-Notes/Flink_Windows.md)
7. [Flink 状态管理与检查点机制](BigData-Notes/Flink状态管理与检查点机制.md)
8. [Flink Standalone 集群部署](BigData-Notes/installation/Flink_Standalone_Cluster.md)


## 六、HBase

1. [Hbase 简介](BigData-Notes/Hbase简介.md)
2. [HBase 系统架构及数据结构](BigData-Notes/Hbase系统架构及数据结构.md)
3. [HBase 基本环境搭建 (Standalone /pseudo-distributed mode)](BigData-Notes/installation/HBase单机环境搭建.md)
4. [HBase 集群环境搭建](BigData-Notes/installation/HBase集群环境搭建.md)
5. [HBase 常用 Shell 命令](BigData-Notes/Hbase_Shell.md)
6. [HBase Java API](BigData-Notes/Hbase_Java_API.md)
7. [HBase 过滤器详解](BigData-Notes/Hbase过滤器详解.md)
8. [HBase 协处理器详解](BigData-Notes/Hbase协处理器详解.md)
9. [HBase 容灾与备份](BigData-Notes/Hbase容灾与备份.md)
10. [HBase的 SQL 中间层 —— Phoenix](BigData-Notes/Hbase的SQL中间层_Phoenix.md)
11. [Spring/Spring Boot 整合 Mybatis + Phoenix](BigData-Notes/Spring+Mybtais+Phoenix整合.md)

## 七、Kafka

1. [Kafka 简介](BigData-Notes/Kafka简介.md)
2. [基于 Zookeeper 搭建 Kafka 高可用集群](BigData-Notes/installation/基于Zookeeper搭建Kafka高可用集群.md)
3. [Kafka 生产者详解](BigData-Notes/Kafka生产者详解.md)
4. [Kafka 消费者详解](BigData-Notes/Kafka消费者详解.md)
5. [深入理解 Kafka 副本机制](BigData-Notes/Kafka深入理解分区副本机制.md)

## 八、Zookeeper

1. [Zookeeper 简介及核心概念](BigData-Notes/Zookeeper简介及核心概念.md)
2. [Zookeeper 单机环境和集群环境搭建](BigData-Notes/installation/Zookeeper单机环境和集群环境搭建.md) 
3. [Zookeeper 常用 Shell 命令](BigData-Notes/Zookeeper常用Shell命令.md)
4. [Zookeeper Java 客户端 —— Apache Curator](BigData-Notes/Zookeeper_Java客户端Curator.md)
5. [Zookeeper  ACL 权限控制](BigData-Notes/Zookeeper_ACL权限控制.md)

## 九、Flume

1. [Flume 简介及基本使用](BigData-Notes/Flume简介及基本使用.md)
2. [Linux 环境下 Flume 的安装部署](BigData-Notes/installation/Linux下Flume的安装.md)
3. [Flume 整合 Kafka](BigData-Notes/Flume整合Kafka.md)

## 十、Sqoop

1. [Sqoop 简介与安装](BigData-Notes/Sqoop简介与安装.md)
2. [Sqoop 的基本使用](BigData-Notes/Sqoop基本使用.md)

## 十一、Azkaban

1. [Azkaban 简介](BigData-Notes/Azkaban简介.md)
2. [Azkaban3.x 编译及部署](BigData-Notes/installation/Azkaban_3.x_编译及部署.md)
3. [Azkaban Flow 1.0 的使用](BigData-Notes/Azkaban_Flow_1.0_的使用.md)
4. [Azkaban Flow 2.0 的使用](BigData-Notes/Azkaban_Flow_2.0_的使用.md)

## 十二、Scala

1. [Scala 简介及开发环境配置](BigData-Notes/Scala简介及开发环境配置.md)
2. [基本数据类型和运算符](BigData-Notes/Scala基本数据类型和运算符.md)
3. [流程控制语句](BigData-Notes/Scala流程控制语句.md)
4. [数组 —— Array](BigData-Notes/Scala数组.md)
5. [集合类型综述](BigData-Notes/Scala集合类型.md)
6. [常用集合类型之 —— List & Set](BigData-Notes/Scala列表和集.md)
7. [常用集合类型之 —— Map & Tuple](BigData-Notes/Scala映射和元组.md)
8. [类和对象](BigData-Notes/Scala类和对象.md)
9. [继承和特质](BigData-Notes/Scala继承和特质.md)
10. [函数 & 闭包 & 柯里化](BigData-Notes/Scala函数和闭包.md)
11. [模式匹配](BigData-Notes/Scala模式匹配.md)
12. [类型参数](BigData-Notes/Scala类型参数.md)
13. [隐式转换和隐式参数](BigData-Notes/Scala隐式转换和隐式参数.md)

## 十三、公共内容

1. [大数据应用常用打包方式](BigData-Notes/大数据应用常用打包方式.md)

<br>

## :bookmark_tabs: 后  记

[资料分享与开发工具推荐](notes/资料分享与工具推荐.md)

<br>

<div align="center">
	<a href = "https://blog.csdn.net/m0_37809146"> 
	<img width="200px" src="https://gitee.com/heibaiying/BigData-Notes/raw/master/pictures/blog-logo.png"/> 
	</a> 
</div>
<div align="center"> <a  href = "https://blog.csdn.net/m0_37809146"> 欢迎关注我的博客：https://blog.csdn.net/m0_37809146</a> </div>
