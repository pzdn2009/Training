# Redis-SortedSet

Value：`（member,score）`

命令：
```
ZADD key score1 member1 [score2 member2] 
# 添加一个或多个成员到有序集合，或者如果它已经存在更新其分数
ZCARD key 
#得到的有序集合成员的数量
ZCOUNT key min max  
#计算一个有序集合成员与给定值范围内的分数

ZINCRBY key increment member 
#在有序集合增加成员的分数
ZINTERSTORE destination numkeys key [key ...] 
# 多重交叉排序集合，并存储生成一个新的键有序集合。
ZLEXCOUNT key min max 
#计算一个给定的字典范围之间的有序集合成员的数量

ZRANGE key start stop [WITHSCORES] 
#由索引返回一个成员范围的有序集合（从低到高）
ZRANGEBYLEX key min max [LIMIT offset count]
# 返回一个成员范围的有序集合（由字典范围）
ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT] 
# 返回有序集key中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员，有序集成员按 score 值递增(从小到大)次序排列

ZRANK key member 
#确定成员的索引中有序集合
ZREM key member [member ...] 
#从有序集合中删除一个或多个成员，不存在的成员将被忽略
ZREMRANGEBYLEX key min max 
#删除所有成员在给定的字典范围之间的有序集合

ZREMRANGEBYRANK key start stop 
#在给定的索引之内删除所有成员的有序集合
ZREMRANGEBYSCORE key min max 
#在给定的分数之内删除所有成员的有序集合
ZREVRANGE key start stop [WITHSCORES] 
#返回一个成员范围的有序集合，通过索引，以分数排序，从高分到低分

ZREVRANGEBYSCORE key max min [WITHSCORES] 
#返回一个成员范围的有序集合，以socre排序从高到低
ZREVRANK key member 
#确定一个有序集合成员的索引，以分数排序，从高分到低分
ZSCORE key member 
#获取给定成员相关联的分数在一个有序集合

ZUNIONSTORE destination numkeys key [key ...] 
#添加多个集排序，所得排序集合存储在一个新的键
ZSCAN key cursor [MATCH pattern] [COUNT count] 
#增量迭代排序元素集和相关的分数
```