# MR-shuffle

![](/assets/MPShuffle.png)


# 1. Map端 Shuffle过程

![](/assets/MPShuffle2.png)

Map任务在完成后输出的`<k2,v2>`，存储在一个环形内存缓冲区中，默认100M。

他有一个阈值，默认是0.8，一旦达到阈值，后台会开启一个线程以**轮询方式**将缓冲区中的内容溢写入磁盘。

溢写和map输出写入缓存可同时进行，除非缓存区已满。

* Partition
* 写入环形缓冲区
* Sort
* Group
* Combine

## Partition

由key值决定Mapper的输出会被哪一个Reducer处理。

```java
public interface Partitioner<K2, V2> extends JobConfigurable {
  
  /** 
   * Get the paritition number for a given key (hence record) given the total 
   * number of partitions i.e. number of reduce-tasks for the job.
   *   
   * <p>Typically a hash function on a all or a subset of the key.</p>
   *
   * @param key the key to be paritioned.
   * @param value the entry value.
   * @param numPartitions the total number of partitions.
   * @return the partition number for the <code>key</code>.
   */
  int getPartition(K2 key, V2 value, int numPartitions);
}
```

比如Hash：

```java
public class HashPartitioner<K2, V2> implements Partitioner<K2, V2> {

  public void configure(JobConf job) {}

  /** Use {@link Object#hashCode()} to partition. */
  public int getPartition(K2 key, V2 value,
                          int numReduceTasks) {
    return (key.hashCode() & Integer.MAX_VALUE) % numReduceTasks;
  }

}
```

## 写入环形缓冲区

因为频繁的磁盘I/O操作会严重的降低效率，因此“中间结果”不会立马写入磁盘，而是优先存储到map节点的“环形内存缓冲区”，并做一些预排序以提高效率，当写入的数据量达到预先设置的阙值后便会执行一次I/O操作将数据写入到磁盘。

每个map任务都会分配一个环形内存缓冲区，用于存储map任务输出的键值对（默认大小100MB，`mapreduce.task.io.sort.mb`调整）以及对应的partition，被缓冲的（key,value）对已经被序列化（为了写入磁盘）。

## 执行溢写出

一旦缓冲区内容达到阈值（mapreduce.map.io.sort.spill.percent,默认0.80，或者80%），就会会锁定这80%的内存，并在每个分区中对其中的键值对按键进行sort排序。

具体是将数据按照partition和key两个关键字进行排序，排序结果为缓冲区内的数据按照partition为单位聚集在一起，同一个partition内的数据按照key有序。

排序完成后会创建一个溢出写文件（临时文件），然后开启一个后台线程把这部分数据以一个临时文件的方式溢出写（spill）到本地磁盘中（如果客户端自定义了Combiner（相当于map阶段的reduce），则会在分区排序后到溢写出前自动调用combiner，将相同的key的value相加，这样的好处就是减少溢写到磁盘的数据量。这个过程叫“合并”）。

剩余的20%的内存在此期间可以继续写入map输出的键值对。溢出写过程按轮询方式将缓冲区中的内容写到`mapreduce.cluster.local.dir`属性指定的目录中。


## Sort

```java
public interface WritableComparable<T> extends Writable, Comparable<T> {
}

Comparable：
public int compareTo(T o);
```

按Key排序：

```java
@Override
  public int compareTo(Key other) {
    //字节长度差，如果长的，就直接在前面
    int result = this.bytes.length - other.getBytes().length;
    
    //长度一样，每个字节相比
    for (int i = 0; result == 0 && i < bytes.length; i++) {
      result = this.bytes[i] - other.bytes[i];
    }
    
    //全部一样，看权重
    if (result == 0) {
      result = Double.valueOf(this.weight - other.weight).intValue();
    }
    return result;
  }
```

## Group

分组是将具有相同key的values放置在一起

## Combine

Combine很简单，就一句话，Combine做的工作和Reduce是一样的。所以他也继承Reducer类，完成reduce方法，要求输出k-v类型等于Reducer输入k-v类型

程序中通过类似`job.setCombinerClass(IntSumReducer.class)`;指定Combiner类.

如果指定了Combiner，可能在两个地方被调用： 

1. 当为作业设置Combiner类后，缓存溢出线程将缓存存放到磁盘时，就会调用； 
2. 缓存溢出的数量超过`mapreduce.map.combine.minspills`（默认3）时，在缓存溢出文件合并的时候会调用

# 2. Reduce端shuffle

* 复制copy
* 归并merge
* reduce

Ref：https://blog.csdn.net/ASN_forever/article/details/81233547?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task

