# Redis

* [Redis简介](/basic/redis/redisjian-jie.md)
* [Redis数据类型](/basic/redis/redisshu-ju-lei-xing.md)
* [Redis线程模型](/basic/redis/redisxian-cheng-mo-xing.md)

Redis的两种持久化操作以及如何保障数据安全（快照和AOF）
如何防止数据出错（Redis事务）
如何使用流水线来提升性能
Redis主从复制
Redis集群的搭建
Redis的几种淘汰策略
Redis集群宕机，数据迁移问题
Redis缓存使用有很多，怎么解决缓存雪崩和缓存穿透？

概述
为什么要用 Redis /为什么要用缓存
为什么要用 Redis 而不用 map/guava 做缓存?
Redis为什么这么快
数据类型
Redis的应用场景
持久化
什么是Redis持久化？
Redis 的持久化机制是什么？各自的优缺点？
如何选择合适的持久化方式
Redis持久化数据和缓存怎么做扩容？
过期键的删除策略
Redis的过期键的删除策略
Redis key的过期时间和永久有效分别怎么设置？
我们知道通过expire来设置key 的过期时间，那么对过期的数据怎么处理呢?
内存相关
MySQL里有2000w数据，redis中只存20w的数据，如何保证redis中的数据都是热点数据
Redis的内存淘汰策略有哪些
Redis主要消耗什么物理资源？
Redis的内存用完了会发生什么？
Redis如何做内存优化？
事务
什么是事务？
Redis事务的概念
Redis事务的三个阶段
Redis事务相关命令
事务管理（ACID）概述
Redis事务支持隔离性吗
Redis事务保证原子性吗，支持回滚吗
Redis事务其他实现
集群方案
哨兵模式
官方Redis Cluster 方案(服务端路由查询)
基于客户端分配
基于代理服务器分片
Redis 主从架构
Redis集群的主从复制模型是怎样的？
生产环境中的 redis 是怎么部署的？
说说Redis哈希槽的概念？
Redis集群会有写操作丢失吗？为什么？
Redis集群之间是如何复制的？
Redis集群最大节点个数是多少？
Redis集群如何选择数据库？
分区
Redis是单线程的，如何提高多核CPU的利用率？
为什么要做Redis分区？
你知道有哪些Redis分区实现方案？
Redis分区有什么缺点？
分布式问题
Redis实现分布式锁
如何解决 Redis 的并发竞争 Key 问题
分布式Redis是前期做还是后期规模上来了再做好？为什么？
什么是 RedLock
缓存异常
缓存雪崩
缓存穿透
缓存击穿
缓存预热
缓存降级
热点数据和冷数据
缓存热点key
常用工具
Redis支持的Java客户端都有哪些？官方推荐用哪个？
Redis和Redisson有什么关系？
Jedis与Redisson对比有什么优缺点？
其他问题
Redis与Memcached的区别
如何保证缓存与数据库双写时的数据一致性？
Redis常见性能问题和解决方案？
Redis官方为什么不提供Windows版本？
一个字符串类型的值能存储最大容量是多少？
Redis如何做大量数据插入？
假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如果将它们全部找出来？
使用Redis做过异步队列吗，是如何实现的
Redis如何实现延时队列
Redis回收进程如何工作的？
Redis回收使用的是什么算法？

# V1.0

## V0.3 Redis应用

1. Redis数据类型

## V0.2 Redis原理

1. Redis单线程

## V0.1 Redis简介

1. 什么是Redis，Redis的优缺点；


