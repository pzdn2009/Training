# 1. 概述

1、Hive中所有的数据都存储在 HDFS 中，没有专门的数据存储格式（可支持Text，SequenceFile，ParquetFile，RCFILE等）

2、只需要在创建表的时候告诉 Hive 数据中的列分隔符和行分隔符，Hive 就可以解析数据。

3、Hive 中包含以下数据模型：DB、Table，External Table，Partition，Bucket。

- db：在hdfs中表现为`${hive.metastore.warehouse.dir}`目录下一个文件夹
- table：在hdfs中表现所属db目录下一个文件夹
- external table：与table类似，不过其数据存放位置可以在任意指定路径
- partition：在hdfs中表现为table目录下的`子目录`
- bucket：在hdfs中表现为同一个表目录下根据hash散列之后的`多个文件`

# 2. Hive中的内部表和外部表

* Hive的create创建表的时候，选择的创建方式：
 * create table
 * create external table
* 特点：
 * 在导入数据到外部表，数据并没有移动到自己的数据仓库目录下，也就是说外部表中的数据并不是由它自己来管理的！而表则不一样；
 * 在删除表的时候，Hive将会把属于表的元数据和数据全部删掉；而删除外部表的时候，Hive仅仅删除外部表的元数据，数据是不会删除的！

# 3. Hive中的Partition

* 在 Hive 中，表中的一个 Partition 对应于表下的一个目录，所有的 Partition 的数据都存储在对应的目录中
 * 例如：pvs 表中包含 ds 和 city 两个 Partition，则
 * 对应于 ds = 20090801, ctry = US 的 HDFS 子目录为：/wh/pvs/ds=20090801/ctry=US；
 * 对应于 ds = 20090801, ctry = CA 的 HDFS 子目录为；/wh/pvs/ds=20090801/ctry=CA
* partition是辅助查询，缩小查询范围，加快数据的检索速度和对数据按照一定的规格和条件进行管理。

# 4. Hive中的Bucket

* hive中table可以拆分成partition，table和partition可以通过‘CLUSTERED BY’进一步分bucket，bucket中的数据可以通过`SORT BY`排序。
```sql
create table bucket_user (id int,name string)
clustered by (id) into 4 buckets;
```
* `set hive.enforce.bucketing = true`，可以自动控制上一轮reduce的数量从而适配bucket的个数，当然，用户也可以自主设置`mapred.reduce.tasks`去适配bucket个数

* Bucket主要作用：
 * 数据sampling
 * 提升某些查询操作效率，例如mapside join

* 查看sampling数据：
> hive> select * from student tablesample(bucket 1 out of 2 on id);
  * tablesample是抽样语句，语法：TABLESAMPLE(BUCKET x OUT OF y)
  * y必须是table总bucket数的倍数或者因子。hive根据y的大小，决定抽样的比例。例如，table总共分了64份，当y=32时，抽取(64/32=)2个bucket的数据，当y=128时，抽取(64/128=)1/2个bucket的数据。x表示从哪个bucket开始抽取。例如，table总bucket数为32，tablesample(bucket 3 out of 16)，表示总共抽取（32/16=）2个bucket的数据，分别为第3个bucket和第（3+16=）19个bucket的数据。

# 5. Hive数据类型

* 数据类型
 * TINYINT
 * SMALLINT
 * INT
 * BIGINT
 * BOOLEAN
 * FLOAT
 * DOUBLE
 * STRING
 * BINARY（Hive 0.8.0以上才可用）
 * TIMESTAMP（Hive 0.8.0以上才可用）

* 复合类型
 * Arrays：`ARRAY<data_type>`
 * Maps:`MAP<primitive_type, data_type>`
 * Structs:`STRUCT<col_name: data_type[COMMENT col_comment],……>`
 * Union:`UNIONTYPE<data_type, data_type,……>`


Ref:https://blog.csdn.net/ForgetThatNight/article/details/79632364