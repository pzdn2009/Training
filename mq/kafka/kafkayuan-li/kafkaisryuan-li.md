* 概述
* ISR配置
* 全部副本宕机

# 1. 概述

kafka不是完全同步，也不是完全异步，是一种**ISR机制**： 

1. leader会维护一个与其基本保持同步的Replica列表，该列表称为ISR(in-sync Replica)，每个Partition都会有一个ISR，**而且是由leader动态维护**————Get List 
2. 如果一个flower比一个leader落后太多，或者超过一定时间未发起数据复制请求，则leader将其重ISR中移除————Remove List
3. 当ISR中所有Replica都向Leader发送ACK时，leader才commit————All List
4. 如果leader挂掉了，从它的follower中选举一个作为leader，并把挂掉的leader从ISR中移除，继续处理数据。一段时间后该leader重新启动了，它知道它之前的数据到哪里了，尝试获取它挂掉后leader处理的数据，获取完成后它就加入了ISR。————Add List
5. 任何一条消息只有被这个集合中的每个节点读取并追加到日志中，才会向外部通知说“这个消息已经被提交”。

# 2. ISR配置

## Server配置
```yml

# 如果leader发现flower超过10秒没有向它发起fech请求， 
# 那么leader考虑这个flower是不是程序出了点问题
rerplica.lag.time.max.ms=10000

# 或者资源紧张调度不过来，它太慢了，不希望它拖慢后面的进度，就把它从ISR中移除。
rerplica.lag.max.messages=4000 # 相差4000条就移除

# flower慢的时候，保证高可用性，同时满足这两个条件后又加入ISR中，
# 在可用性与一致性做了动态平衡   亮点
```

## Topic配置
```yml
min.insync.replicas=1 # 需要保证ISR中至少有多少个replica
```

## Producer配置
```yml
request.required.asks=0
```

* 0:相当于异步的，不需要leader给予回复，producer立即返回，发送就是成功,那么发送消息网络超时或broker crash(1.Partition的Leader还没有commit消息 2.Leader与Follower数据不同步)，既有可能丢失也可能会重发  
* 1：当leader接收到消息之后发送ack，丢会重发，丢的概率很小
* -1：当所有的follower都同步消息成功后发送ack.  丢失消息可能性比较低

# 3. 全部副本宕机

两种方案：

1. 等待ISR中任一Replica恢复,并选它为Leader
等待时间较长,降低可用性或ISR中的所有Replica都无法恢复或者数据丢失,则该Partition将永不可用。————CAP中的C，一致性较高。

2. 选择第一个恢复的Replica为新的Leader,无论它是否在ISR中,并未包含所有已被之前Leader Commit过的消息,因此会造成数据丢失。————CAP中的A，可用性较高。
