
* [HiveDDL-CreateTable](/basic/hadoop/hive/hiveddlcao-zuo/hiveddl-create.md)
* [HiveDDL-alter](/basic/hadoop/hive/hiveddlcao-zuo/hiveddl-alter.md)

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
