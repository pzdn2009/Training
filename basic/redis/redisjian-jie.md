# Redis简介

* 什么是Redis
* Redis有哪些优缺点

# 1. 什么是Redis

Redis is an **open source** (BSD licensed), **in-memory** data structure store, used as a **database**, **cache** and **message broker**. It supports data structures such as **strings, hashes, lists, sets, sorted sets** with range queries, bitmaps, hyperloglogs, geospatial indexes with radius queries and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

* 开源，内存，数据库，缓存，消息代理
* 五种数据类型
* 副本，Lua，LRU算法，事务，持久化
* HA，集群

# 2. Redis有哪些优缺点

## 优点

* in-memory：IO读写很快；
* 支持持久化：AOF和RDB两种方式；
* 支持主从复制；
* 数据结构丰富：string,list,hast,set,sorted-set；
* 单线程-多路复用IO模型。

>redis提供两种方式进行持久化，一种是RDB持久化:指在指定的时间间隔内将内存中的数据集快照写入磁盘，实际操作过程是fork一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。还有一种是AOF持久化：以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细的操作记录。
 

## 缺点：

* 主从同步，如果主机宕机，宕机前有一部分数据没有同步到从机，会导致数据不一致。
* 主从同步，数据同步会有延迟。
* 读写分离，主机写的负载量太大，也会导致主机的宕机