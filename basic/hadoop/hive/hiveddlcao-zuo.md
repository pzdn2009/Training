Hive文法：
```
create [external] table[if not exists] table_name
[(col_name data_type [comment col_comment], ...)]
[comment table_comment]
[partitioned by (col_name data_type [comment col_comment], ...)]
[clustered by (col_name, col_name, ...)
[sorted by (col_name [asc|desc], ...)] into num_buckets buckets]
[row format row_format]
[sorted as file_format]
[location hdfs_path]
```

* external：外部表，需要配置location来使用；
* 列描述: 列名 数据类型
* partitioned by：分区字段
* clustered by：桶字段，需要加上into num buckets
* row format：序列化方式
* like：复制表结构，不复制数据


# 1. 创建表

```sql
# 创建简单表
create table pokes (foo int, bar string);

# 内置分隔符创建表
create table pokes (foo int, bar string) 
row format delimited fields terminated by ',';

# 行分隔，字段分隔
create table user_info (user_id int, cid string, ckid string, username string)
row format delimited
fields terminated by '\t'
lines terminated by '\n';

# 创建分区表
create table table_name (
  id                int,
  dtDontQuery       string,
  name              string
)
partitioned by (date string);

# 复制表结构
create table empty_key_value_store like key_value_store;

# 创建Buckets表
creat table par_table(viewTime int, userid bigint)
  partitioned by(date string, pos string)
  clustered by(userid) into 32 buckets
  row format delimited ‘\t’
  fields terminated by '\n';
```

# 2. 显示表

```sql
# 显示所有表
show tables;

# 正则匹配
show tables '.*s';

# 查看单表
desc formatted table_name;  
desc table_name;  
```

# 3. 修改表

```sql
# 新增列
alter table pokes add columns (new_col int);

# 新增列带注释
alter table invites add columns (new_col2 int comment 'a comment');

# 修改表明
alter table events rename to pzdn;

# 删除列
alter table pokes drop name;

# 删除表
drop table pokes;
```
