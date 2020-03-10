* 副本概念
* 同步复制 & 异步复制
* 副本分配算法
* 副本协同机制
 * LEO
 * HW

# 1. 副本概念

副本是针对Topic的分区的。
>Eg：一个Topic有三个分区，2个副本，那么总共有6个分区。

在正常情况下，kafka每个分区都有一个单独的leader，0个或多个follower。其中，副本的总数包括Leader。

所有的读写都经过Leader。

通常，分区数比broker多，leader均匀分布在broker。follower的日志完全等同于leader的日志 — 相同的顺序相同的偏移量和消息（当然，在任何一个时间点上，leader可能比follower多几条消息，尚未同步到follower）。

## Kafka节点存活条件
* A node must be able to maintain its session with ZooKeeper (via ZooKeeper's heartbeat mechanism) ——节点和Zookeeper保持心跳
* If it is a slave it must replicate the writes happening on the leader and not fall "too far" behind —— slave不要隔得太远

## 移除条件

如果一个follower死掉，卡住，或落后，leader将从同步副本列表中移除它。
* 落后是通过`replica.lag.max.messages`配置控制；
* 卡住是通过`replica.lag.time.max.ms`配置控制的。

## 已提交定义

我们现在可以更精确的定义当该分区的所有同步副本已经写入到其日志中时漫该消息视为“已提交”。只有“已提交”的消息才会给到消费者。所有消费者无需担心如果leader故障，会消费到丢失的消息。

另一方面，生产者可以选择等待消费提交，这取决于你更偏向延迟或耐用性（通过acks控制）。当生产者请求确保消息已经写入到全部的同步副本中（可以通过设置topic同步副本的“最小数”）。如果生产者要求不严格，则即使同步副本的数量低于最小值，也可以提交和消费该消息。

kafka提供担保，在任何时候，只要至少有一个同步副本活着，承诺的消息就不会丢失。

# 2. 同步复制与异步复制

同步复制：

1. producer联系zk识别leader
2. 向leader发送消息
3. leader收到消息写入到本地log
4. follower从leader pull消息
5. follower向本地写入log
6. follower向leader发送ack消息
7. leader收到所有follower的ack消息
8. leader向producer回传ack

异步复制：

和同步复制的区别在于，leader写入本地log之后，直接向client回传ack消息，不需要等待所有follower复制完成。

# 3. 副本分配算法

* 将所有N Broker和待分配的i个Partition排序.
* 将第i个Partition分配到第(i mod n)个Broker上.
* 将第i个Partition的第j个副本分配到第((i + j) mod n)个Broker上.

# 4. 副本协同机制

**LEO（log end offset）** 

日志末端位移(log end offset)，记录了该副本底层 日志(log)中下一条消息的offset。如果LEO=10，那么表示该副本保存了10条消息， 位移值范围是[0, 9]。对于leader副本而言，leo在新消息写入时leo更新；对于follower而言，在向leader副本发起一次同步数据请求时，leader有新消息写入并且没同步给follower，这时将消息返回给follower，follower的leo更新。

**HW（high watermark）**

高水位，用来指示，在hw之前的offset消息都是可以消费的，也就是消息是已经提交的概念。例如hw是10，那么0-9的offset’的消息都是可以进行消费的消息。 
对于leader而言，hw更新需要根据producer设置的ack模式来确定。 

1. ack设置为0，表示不需要任何副本确认，这时候消息在leader副本写入之后直接更新leader分区hw，表示消息是已提交可以消费。 
2. ack设置为1，表示只需要leader副本确认消息，这时候消息在leader副本写入之后直接更新leader分区hw，并且向producer发送确认ack，表示消息是已提交可以消费。 
3. ack设置为-1，表示消息需要n个副本确认消息，这时候需要leader收到n个副本的同步消息成功的ack之后才更新leader的hw并且发送ack给producer确认，表示消息是已提交可以消费。 
这个n值指的是min.insync.replicas，设定ISR中的最小副本数是多少，默认值为 1, 当且仅当 acks 参数设置为-1 （表示需要所有副本确认）时，此参数才生效. 表达的含义 是，isr中至少有这么多个副本，需要isr中所有副本确认才可以认为消息已经提交。

对于follower而言，hw用来在重新选举leader时，表明哪些消息是可以消费的。

### 协同过程

Ref：https://blog.csdn.net/qq_20597727/article/details/81742431
