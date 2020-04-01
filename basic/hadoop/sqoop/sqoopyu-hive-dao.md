# Sqoop导入Hive

# 1. 导入到hive

```
sqoop import --help | grep hive
```
* hive-import，表示导入到hive
* hive-database，指定数据库
* hive-table，指定表
* hive-overwrite，替换已有的表

```
bin/sqoop import \
--connect jdbc:mysql://hostname:3306/mydb \
--username root \
--password root \
--table mytable \
--num-mappers 1 \
--hive-import \
--hive-database mydb \
--hive-table mytable \
--fields-terminated-by "\t" \
--delete-target-dir \
--hive-overwrite 
```

# 2. 从hive导出

```
sqoop export --help | grep hive
```


```
bin/sqoop export \
--connect jdbc:mysql://hostname:3306/mydb \
--username root \
--password root \
--table mytable \
--num-mappers 1 \
--export-dir /user/hive/warehouse/mydb.db/mytable \
--input-fields-terminated-by "\t"
```

* export-dir：指定源目录
* `--hive-partition-key <partition-key>`
* `--hive-home <dir>`
* `--hive-partition-value <partition-value>`
* `--map-column-hive <arg>`
