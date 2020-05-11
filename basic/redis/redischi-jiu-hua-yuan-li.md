# Redis持久化

Ref:https://mp.weixin.qq.com/s?__biz=MzI4NTA1MDEwNg==&mid=2650769300&idx=1&sn=49a11efa1a6ee605fceaddf240a55c40&chksm=f3f93201c48ebb175fa76053d95e315b621485b0e65e42d8b41fe91b8f859c9278f3adec7ca9&mpshare=1&scene=23&srcid=0731SR4C94CRM0Mljym0oEI3%23rd

* 什么是Redis持久化？
 * 将内存数据同步至磁盘。
* Redis的持久化机制是什么？各自的优缺点？
 * RDB：快照，全量
 * AOF：文件只追加，增量
* 如何选择合适的持久化方式
* Redis持久化数据和缓存怎么做扩容？
* Redis的两种持久化操作以及如何保障数据安全（快照和AOF）

# 1. RDB方式

* 触发条件
 * bgsave，Fork方式
 * save，阻塞方式
 * save n m 
 * shutdown 
 * sync 

