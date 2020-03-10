* 机器约定
* 集群部署步骤
* 操作命令

## 机器约定

Host | IP | 软件| 端口 | 节点 
---|---|--|--|-- 
server-1 | 21 | kafka_version* | 9092 | node1
server-2 | 22 | kafka_version* | 9092 | node2
server-3 | 23 | kafka_version* | 9092 | node3

Kafka源码目录：
* **bin**。一系列Shell脚本，比如server-start/stop，topics等。
* **config**。一系列配置文件，zk,sink,source,log,server等。
* **libs**。一系列运行jar包。
* **site-docs**。文档。

## 集群部署步骤

### S1 下载代码

http://kafka.apache.org/quickstart
```shell
# 解压
tar -xzf kafka_2.12-2.3.0.tgz
cd kafka_2.12-2.3.0
```
### S2 单机运行

```shell
# 启动单机的zk
bin/zookeeper-server-start.sh config/zookeeper.properties

# 启动单机kafka
bin/kafka-server-start.sh config/server.properties

# 创建一个单分区，单副本，本机的topcis
bin/kafka-topics.sh --create --bootstrap-server \
localhost:9092 --replication-factor 1 --partitions 1 --topic test

# 列出本机的topic
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

### S3 配置集群

配置server-1
针对config/server.properties文件
```shell
broker.id=1 #server2,3修改此处
listeners=PLAINTEXT://:9092
log.dirs=/tmp/kafka-logs-1
zookeeper.connect=  #配置zk的地址
```

* 如上配置server2，3
* `broker.id`:2,3
* `listeners`:9093,9094

### S4 启动集群

每台服务器均：

```
./kafka-server-start.sh -daemon ../config/server.properties
```


创建集群的topics：

```shell
bin/kafka-topics.sh --create --bootstrap-server \
localhost:9092 --replication-factor 3 --partitions 1 \
 --topic my-replicated-topic
```

参数说明：
* boostrap-server：指定kafka集群地址
* replication-factor：对每一个Topic的分区的副本数（包括Leader）
* partitions：分区数
* topic：Topic的名称

## 操作命令

```shell

1. 查看topic的详细信息  

./kafka-topics.sh -zookeeper 127.0.0.1:2181 -describe -topic testKJ1  

2. 为topic增加副本  

./kafka-reassign-partitions.sh -zookeeper 127.0.0.1:2181 \
-reassignment-json-file json/partitions-to-move.json -execute  

3. 创建topic 

./kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic testKJ1  

4. 为topic增加partition  

./bin/kafka-topics.sh –zookeeper 127.0.0.1:2181 \
–alter –partitions 20 –topic testKJ1  

5. kafka生产者客户端命令  

./kafka-console-producer.sh --broker-list localhost:9092 --topic testKJ1  

6. kafka消费者客户端命令  

./kafka-console-consumer.sh -zookeeper localhost:2181 --from-beginning --topic testKJ1  

7. kafka服务启动  

./kafka-server-start.sh -daemon ../config/server.properties   

8. 下线broker  

./kafka-run-class.sh kafka.admin.ShutdownBroker --zookeeper 127.0.0.1:2181 --broker #brokerId# --num.retries 3 --retry.interval.ms 60  


9. 删除topic  

./kafka-run-class.sh kafka.admin.DeleteTopicCommand --topic testKJ1 --zookeeper 127.0.0.1:2181  

./kafka-topics.sh --zookeeper localhost:2181 --delete --topic testKJ1  

10. 查看consumer组内消费的offset  

./kafka-run-class.sh kafka.tools.ConsumerOffsetChecker --zookeeper localhost:2181 --group test --topic testKJ1

11. 停止
./kafka-server-stop.sh
```