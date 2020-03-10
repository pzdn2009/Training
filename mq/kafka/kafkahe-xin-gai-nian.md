* 核心概念
* Producer
* Broker
* Consumer

# 1. 核心概念

名称|解释
--|--
Cluster|由多个服务器组成。每个服务器单独的名字broker
Broker|物理概念，kafka集群中的每个节点。
Producer|消息和数据的生产者，向kafka的一个Topic发布消息的进程/代码/服务。
Consumer|负责消费数据。每个consumer属于一个特定的consumer group（可为每个consumer指定group name，若不指定group name则属于默认的group）。使用consumer high level API时，同一topic的一条消息只能被同一个consumer group内的一个consumer消费，但多个consumer group可同时消费这一消息。
Topic|主题，代表一类消息数据。
Partition|parition是物理上的概念，每个topic包含一个或多个partition，创建topic时可指定parition数量。  每个partition对应于一个文件夹，该文件夹下存储该partition的数据和索引文件。
Replication|同一个Partition可能会多个Replica，多个Replica之间数据是一样的
Replication Leader|一个Partition的多个Replica上，需要一个Leader负责该Partition上与Producer和Consumer交互
ReplicaManager|负责管理当前broker所有分区和副本的信息，处理KafkaController发起的一些请求，副本状态的切换、添加/读取消息等。
Consumer Group|逻辑概念，对于同一个topic，会广播给不同的group，一个group中，只有一个consumer可以消费该消息。如果需要实现广播，只要每个Consumer有一个独立的Group就可以了。要实现单播只要所有的Consumer在同一个Group里
Lag|延迟，当 Consumer 的速度跟不上消息的产生速度时，Consumer 就会因为无法从分区中读取消息，而产生延迟。延迟表示为分区头后面的 Offset 数量。从延迟状态(到“追赶上来”)恢复正常所需要的时间，取决于 Consumer 每秒能够应对的消息速度。

# 2. Producer

* **顺序写入**。如果是随机IO，磁盘会进行频繁的寻址，导致写入速度下降。Kafka使用了顺序IO提高了磁盘的写入速度，Kafka会将数据顺序插入到文件末尾，消费者端通过控制偏移量来读取消息。
* **Memory Mapped Files**。
MMF直接利用操作系统的Page来实现文件到物理内存的映射，完成之后对物理内存的操作会直接同步到硬盘。mmf通过内存映射的方式大大提高了IO速率，省去了用户空间到内核空间的复制。它的缺点显而易见--不可靠，当发生宕机而数据未同步到硬盘时，数据会丢失，Kafka提供了produce.type参数来控制是否主动的进行刷新，如果kafka写入到mmp后立即flush再返回给生产者则为同步模式，反之为异步模式。

# 3. Broker

* 顺序写入
* Segment
* 分区
* ISR机制

# 4. Consumer

* **零拷贝**：平时从服务器读取静态文件时，服务器先将文件从复制到内核空间，再复制到用户空间，最后再复制到内核空间并通过网卡发送出去，而零拷贝则是直接从内核到内核再到网卡，省去了用户空间的复制。