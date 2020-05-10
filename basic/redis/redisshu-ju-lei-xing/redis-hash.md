# Redis-Hash

Value：Hash表

命令：
```
HDEL key field[field...] # 删除对象的一个或几个属性域，不存在的属性将被忽略
HEXISTS key field # 查看对象是否存在该属性域
HGET key field #获取对象中该field属性域的值
HGETALL key #获取对象的所有属性域和值
HINCRBY key field value #将该对象中指定域的值增加给定的value，原子自增操作，只能是integer的属性值可以使用
HINCRBYFLOAT key field increment #将该对象中指定域的值增加给定的浮点数
HKEYS key #获取对象的所有属性字段
HVALS key #获取对象的所有属性值
HLEN key #获取对象的所有属性字段的总数
HMGET key field[field...] #获取对象的一个或多个指定字段的值
HSET key field value #设置对象指定字段的值
HMSET key field value [field value ...] #同时设置对象中一个或多个字段的值
HSETNX key field value #只在对象不存在指定的字段时才设置字段的值
HSTRLEN key field #返回对象指定field的value的字符串长度，如果该对象或者field不存在，返回0.
HSCAN key cursor [MATCH pattern] [COUNT count] #类似SCAN命令
```

