# RDD編程

RDD實際上是（immutable）不可變的分佈式對象集合。

RDD支持兩種類型的操作：**transformation** & **action**。

* 创建
* 转换
* 动作
* 缓存

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

## 2. Transformation

* map：高阶，每个元素，返回新RDD。
* flatMap：高阶，每个元素返回一个序列，扁平化，返回新RDD。
* mapPartitions：高阶，
* filter：高阶，布尔函数，每个元素，返回新RDD。
* sample\(withReplacement,fraction,\[seed\]\)
* union：并集
* distinct：去重
* intersection：交集
* substract：差集
* cartesian：笛卡尔积
* zip：
* zipWithIndex:
* groupBy
* keyBy
* sortBy
* pipe
* sample


## 3. Action

* collect()  
* count
* countByValue
* take\(num\)  
* top\(num\) 
* takeOrdered\(num\)\(ordering\)  
* takeSample\(withReplacement,num,\[seed\]\)  
* reduce\(func\)  
* fold\(zeor\)\(func\)  
* aggregate\(zeroValue\)\(seqOp,comOp\)  
* foreach\(func\)
* first
* max
* min

```scala
scala> rdd.takeSample(false,1)
res23: Array[Int] = Array(3)

scala> rdd.takeSample(false,1)
res24: Array[Int] = Array(3)

scala> rdd.takeSample(false,1)
res25: Array[Int] = Array(3)

scala> rdd.takeSample(false,1)
res26: Array[Int] = Array(2)

scala> rdd.takeSample(false,1)
res27: Array[Int] = Array(3)

scala> rdd.takeSample(false,1)
res28: Array[Int] = Array(1)

scala> rdd.reduce((x,y)=>x+y)
res29: Int = 9

scala> rdd.fold(0)((x,y)=>x+y)
res30: Int = 9

scala> rdd.fold(1)((x,y)=>x+y)
res31: Int = 12

scala> rdd.fold(2)((x,y)=>x+y)
res32: Int = 15

scala> rdd.fold(5)((x,y)=>x+y)
res33: Int = 24

scala> rdd.aggregate((0,0))((x,y)=>(x._1+y,x._2+1),(x,y)=>(x._1+y._1,x._2+y._2))
res34: (Int, Int) = (9,4)
```

# 4. 缓存

* cache
* persist
 * MEMORY_ONLY
 * DISK_ONLY
 * MEMORY_AND_DISK
 * MEMROY_OLY_SER
 * MEMORY_AND_DISK_SER

