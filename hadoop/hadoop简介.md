# Hadoop 简介

# 1. 起源
Apache Hadoop是一款支持数据密集型分布式应用并以Apache 2.0许可协议发布的开源软件框架。

Hadoop是Google群集系统的一个开源实现。
- Google论文：GFS，MapReduce，BigTable
- Hadoop实现：HDFS，MapReduce，HBase

Hadoop是Apache Lucene创始人Doug Cutting创建的，原本是Lucene的子项目Nutch（其旨在解决海量数据爬虫和存储）的一部分。2008年Hadoop成为Apache的顶级项目。

起名：Doug Cutting儿子的黄色大象玩具的名字。

# 2. Hadoop生态

![](/assets/hadoop1.jpg)
 
- Hadoop Common：一组分布式文件系统和通用I/O的组件与接口（序列化、Java RPC和持久化数据结构）。在0.20及以前的版本中，包含HDFS、MapReduce和其他项目公共内容，从0.21开始HDFS和MapReduce被分离为独立的子项目，其余内容为Hadoop Common
- HDFS：Hadoop分布式文件系统（Distributed File System）－HDFS（Hadoop Distributed File System），运行与大型商用机集群。
- MapReduce：并行计算框架，0.20前使用org.apache.hadoop.mapred旧接口，0.20版本开始引入org.apache.hadoop.mapreduce的新API
- Apache HBase：分布式NoSQL列数据库，类似谷歌公司BigTable。
- Apache Hive：构建于hadoop之上的数据仓库，通过一种类SQL语言HiveQL为用户提供数据的归纳、查询和分析等功能。Hive最初由Facebook贡献。
- Apache Mahout：机器学习算法软件包。
- Apache Sqoop：结构化数据（如关系数据库）与Apache Hadoop之间的数据转换工具。
- Apache ZooKeeper：分布式锁设施，提供类似Google Chubby的功能，由Facebook贡献。
- Apache Avro：一种支持高校、跨语言的RPC以及永久存储数据的序列化系统。
新的数据序列化格式与传输工具，将逐步取代Hadoop原有的IPC机制。
- Flume：一个分布式、可靠、和高可用的海量日志聚合系统，支持在系统中定制各类数据发送方，用于收集数据。

应用场景：
数据仓库，离线数据分析，大规模在线实时应用。

# 3. Hadoop的版本

主要分为两代：第一代和第二代
第一代：0.20.x
第二代：0.23.x和2.x
