# 操作

# 1. 数据库与表的操作

```sql
#创建数据库  
create database "db_name"  
   
#显示所有的数据库  
show databases  
  
#删除数据库  
drop database "db_name"  
  
#使用数据库  
use db_name  
   
#显示该数据库中所有的表  
show measurements  
  
#创建表，直接在插入数据的时候指定表名  
insert test,host=127.0.0.1,monitor_name=test count=1  
  
#删除表  
drop measurement "measurement_name"  
```

# 2. 插入数据

```sql
use metrics  
insert test,host=127.0.0.1,monitor_name=test count=1  
```
通过Http API
```shell
curl -i -XPOST 'http://127.0.0.1:8086/write?db=metrics' --data-binary 'test,host=127.0.0.1,monitor_name=test count=1'
```

# 3. 查询

```sql
use metrics
select * from test order by time desc 
```

通过Http API
```
curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=select * from test order by time desc" 
```

# 4. 数据保存策略（Retention Policies）

influxDB是没有提供直接删除数据记录的方法，但是提供数据保存策略，主要用于指定数据保留时间，超过指定时间，就删除这部分数据。

可以理解为**过期策略**。

```sql
# 显示保存策略
show retention policies on "db_name"  

# 创建保存策略
create retention policy "rp_name" on "db_name" duration 3w replication 1 default  
参数解释：
○ rp_name：策略名
○ db_name：具体的数据库名
○ 3w：保存3周，3周之前的数据将被删除，influxdb具有各种事件参数，比如：h（小时），d（天），w（星期）
○ replication 1：副本个数，一般为1就可以了
○ default：设置为默认策略

# 修改保存策略
alter retention policy "rp_name" on "db_name" duration 30d default  

# 删除保存策略
drop retention policy "rp_name"  
```

# 5. 连续查询（Continous Queries）

当数据超过保存策略里指定的时间之后就会被删除，但是这时候可能并不想数据被完全删掉，怎么办？

influxdb提供了联系查询，可以做数据统计采样。

```sql
# 查看连续查询
show continuous queries  

# 创建新的Continous Queries
create continous query cq_name on db_name begin select sum(count) into new_table_name from table_name group by time(30m) end  

参数解析：
● cq_name：连续查询名字
● db_name：数据库名字
● sum(count)：计算总和
● table_name：当前表名
● new_table_name：存新的数据的表名
● 30m：时间间隔为30分钟

# 删除Continous Queries
drop continous query cp_name on db_name  
```

# 6. 用户管理

可以直接在web管理页面做操作，也可以命令行。

```sql
#显示用户  
show users  
   
#创建用户  
create user "username" with password 'password'  
   
#创建管理员权限用户  
create user "username" with password 'password' with all privileges  

#删除用户  
drop user "username"  
```