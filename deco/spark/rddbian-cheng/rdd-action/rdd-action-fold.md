# fold

Aggregates the values of each partition. 
聚合每个分区的值。

The aggregation variable within each partition is initialized with zeroValue.
需要给一个初始值。

```
def fold(zeroValue: T)(op: (T, T) => T): T
```

```scala
val a = sc.parallelize(List(1,2,3), 3)
a.fold(0)(_ + _)
res59: Int = 6
```