# Hive

* [Hive安装](/basic/hadoop/hive/hivean-zhuang.md)
* [Hive体系架构](/basic/hadoop/hive/hiveti-xi-jia-gou.md)
* [HiveDDL操作](/basic/hadoop/hive/hiveddlcao-zuo.md)
* [Hive数据管理](/basic/hadoop/hive/hiveshu-ju-guan-li.md)
* [Hive-Training-单词统计](/basic/hadoop/hive/hive-trainingdan-ci-tong-ji.md)

## Hive简介

* 数据仓库框架
* 与关系数据库的区别

Hive是Facebook构建在Hadoop上的数据仓库框架。

## 1 数据仓库框架

* Hive是基于Hadoop的一个数据仓库工具，可以**将结构化的数据文件映射为一张数据库表**，并使用**sql语句转换为MapReduce任务**进行运行。其优点是学习成本低，可以通过类SQL语句快速实现简单的MapReduce统计，不必开发专门的MapReduce应用，十分适合数据仓库的统计分析；
* Hive是建立在 Hadoop 上的数据仓库基础构架。它提供了一系列的工具，可以用来进行数据提取转化加载（ETL），这是一种可以存储、查询和分析存储在 Hadoop 中的大规模数据的机制。Hive 定义了简单的类 SQL 查询语言，称为 HQL，它允许熟悉 SQL 的用户查询数据。同时，这个语言也允许熟悉 MapReduce 开发者的开发自定义的 Mapper 和 Reducer 来处理内建的Mapper 和Reducer 无法完成的复杂的分析工作。

## 2 Hive与关系数据库的区别

Hive|关系数据库
--|--
HDFS|本地文件系统
MapReduce|Executor
实时查询|离线计算
扩展自己的存储能力和计算能力|固定

## 3. Hive的数据存储

Hive 的存储是建立在 Hadoop 文件系统之上的。Hive 本身没有专门的数据存储格式，也不能为数据建立索引，因此用户可以非常自由地组织 Hive 中的表，只需要在创建表的时候告诉 Hive 数据中的列分隔符就可以解析数据了。

Hive 中主要包括 4 种数据模型：**表（Table）、外部表（External Table）、分区（Partition）以及 桶（Bucket）**。

* Hive 的表和数据库中的表在概念上没有什么本质区别
* 外部表指向已经在 HDFS 中存在的数据，也可以创建分区。
* Hive 中的每个分区都对应数据库中相应分区列的一个索引，但是其对分区的组织方式和传统关系数据库不同。
* 桶在指定列进行 Hash 计算时，会根据哈希值切分数据，使每个桶对应一个文件。

**Hive 的元数据存储**

由于 Hive 的元数据可能要面临不断地更新、修改和读取操作，所以它显然不适合使用 Hadoop 文件系统进行存储。目前 Hive 把元数据存储在 RDBMS 中，比如存储在 MySQL, Derby 中。这点我们在上面介绍的 Hive 的体系结构图中，也可以看出。

