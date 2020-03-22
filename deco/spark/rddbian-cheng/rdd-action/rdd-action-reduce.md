# reduce

This function provides the well-known reduce functionality in Spark.

Please note that any function f you provide, should be commutative（可交换的） in order to generate reproducible （可重现的）results.


```scala
//输入两个通类型的元素，返回一个同类型的元素
//Eg: sum
def reduce(f: (T, T) => T): T
```

示例：

```scala
val a = sc.parallelize(1 to 100, 3)
a.reduce(_ + _)

res41: Int = 5050
```