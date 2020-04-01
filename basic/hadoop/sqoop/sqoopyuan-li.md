# Sqoop-import原理

官方文档：https://sqoop.apache.org/docs/1.4.7/SqoopUserGuide.html#_purpose

# 1. import原理

![](/assets/sqoop-import2.png)

1. Sqoop使用JDBC来检查将要导入的表；
2. 检索出表中所有的列以及列的SQL数据类型；
3. SQL类型（VARCHAR、INTEGER）被映射到Java数据类型（String、Integer等）；
4. Sqoop的代码生成器使用这些信息来创建对应表的类，用于保存从表中抽取的记录；
5. 启动MapTask执行读取；
6. 将数据保存在HDFS中。

# 2. sqoop-import 

语法：
```
$ sqoop import (generic-args) (import-args)
$ sqoop-import (generic-args) (import-args)
```
* 通用参数，导入参数两大部分

## 2.1 通用参数generic-args

Argument|Description
--|--
--connect <jdbc-uri>| Specify JDBC connect string
--connection-manager <class-name>| Specify connection manager class to use
--driver <class-name>|Manually specify JDBC driver class to use
--hadoop-mapred-home <dir>|Override $HADOOP_MAPRED_HOME
--help|Print usage instructions
--password-file	| Set path for a file containing the authentication password
-P|Read password from console
--password <password>|	Set authentication password
--username <username>|	Set authentication username
--verbose	|Print more information while working
--connection-param-file <filename>|	Optional properties file that provides connection parameters
--relaxed-isolation|	Set connection transaction isolation to read uncommitted for the mappers.

密码：
* password，命令行直接输入
* password-file，文件输入
* -P ，隐藏输入

连接要素：
* connect：连接字符串 
* driver：驱动，Eg：`com.mysql.jdbc.Driver`
* username：用户名
* password：密码

## 2.2 导入控制参数import-args

Argument|Description
--|--
--append|Append data to an existing dataset in HDFS。追加形式
--as-avrodatafile|Imports data to Avro Data Files Avro格式
--as-sequencefile|Imports data to SequenceFiles Seq格式
--as-textfile	|Imports data as plain text (default) 文本格式
--as-parquetfile|	Imports data to Parquet Files 列格式
--boundary-query <statement>|	Boundary query to use for creating splits
--columns <col,col,col…>|Columns to import from table 指定列
--delete-target-dir|Delete the import target directory if it exists
--direct|Use direct connector if exists for the database
--fetch-size <n>|Number of entries to read from database at once.
--inline-lob-limit <n>	|Set the maximum size for an inline LOB
-m,--num-mappers <n>	|Use n map tasks to import in parallel
-e,--query <statement>	|Import the results of statement.
--split-by <column-name>	|Column of the table used to split work units. Cannot be used with --autoreset-to-one-mapper option.
--split-limit <n>	|Upper Limit for each split size. This only applies to Integer and Date columns. For date or timestamp fields it is calculated in seconds.
--autoreset-to-one-mapper	|Import should use one mapper if a table has no primary key and no split-by column is provided. Cannot be used with --split-by <col> option.
--table <table-name>|	Table to read
--target-dir <dir>	|HDFS destination dir
--temporary-rootdir <dir>|	HDFS directory for temporary files created during import (overrides default "_sqoop")
--warehouse-dir <dir>	|HDFS parent for table destination
--where <where clause>	|WHERE clause to use during import
-z,--compress	|Enable compression
--compression-codec <c>|	Use Hadoop codec (default gzip)
--null-string <null-string>|	The string to be written for a null value for string columns
--null-non-string <null-string>|	The string to be written for a null value for non-string columns

指定导入的数据？
* table
* columns
* where

自由查询导入？
* query
* target-dir，必须指定。默认路径：`/user/someuser/tablename/(files)`
* split-by，控制并行度

```
$ sqoop import \
  --query 'SELECT a.*, b.* FROM a JOIN b on (a.id == b.id) WHERE $CONDITIONS' \
  --split-by a.id --target-dir /user/foo/joinresults
  
  
$ sqoop import \
  --query 'SELECT a.*, b.* FROM a JOIN b on (a.id == b.id) WHERE $CONDITIONS' \
  -m 1 --target-dir /user/foo/joinresults
```

并行度？
* m，指定Mapper的个数
* split-by，指定用来切分的列，如果不指定，则默认选择主键，如果表没有主键，则必须指定此Option
* split-limit：切片的大小，控制数据的条数。

增量导入？

* --check-column (col)
* --incremental (mode：append|lastmodified)
* --last-value (value)

