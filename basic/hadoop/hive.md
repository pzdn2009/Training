# Hive

## Hive

## 1. 简介

Hive是Facebook构建在Hadoop上的数据仓库框架。

### 1.1 数据仓库框架

* Hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并使用sql语句转换为MapReduce任务进行运行。其优点是学习成本低，可以通过类SQL语句快速实现简单的MapReduce统计，不必开发专门的MapReduce应用，十分适合数据仓库的统计分析；
* Hive是建立在 Hadoop 上的数据仓库基础构架。它提供了一系列的工具，可以用来进行数据提取转化加载（ETL），这是一种可以存储、查询和分析存储在 Hadoop 中的大规模数据的机制。Hive 定义了简单的类 SQL 查询语言，称为 HQL，它允许熟悉 SQL 的用户查询数据。同时，这个语言也允许熟悉 MapReduce 开发者的开发自定义的 Mapper 和 Reducer 来处理内建的Mapper 和Reducer 无法完成的复杂的分析工作。

### 1.2 Hive与关系数据库的区别

1. Hive和关系数据库存储文件的系统不同，Hive使用的是Hadoop的HDFS（Hadoop的分布式文件系统），关系数据库则是服务器本地的文件系统；
2. Hive使用的计算模型是MapReduce，而关系数据库则是自身的计算模型；
3. 关系数据库都是为实时查询的业务进行设计的，而Hive则是为海量数据做数据挖掘设计的，实时性很差；实时性的区别导致Hive的应用场景和关系数据库有很大的不同；
4. Hive很容易扩展自己的存储能力和计算能力，这个是继承Hadoop的，而关系数据库在这个方面要比数据库差很多。

### 1.3 安装

直接从官方下载一个发布包，

```text
tar xzf hive-x.y.z-dev.tar.gz
export HIVE_HOME=/home/pzdn/hive-x.y.z-dev
export PATH=$PATH:$HIVE_HOME/bin
```

hive命令

```text
hive>SHOW TABLES;
```

## 2. Hive架构

![](../../.gitbook/assets/hd6%20%281%29.png)

Hadoop的mapreduce是Hive架构的根基。

服务端组件：

* Driver组件：该组件包括Complier、Optimizer和Executor，它的作用是将HiveQL（类SQL）语句进行解析、编译优化，生成执行计划，然后调用底层的mapreduce计算框架；
* Metastore组件：元数据服务组件，这个组件存储Hive的元数据，Hive的元数据存储在关系数据库里，Hive支持的关系数据库有derby和mysql。元数据对于Hive十分重要，因此Hive支持把metastore服务独立出来，安装到远程的服务器集群里，从而解耦Hive服务和metastore服务，保证Hive运行的健壮性；
* Thrift服务：thrift是facebook开发的一个软件框架，它用来进行可扩展且跨语言的服务的开发，Hive集成了该服务，能让不同的编程语言调用hive的接口。

客户端组件：

* CLI：command line interface，命令行接口。
* Thrift客户端：上面的架构图里没有写上Thrift客户端，但是Hive架构的许多客户端接口是建立在thrift客户端之上，包括JDBC和ODBC接口。
* WEBGUI：Hive客户端提供了一种通过网页的方式访问hive所提供的服务。这个接口对应Hive的hwi组件（hive web interface），使用前要启动hwi服务。

## 3. Hive的数据存储

Hive 的存储是建立在 Hadoop 文件系统之上的。Hive 本身没有专门的数据存储格式，也不能为数据建立索引，因此用户可以非常自由地组织 Hive 中的表，只需要在创建表的时候告诉 Hive 数据中的列分隔符就可以解析数据了。

Hive 中主要包括 4 种数据模型：**表（Table）、外部表（External Table）、分区（Partition）以及 桶（Bucket）**。

* Hive 的表和数据库中的表在概念上没有什么本质区别
* 外部表指向已经在 HDFS 中存在的数据，也可以创建分区。
* Hive 中的每个分区都对应数据库中相应分区列的一个索引，但是其对分区的组织方式和传统关系数据库不同。
* 桶在指定列进行 Hash 计算时，会根据哈希值切分数据，使每个桶对应一个文件。

**Hive 的元数据存储**

由于 Hive 的元数据可能要面临不断地更新、修改和读取操作，所以它显然不适合使用 Hadoop 文件系统进行存储。目前 Hive 把元数据存储在 RDBMS 中，比如存储在 MySQL, Derby 中。这点我们在上面介绍的 Hive 的体系结构图中，也可以看出。

