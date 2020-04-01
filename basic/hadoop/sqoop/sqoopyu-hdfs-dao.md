# sqoop导入Hdfs

Sample：

```
sqoop-import \
--connect jdbc:mysql://hostname:3306/mydb \
--username root \
--password root \

--table mytable \
--target-dir /user/hive/warehouse/my_user \
--delete-target-dir \
--num-mappers 1 \
--fields-terminated-by "\t" \
--columns  id,passwd \
--where "id<=3"

--check-column id \
--incremental append \
--last-value 4      //表示从第5位开始导入

--as-parquetfile

--compress \
--compression-codec org.apache.hadoop.io.compress.SnappyCodec

--query 'select id,account from my_user where id>=3 and $CONDITIONS'
```

# Options Files

sqoop.opt
```
import
--connect
jdbc:mysql://hostname:3306/mydb
--username
root
--password
root
--table
mytable
--num-mappers
1
--export-dir
/user/hive/warehouse/mydb.db/mytable
--input-fields-terminated-by
"\t"
```

```
$ bin/sqoop  --options-file sqoop.opt
```