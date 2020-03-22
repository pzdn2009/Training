# keyBy

```scala
//apply一个函数，函数的值作为_1,item作为_2
def keyBy[K](f: T => K): RDD[(K, T)]
```

示例：

```scala
val a = sc.parallelize(List("dog", "salmon", "salmon", "rat", "elephant"), 3)
val b = a.keyBy(_.length)
b.collect

res26: Array[(Int, String)] = Array((3,dog), (6,salmon), (6,salmon), (3,rat), (8,elephant))

```