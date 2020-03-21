# Scala-元组和泛型类

# 1. 元组

元组是一个可以容纳不同类型元素的**类**。元组是**不可变**的。

当我们需要从函数返回多个值时，元组会派上用场。

Scala 中的元组包含一系列类：Tuple2，Tuple3等，直到 Tuple22。

* 类型指定
* 访问
* 解构
* for
* 模式匹配

```scala
//创建元组，并指定类型
val ingredient = ("Sugar" , 25):Tuple2[String, Int]

//访问 
println(ingredient._1) // Sugar
println(ingredient._2) // 25

//解构
val (name, quantity) = ingredient
println(name) // Sugar
println(quantity) // 25


val planetDistanceFromSun = List(("Mercury", 57.9), ("Venus", 108.2), ("Earth", 149.6 ), ("Mars", 227.9), ("Jupiter", 778.3))

planetDistanceFromSun.foreach{ tuple => {
  //模式匹配  
  tuple match {
    
      case ("Mercury", distance) => println(s"Mercury is $distance millions km far from Sun")
      
      case p if(p._1 == "Venus") => println(s"Venus is ${p._2} millions km far from Sun")
      
      case p if(p._1 == "Earth") => println(s"Blue planet is ${p._2} millions km far from Sun")
      
      case _ => println("Too far....")
      
    }
  } 
}


val numPairs = List((2, 5), (3, -7), (20, 56))

//for语句
for ((a, b) <- numPairs) {
  println(a * b)
}
```

>类型 Unit 的值 () 在概念上与类型 Tuple0 的值 () 相同。 Tuple0 只能有一个值，因为它没有元素。

# 2. 泛型类

* 使用[A]
* new 指定具体，new Stack[Int]

```scala
//泛型类
class Stack[A] {
  private var elements: List[A] = Nil
  def push(x: A) { elements = x :: elements }
  def peek: A = elements.head
  def pop(): A = {
    val currentTop = peek
    elements = elements.tail
    currentTop
  }
}
```