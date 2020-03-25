# count

# 1. count

Returns the number of items stored within a RDD.

```
def count(): Long
val c = sc.parallelize(List("Gnu", "Cat", "Rat", "Dog"), 2)
c.count

res2: Long = 4
```
# 2. countByValue

按照值统计个数。

Returns a map that contains all unique values of the RDD and their respective occurrence counts.

  (Warning: This operation will finally aggregate the information in a single reducer.)

```scala
//T 值，Long 个数，最终返回一个Map
def countByValue(): Map[T, Long]

val b = sc.parallelize(List(1,2,3,4,5,6,7,8,2,4,2,1,1,1,1,1))
b.countByValue
res27: scala.collection.Map[Int,Long] = Map(5 -> 1, 8 -> 1, 
3 -> 1, 6 -> 1, 1 -> 6, 2 -> 3, 4 -> 2, 7 -> 1)
```