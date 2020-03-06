# Hadoop简介

## Hadoop 生态

## 1. 关于Hadoop的起源

Apache Hadoop是一款支持数据密集型分布式应用并以Apache 2.0许可协议发布的开源软件框架。

Hadoop是Google群集系统的一个开源实现。

* Google论文：GFS，MapReduce，BigTable
* Hadoop实现：HDFS，MapReduce，HBase

Hadoop是Apache Lucene创始人Doug Cutting创建的，原本是Lucene的子项目Nutch（其旨在解决海量数据爬虫和存储）的一部分。2008年Hadoop成为Apache的顶级项目。

起名：Doug Cutting儿子的黄色大象玩具的名字。

## 2. Hadoop简介

![](../../.gitbook/assets/hadoop1%20%281%29.jpg)

* **Hadoop Common**：一组分布式文件系统和通用I/O的组件与接口（序列化、Java RPC和持久化数据结构）。在0.20及以前的版本中，包含HDFS、MapReduce和其他项目公共内容，从0.21开始HDFS和MapReduce被分离为独立的子项目，其余内容为Hadoop Common
* **HDFS**：Hadoop分布式文件系统（Distributed File System）－HDFS（Hadoop Distributed File System），运行与大型商用机集群。
* **MapReduce**：并行计算框架，0.20前使用org.apache.hadoop.mapred旧接口，0.20版本开始引入org.apache.hadoop.mapreduce的新API
* **Apache HBase**：分布式NoSQL列数据库，类似谷歌公司BigTable。
* **Apache Hive**：构建于hadoop之上的数据仓库，通过一种类SQL语言HiveQL为用户提供数据的归纳、查询和分析等功能。Hive最初由Facebook贡献。
* **Apache Mahout**：机器学习算法软件包。
* **Apache Sqoop**：结构化数据（如关系数据库）与Apache Hadoop之间的数据转换工具。
* **Apache ZooKeeper**：分布式锁设施，提供类似Google Chubby的功能，由Facebook贡献。
* **Apache Avro**：一种支持高校、跨语言的RPC以及永久存储数据的序列化系统。

  新的数据序列化格式与传输工具，将逐步取代Hadoop原有的IPC机制。

* **Flume**：一个分布式、可靠、和高可用的海量日志聚合系统，支持在系统中定制各类数据发送方，用于收集数据。

## 3. Hadoop生态

![](../../.gitbook/assets/hadoop-sheng-tai.png)

* Apache Cassandra:是一套开源分布式NoSQL数据库系统。它最初由Facebook开发，用于储存简单格式数据，集Google BigTable的数据模型与Amazon Dynamo的完全分布式的架构于一身
* Apache Avro: 是一个数据序列化系统，设计用于支持数据密集型，大批量数据交换的应用。Avro是新的数据序列化格式与传输工具，将逐步取代Hadoop原有的IPC机制
* Apache Ambari: 是一种基于Web的工具，支持Hadoop集群的供应、管理和监控。
* Apache Chukwa: 是一个开源的用于监控大型分布式系统的数据收集系统，它可以将各种各样类型的数据收集成适合 Hadoop 处理的文件保存在 HDFS 中供 Hadoop 进行各种 MapReduce 操作。
* Apache Hama: 是一个基于HDFS的BSP（Bulk Synchronous Parallel\)并行计算框架, Hama可用于包括图、矩阵和网络算法在内的大规模、大数据计算。
* Apache Flume: 是一个分布的、可靠的、高可用的海量日志聚合的系统，可用于日志数据收集，日志数据处理，日志数据传输。
* Apache Giraph: 是一个可伸缩的分布式迭代图处理系统， 基于Hadoop平台，灵感来自 BSP \(bulk synchronous parallel\) 和 Google 的 Pregel。
* Apache Oozie: 是一个工作流引擎服务器, 用于管理和协调运行在Hadoop平台上（HDFS、Pig和MapReduce）的任务。
* Apache Crunch: 是基于Google的FlumeJava库编写的Java库，用于创建MapReduce程序。与Hive，Pig类似，Crunch提供了用于实现如连接数据、执行聚合和排序记录等常见任务的模式库
* Apache Whirr: 是一套运行于云服务的类库（包括Hadoop），可提供高度的互补性。Whirr学支持Amazon EC2和Rackspace的服务。
* Apache Bigtop: 是一个对Hadoop及其周边生态进行打包，分发和测试的工具。
* Apache HCatalog: 是基于Hadoop的数据表和存储管理，实现中央的元数据和模式管理，跨越Hadoop和RDBMS，利用Pig和Hive提供关系视图。
* Cloudera Hue: 是一个基于WEB的监控和管理系统，实现对HDFS，MapReduce/YARN, HBase, Hive, Pig的web化操作和管理。

## 3. Hadoop的版本

* 第一代：0.20.x
* 第二代：0.23.x和2.x
* 第三代：3.x

