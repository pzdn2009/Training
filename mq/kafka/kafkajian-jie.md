1. 概述
2. 关键概念
3. 特点
4. Core项目结构初识

# 1. 概述

* Apache Kafka起源于LinkedIn。
* Apache Kafka® is a distributed streaming platform
  
  Kafka是一个分布式的、可分区的、可复制的消息系统。

* JAVA和Scala语言开发
* Git地址：https://github.com/apache/kafka

分布式的、可分区的、可复制：
1. 发布和订阅消息流，这个功能类似于消息队列，这也是kafka归类为消息队列框架的原因
2. 以容错的方式记录消息流，kafka以文件的方式来存储消息流
3. 可以再消息发布的时候进行处理


>Kafka is generally used for two broad classes of applications:
>
* Building real-time streaming data pipelines that reliably get data between systems or applications
* Building real-time streaming applications that transform or react to the streams of data

所以，Kafka能够构建实时流记录应用。 Real-time streaming.

# 2. 关键概念

* Cluster——集群
* Topic——主题，表示一类消息。
* Record——记录，即Message，消息。
* Message——消息，Kafka中数据传输的载体。
* Producer API——生产者API
* Consumer API——消费者API
* Streams API ——流API
* Connectors API——连接器API
* Topics And Logs——主题和日志，一个是逻辑概念，一个是物理概念。
* Partition——分区，一个Topic可以分为多个分区，目的是为了做水平负载扩展。
* Group——分组，针对消费者而言。
* Multi-tennancy——多租户。
* Broker——指一台机器节点。
* Replication——副本，对Partition的备份。
* Replication Leader
* ReplicaManager
* Offset——偏移量，消息在log文件中相对位置。
* Lag——延迟，对于Message的处理。
* 作为一个消息系统——类似消息队列
* 作为一个存储系统——类似DB
* 流处理

# 3. 特点

**分布式**

通过多个Broker来组成集群，使用了ZAB协议来维持Leader和Follow之间的通讯，特点是Leader负责读写，Follow只负责备份。
* **多分区**。 对于同一个Topic，通过多个分区，将其分散到多个Broker上，以实现**负载均衡和水平扩展**。每一个分区内的消息是顺序的，给到Consumer的时候，可以保证这个顺序性，顺序是通过Offset来记录实现的。对于整个Topic来说，顺序性不能被保证。
* **多副本**。指定--replication-factor来实现副本集，不能多于Broker的数量。基于一个分区副本算法均匀分配。
* **多订阅者**。
* 基于ZooKeeper调度。使用了ZAX协议。

**高性能**
* 高吞吐量。生产者：顺序写入，MMF。消费者：零拷贝。
* 低延迟。OS Page Cache，顺序写，零拷贝技术——解决从os cache 到应用，应用到socket缓存。
* 高并发
* 时间复杂度为O(1)

**持久性和扩展性**
* 数据可持久化。顺序，日志文件，稀疏索引。
* 容错性。
* 支持水平在线扩展。
* 消息自动平衡。


多个分区顺序写是保证高吞吐量的手段之一。


# 4. Core项目结构初识

![](/assets/kafka-001.png)

* **Admin**。 kafka的管理员模块，操作和管理其topic，partition相关，包含创建，删除topic，或者拓展分区等。
* **Api** 。主要负责数据交互，客户端与服务端交互数据的编码与解码。
* **cluster**。这里包含多个实体类，有Broker，Cluster，Partition，Replica。其中一个Cluster由多个Broker组成，一个Broker包含多个Partition，一个Topic的所有Partition分布在不同的Broker中，一个Replica包含都个Partition。
* **common**。这是一个通用模块，其只包含各种异常类以及错误验证。
* **consumer**。消费者处理模块，负责所有的客户端消费者数据和逻辑处理。
* **controller**。此模块负责中央控制器的选举，分区的Leader选举，Replica的分配或其重新分配，分区和副本的扩容等。
* **coordinator**。负责管理部分consumer group和他们的offset。
* **log**。这是一个负责Kafka文件存储模块，负责读写所有的Kafka的Topic消息数据。
* **message**。封装多条数据组成一个数据集或者压缩数据集。
* **metrics**。负责内部状态的监控模块。
* network
* **security**。负责Kafka的安全验证和管理模块。
* **serializer**。序列化和反序列化当前消息内容。
* **tools**
* **utils**。各种工具类，比如Json，ZkUtils，线程池工具类，KafkaScheduler公共调度器类，Mx4jLoader监控加载器，ReplicationUtils复制集工具类，CommandLineUtils命令行工具类，以及公共日志类等内容。
* **zk**。
* **zookeeper**。

