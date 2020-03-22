# RDD編程

* 转换
* 动作
* 缓存
* 共享变量

RDD實際上是（immutable）不可變的分佈式對象集合。

RDD支持兩種類型的操作：**transformation** & **action**。

## 1. 創建

抽象类，只能通过工厂方法创建。

* `parallelize`：从本地scala集合创建RDD实例。对集合中的数据重新分区、重新分布，然后返回一个代表这些数据的RDD。
* `textFile`：从文本文件创建RDD。
* `wholeTextFiles`：读取目录下的所有文本文件，然后返回一个键值的RDD。（文件路径，文件内容）。
* `sequenceFile`：

```scala
val lines = sc.parallelize(List("pandas","i like pandas"))
val lines = sc.textFile("/path/to/README.md")


val data = Array(1, 2, 3, 4, 5)
val distData = sc.parallelize(data)
```

# 2. 缓存

* cache
* persist
 * MEMORY_ONLY
 * DISK_ONLY
 * MEMORY_AND_DISK
 * MEMROY_OLY_SER
 * MEMORY_AND_DISK_SER

