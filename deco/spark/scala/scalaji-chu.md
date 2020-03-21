# Scala-基础

https://scalafiddle.io/

* 表达式
* 常量：val
* 变量：var
* 代码块：{}，最后一条语句即值。
* 函数：表达式
 * 匿名函数
 * 多参数
* 方法：由def关键字定义。def后面跟着一个名字、参数列表、返回类型和方法体。
`def addThenMultiply(x: Int, y: Int)(multiplier: Int): Int = (x + y) * multiplier`
* 类
 * new
* 样例类：样例类一般用于不可变对象，并且可作值比较。
 * 不用new
 * 类似结构体
 * ==直接比较
* 对象object
 * 单例
 * 静态
* 特质
 * 接口
 * default实现
 * 实现类，使用`overrid def`
* 主方法
 * object 
 * `def main(args: Array[String]): Unit =`


# 统一类型

* Any——类似JAVA Object
* AnyVal——值类型
  * `Double、Float、Long、Int、Short、Byte、Char、Unit和Boolean`
  * Unit类似Void
  
* AnyRef——引用类型
* 转换是单向的：
 * Byte-Short-Int-Long-Float-Double
 * Char-Int
* Nothing：所有类型的子类型，也称为底部类型。它的用途之一是给出非正常终止的信号，如抛出异常、程序退出或者一个无限循环
* Null：是所有引用类型的子类型（即AnyRef的任意子类型）。它有一个单例值由关键字null所定义。Null主要是使得Scala满足和其他JVM语言的互操作性，但是几乎不应该在Scala代码中使用。

```scala
val list: List[Any] = List(
  "a string",
  732,  // an integer
  'c',  // a character
  true, // a boolean value
  () => "an anonymous function returning a string"
)

list.foreach(element => println(element))
```
