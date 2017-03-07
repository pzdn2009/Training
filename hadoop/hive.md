# Hive

# 1. 简介

Hive是Facebook构建在Hadoop上的数据仓库框架。

## 1.1 数据仓库框架

- Hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并使用sql语句转换为MapReduce任务进行运行。其优点是学习成本低，可以通过类SQL语句快速实现简单的MapReduce统计，不必开发专门的MapReduce应用，十分适合数据仓库的统计分析；
- Hive是建立在 Hadoop 上的数据仓库基础构架。它提供了一系列的工具，可以用来进行数据提取转化加载（ETL），这是一种可以存储、查询和分析存储在 Hadoop 中的大规模数据的机制。Hive 定义了简单的类 SQL 查询语言，称为 HQL，它允许熟悉 SQL 的用户查询数据。同时，这个语言也允许熟悉 MapReduce 开发者的开发自定义的 Mapper 和 Reducer 来处理内建的Mapper 和Reducer 无法完成的复杂的分析工作。

## 1.2 Hive与关系数据库的区别

1. Hive和关系数据库存储文件的系统不同，Hive使用的是Hadoop的HDFS（Hadoop的分布式文件系统），关系数据库则是服务器本地的文件系统；
2. Hive使用的计算模型是MapReduce，而关系数据库则是自身的计算模型；
3. 关系数据库都是为实时查询的业务进行设计的，而Hive则是为海量数据做数据挖掘设计的，实时性很差；实时性的区别导致Hive的应用场景和关系数据库有很大的不同；
4. Hive很容易扩展自己的存储能力和计算能力，这个是继承Hadoop的，而关系数据库在这个方面要比数据库差很多。

## 1.3 安装

直接从官方下载一个发布包，
```shell
tar xzf hive-x.y.z-dev.tar.gz
export HIVE_HOME=/home/pzdn/hive-x.y.z-dev
export PATH=$PATH:$HIVE_HOME/bin
```

hive命令
```
hive>SHOW TABLES;
```