
# 1. zip
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

# 2. zipWithIndex

Zips the elements of the RDD with its element indexes. The indexes start from 0. 

If the RDD is spread across multiple partitions then a spark Job is started to perform this operation.


```scala
def zipWithIndex(): RDD[(T, Long)]
```

```scala
val z = sc.parallelize(Array("A", "B", "C", "D"))
val r = z.zipWithIndex
res110: Array[(String, Long)] = Array((A,0), (B,1), (C,2), (D,3))

val z = sc.parallelize(100 to 120, 5)
val r = z.zipWithIndex
r.collect
res11: Array[(Int, Long)] = Array((100,0), (101,1), (102,2), (103,3), (104,4), (105,5), (106,6), (107,7), (108,8), (109,9), (110,10), (111,11), (112,12), (113,13), (114,14), (115,15), (116,16), (117,17), (118,18), (119,19), (120,20))
```

# 3. zipWithUniqueId

This is different from zipWithIndex since just gives a unique id to each data element but the ids may not match the index number of the data element. 

This operation does not start a spark job even if the RDD is spread across multiple partitions.


Compare the results of the example below with that of the 2nd example of zipWithIndex. You should be able to see the difference.

```scala
def zipWithUniqueId(): RDD[(T, Long)]

val z = sc.parallelize(100 to 120, 5)
val r = z.zipWithUniqueId
r.collect

res12: Array[(Int, Long)] = Array((100,0), (101,5), (102,10), (103,15), (104,1), (105,6), (106,11), (107,16), (108,2), (109,7), (110,12), (111,17), (112,3), (113,8), (114,13), (115,18), (116,4), (117,9), (118,14), (119,19), (120,24))
```