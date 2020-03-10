* 分区时机
* 默认分区源码分析
* 自定义分区算法

# 1. 分区时机

KafkaProducer在调用`send`方法发送消息至broker的过程中，首先是经过拦截器Inteceptors处理，然后是经过序列化Serializer处理，之后就到了Partitions阶段，即分区分配计算阶段。

在KafkaProducer计算分配时，首先根据的是ProducerRecord中的partition字段指定的序号计算分区。

# 2. 默认分区源码分析


* 序列化key存在时，对其采用`murmur2 hash`算法，再对总分区数取模。得到分区数；
* 序列化key不存在时，(轮询，round robin)
 * 可用分区数大于0时，用线程安全生成的随机数的绝对值 对 可用分区数 取模，在总分区列表中，找到对应的分区数。
 * 可用分区数等于0时，用线程安全生成的随机数的绝对值 对 总分区数 取模，得到分区数。

ProducerRecord：
```java
public class ProducerRecord<K, V> {

    private final String topic;
    private final Integer partition;
    private final Headers headers;
    private final K key;
    private final V value;
    private final Long timestamp;
}
```

KafkaProducer中：

```java
private int partition(ProducerRecord<K, V> record, byte[] serializedKey, byte[] serializedValue, Cluster cluster) {
    Integer partition = record.partition();
    return partition != null ?
            partition :
            partitioner.partition(
                    record.topic(), record.key(), 
                    serializedKey, record.value(), 
                    serializedValue, cluster);
}
```

DefaultPartitioner中：
```java
/**
 * If a partition is specified in the record, use it
 * If no partition is specified but a key is present choose a partition based on a hash of the key
 * If no partition or key is present choose the sticky partition that changes when the batch is full.
 */
public class DefaultPartitioner implements Partitioner {

    private final StickyPartitionCache stickyPartitionCache = new StickyPartitionCache();

    public void configure(Map<String, ?> configs) {}

    public int partition(String topic, Object key, byte[] keyBytes, Object value, byte[] valueBytes, Cluster cluster) {
        if (keyBytes == null) {
            return stickyPartitionCache.partition(topic, cluster);
        } 
        List<PartitionInfo> partitions = cluster.partitionsForTopic(topic);
        int numPartitions = partitions.size();
        // hash the keyBytes to choose a partition
        return Utils.toPositive(Utils.murmur2(keyBytes)) % numPartitions;
    }

    public void close() {}
    
    public void onNewBatch(String topic, Cluster cluster, int prevPartition) {
        stickyPartitionCache.nextPartition(topic, cluster, prevPartition);
    }
}
```

StickyPartitionCache：
```java
public class StickyPartitionCache {
    //(key,partition) 缓存
    private final ConcurrentMap<String, Integer> indexCache;
    public StickyPartitionCache() {
        this.indexCache = new ConcurrentHashMap<>();
    }

    //根据topic和集群信息获取分区
    public int partition(String topic, Cluster cluster) {
        Integer part = indexCache.get(topic);
        if (part == null) {
            return nextPartition(topic, cluster, -1);
        }
        return part;
    }

    public int nextPartition(String topic, Cluster cluster, int prevPartition) {
        //集群的分区数
        List<PartitionInfo> partitions = cluster.partitionsForTopic(topic);
        //缓存中获取
        Integer oldPart = indexCache.get(topic);
        Integer newPart = oldPart;
        // Check that the current sticky partition for the topic is either not set or that the partition that 
        // triggered the new batch matches the sticky partition that needs to be changed.
        
        // 缓存中没有 或者 为 -1
        if (oldPart == null || oldPart == prevPartition) {
            //获取可用分区数
            List<PartitionInfo> availablePartitions = cluster.availablePartitionsForTopic(topic);
            
            //没有可用分区数
            if (availablePartitions.size() < 1) {
                //随机正数
                Integer random = Utils.toPositive(ThreadLocalRandom.current().nextInt());
                //取模
                newPart = random % partitions.size();
            } else if (availablePartitions.size() == 1) {
                //只有一个可用
                newPart = availablePartitions.get(0).partition();
            } else {
                while (newPart == null || newPart.equals(oldPart)) {
                    Integer random = Utils.toPositive(ThreadLocalRandom.current().nextInt());
                    newPart = availablePartitions.get(random % availablePartitions.size()).partition();
                }
            }
            // Only change the sticky partition if it is null or prevPartition matches the current sticky partition.
            if (oldPart == null) {
                indexCache.putIfAbsent(topic, newPart);
            } else {
                indexCache.replace(topic, prevPartition, newPart);
            }
            return indexCache.get(topic);
        }
        return indexCache.get(topic);
    }

}
```

# 自定义Partitioner：

分为两步：
1. 实现Partition接口；
2. 注册接口：`properties.put("partitioner.class","com.hidden.partitioner.DemoPartitioner");`

