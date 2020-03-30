# HiveDML-Load


Hive does not do any transformation while loading data into tables.

Load operations are currently **pure copy/move** operations that move datafiles into locations corresponding to Hive tables.

语法：
```sql
LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)]
 
LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)] [INPUTFORMAT 'inputformat' SERDE 'serde'] (3.0 or later)
```

* LOCAL：Hive服务所在的服务器上；
* OVERWRTE：覆盖
* filepath：文件或者文件夹

```sql
CREATE TABLE tab1 (col1 int, col2 int) PARTITIONED BY (col3 int) STORED AS ORC;

LOAD DATA LOCAL INPATH 'filepath' INTO TABLE tab1;
```
