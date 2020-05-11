# Redis计数器

* 数量
* 有上限的数量
* 原子操作

# 1. 全局数量

**每天用户注册数量**
* 每注册一个，Incr 1
* 24点过期

改进：
* Key：PREFIX+DATE
* 过期时间：一周
* job刷到关系数据库

**微博数量**
* 点赞数，评论数，转发数，查看数
* 采用hash：weibo:id (like,comment,forward,views)存储
* 使用hincrby来实现自增

