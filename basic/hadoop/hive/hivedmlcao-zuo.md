
# 1. Load

* local：指Hive服务所在的机器上
* LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION(p1=val1,p2=val2,...)]
* OVERWRITE：覆盖
* filepath：文件或者文件夹

# 2. Insert 

* 结合select来使用
* 如果insert和select出来的列不一致，会尝试转换
* 多重插入：`from source_table insert table t1 select id insert table  t2 select name`;
* 动态分区插入

# 3. Select

