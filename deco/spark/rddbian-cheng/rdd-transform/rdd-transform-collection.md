# collection

# 1. cartesian

Computes the cartesian product between two RDDs (i.e. Each item of the first RDD is joined with each item of the second RDD) and returns them as a new RDD. 

笛卡尔积，同类型，元组。

```scala
def cartesian[U: ClassTag](other: RDD[U]): RDD[(T, U)]


val x = sc.parallelize(List(1,2,3,4,5))
val y = sc.parallelize(List(6,7,8,9,10))
x.cartesian(y).collect
res0: Array[(Int, Int)] = Array((1,6), (1,7), (1,8), (1,9), (1,10), (2,6), (2,7), (2,8), (2,9), (2,10), (3,6), (3,7), (3,8), (3,9), (3,10), (4,6), (5,6), (4,7), (5,7), (4,8), (5,8), (4,9), (4,10), (5,9), (5,10))
```

# 2. union, ++

Performs the standard set operation: A union B


并集

```scala
def ++(other: RDD[T]): RDD[T]
def union(other: RDD[T]): RDD[T]


val a = sc.parallelize(1 to 3, 1)
val b = sc.parallelize(5 to 7, 1)
(a ++ b).collect
res0: Array[Int] = Array(1, 2, 3, 5, 6, 7)
```

# 3. intersection

Returns the elements in the two RDDs which are the same.


交集

```scala
def intersection(other: RDD[T], numPartitions: Int): RDD[T]
def intersection(other: RDD[T], partitioner: Partitioner)(implicit ord: Ordering[T] = null): RDD[T]
def intersection(other: RDD[T]): RDD[T]


val x = sc.parallelize(1 to 20)
val y = sc.parallelize(10 to 30)
val z = x.intersection(y)

z.collect
res74: Array[Int] = Array(16, 12, 20, 13, 17, 14, 18, 10, 19, 15, 11)
```

# 4. distinct

Returns a new RDD that contains each unique value only once.

```
def distinct(): RDD[T]
//指定分区数
def distinct(numPartitions: Int): RDD[T]


val c = sc.parallelize(List("Gnu", "Cat", "Rat", "Dog", "Gnu", "Rat"), 2)
c.distinct.collect
res6: Array[String] = Array(Dog, Gnu, Cat, Rat)

val a = sc.parallelize(List(1,2,3,4,5,6,7,8,9,10))
a.distinct(2).partitions.length
res16: Int = 2

a.distinct(3).partitions.length
res17: Int = 3
```

