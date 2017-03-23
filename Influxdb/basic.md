# 基础

# 术语

- database：数据库
- measurement：表
- points：记录，一行数据

  Point由时间戳（time）、数据（field）和标签（tags）组成：
 - time：每条数据记录的时间，也是数据库自动生成的主索引；
 - fields：字段；
 - tags：各种有索引的属性。

- series
  series表示这个表里面的所有的数据可以在图标上画成几条线

# Grammar Rule
  
```
<measurement>[,<tag-key>=<tag-value>...] <field-key>=<field-value>[,<field2-key>=<field2-value>...] [unix-nano-timestamp]
```
eg:
```
insert test,host=127.0.0.1,monitor_name=test count=1

Line Protocol格式：写入数据库的Point的固定格式：
1. test：表名；
2. host=127.0.0.1,monitor_name=test：tag；
3. count=1：field
```