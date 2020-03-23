# RDD-action

* collect()  
* count
* countByValue
* take(num)  
* top(num) 
* takeOrdered\(num\)\(ordering\)  
* takeSample\(withReplacement,num,\[seed\]\)  
* [reduce](/deco/spark/rddbian-cheng/rdd-action/rdd-action-reduce.md)(func)  
* [fold](/deco/spark/rddbian-cheng/rdd-action/rdd-action-fold.md)(zero)(func)  
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

