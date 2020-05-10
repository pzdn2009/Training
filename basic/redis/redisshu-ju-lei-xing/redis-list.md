# Redis-List

Value：双向链表

命令：
```
BLPOP key1 [key2 ] timeout #取出并获取列表中的第一个元素，或阻塞，直到有可用
BRPOP key1 [key2 ] timeout #取出并获取列表中的最后一个元素，或阻塞，直到有可用
BRPOPLPUSH source destination timeout #从列表中弹出一个值，它推到另一个列表并返回它;或阻塞，直到有可用
LINDEX key index #从一个列表其索引获取对应的元素
LINSERT key BEFORE|AFTER pivot value #在列表中的其他元素之后或之前插入一个元素
LLEN key #获取列表的长度
LPOP key #获取并取出列表中的第一个元素
LPUSH key value1 [value2] #在前面加上一个或多个值的列表
LPUSHX key value #在前面加上一个值列表，仅当列表中存在
LRANGE key start stop #从一个列表获取各种元素
LREM key count value #从列表中删除元素
LSET key index value #在列表中的索引设置一个元素的值
LTRIM key start stop #修剪列表到指定的范围内
RPOP key #取出并获取列表中的最后一个元素
RPOPLPUSH source destination #删除最后一个元素的列表，将其附加到另一个列表并返回它
RPUSH key value1 [value2] #添加一个或多个值到列表
RPUSHX key value #添加一个值列表，仅当列表中存在
```