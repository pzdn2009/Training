
## 图1

![](/assets/kafka-act1.jpeg)

1）Producer
2）Broker
3）Consumer
4）Zookeeper

## 图2
![](/assets/kafka-003.jpeg)

1）Topic A：有三个分区，每个分区有一个副本集，注意Leader的不同位置，注意Follower的不同位置。
2）Consumer Group1：分别消费每个Broker；每个Consumer只能消费一个分区；
3）消费者是按照分区来的；同一个组里面的不会重复消费；

## 图3
![](/assets/kafka02.png)

1）Cluster和Consumer都依赖Zookeeper。
2）Producer：Push模式
3）Consumer：Pull模式
