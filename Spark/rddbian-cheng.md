# RDD編程

RDD實際上是（immutable）不可變的分佈式對象集合。

RDD支持兩種類型的操作：transformation & action。

# 1. 創建

```scala
scala> val lines = sc.parallelize(List("pandas","i like pandas"))
scala> val lines = sc.textFile("/path/to/README.md")
```

# 2. Transformation

* map
* flatMap
* filter
* sample\(withReplacement,fraction,\[seed\]\)

* union

* distinct
* intersection
* substract
* cartesian

# 3. Action

- collect\(\)  
- count\(\)  
- countByValue\(\)  
- take\(num\)  
- top\(num\)  
- takeOrdered\(num\)\(ordering\)  
- takeSample\(withReplacement,num,\[seed\]\)  
- reduce\(func\)  
- fold\(zeor\)\(func\)  
- aggregate\(zeroValue\)\(seqOp,comOp\)  
- foreach\(func\)

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

# 4. 持久化（緩存）  
- persist

# 5. Pair RDD  
- reduceByKey  
- groupByKey  
- combinByKey  
- mapValues  
- flatMapValues  
- keys  
- values  
- sortByKey

- subtractByKey  
- join  
- rightOuterJoin  
- leftOuterJoin  
- cogroup

- countByKey  
- collectAsMap  
- lookup

