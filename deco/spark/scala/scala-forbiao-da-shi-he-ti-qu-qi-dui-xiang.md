# Scala-For表达式和提取器对象

# 1. For表达式

* 序列推导
 * 格式：`for (enumerators) yield e`
 * enumerator：生成器或者过滤器
 * e：新值
 * 结果：产生一个新的序列

Scala 提供一个轻量级的标记方式用来表示 **序列推导**。推导使用形式为 `for (enumerators) yield e` 的 for 表达式，此处 enumerators 指一组以分号分隔的枚举器。

一个 enumerator 要么是一个产生新变量的**生成器**，要么是一个**过滤器**。for 表达式在枚举器产生的每一次绑定中都会计算 e 值，并在循环结束后**返回这些值组成的序列**。

Eg1：

```scala
//样例类
case class User(name: String, age: Int)

//List
val userBase = List(User("Travis", 28),
  User("Kelly", 33),
  User("Jennifer", 44),
  User("Dennis", 23))

//过滤器 if
val twentySomethings = for (user <- userBase if (user.age >=20 && user.age < 30))
  yield user.name  // i.e. add this to a list

twentySomethings.foreach(name => println(name))  // prints Travis Dennis
```

Eg2：

```scala
def foo(n: Int, v: Int) =
   //两个枚举器
   for (i <- 0 until n;
        j <- i until n if i + j == v)
   yield (i, j)

foo(10, 10) foreach {
  case (i, j) =>
    println(s"($i, $j) ")  // prints (1, 9) (2, 8) (3, 7) (4, 6) (5, 5)
}
```


注意 for 表达式并不局限于使用列表。任何数据类型只要支持 withFilter，map，和 flatMap 操作（不同数据类型可能支持不同的操作）都可以用来做序列推导。

>你可以在使用 for 表达式时省略 yield 语句。此时会返回 Unit。

# 2. 提取器对象

* 提取器对象是一个包含有 `unapply` 方法的单例对象。
* `apply` 方法就像一个构造器，接受参数然后创建一个实例对象，
* 反之 `unapply` 方法接受一个实例对象然后返回最初创建它所用的参数。
* 提取器常用在模式匹配和**偏函数**中。

```scala
import scala.util.Random
//单例对象
object CustomerID {

  //构造
  def apply(name: String) = s"$name--${Random.nextLong}"

  //返回name:String
  def unapply(customerID: String): Option[String] = {
    val stringArray: Array[String] = customerID.split("--")
    if (stringArray.tail.nonEmpty) Some(stringArray.head) else None
  }
}

//等价于CustomerID.apply("Sukyoung")
val customer1ID = CustomerID("Sukyoung")  // Sukyoung--23098234908
customer1ID match {
  case CustomerID(name) => println(name)  // prints Sukyoung
  case _ => println("Could not extract a CustomerID")
}
```