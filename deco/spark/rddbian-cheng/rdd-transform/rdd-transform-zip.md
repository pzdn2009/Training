# zip

Joins two RDDs by combining the i-th of either partition with each other. 

The resulting RDD will consist of two-component tuples which are interpreted as key-value pairs by the methods provided by the PairRDDFunctions extension.

将两个RDD，合并一个新的RDD，以元组的形式出现

```scala
def zip[U: ClassTag](other: RDD[U]): RDD[(T, U)]
```

```scala

val a = sc.parallelize(1 to 100, 3)
val b = sc.parallelize(101 to 200, 3)
a.zip(b).collect

res1: Array[(Int, Int)] = Array((1,101), (2,102), (3,103), (4,104), (5,105), (6,106), (7,107),...

val a = sc.parallelize(1 to 100, 3)
val b = sc.parallelize(101 to 200, 3)
val c = sc.parallelize(201 to 300, 3)
//(a,b).zip(c) == ((a,b),c)
//map (a,b,c)
a.zip(b).zip(c).map((x) => (x._1._1, x._1._2, x._2 )).collect
res12: Array[(Int, Int, Int)] = Array((1,101,201), (2,102,202), (3,103,203), (4,104,204), (5,105,205), (6,106,206)...
```