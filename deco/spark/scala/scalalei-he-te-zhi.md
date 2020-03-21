# Scala-类和特质

# 1. 类

* class
* new
* 构造器
* 重写
* 成员变量
* 默认参数，命名参数
* Getter/Setter
* 参数
 * val：不可变
 * var：可变
 * 不带修饰：私有
 
Eg1：
```scala
// 定义构造器，指定为[var p: Type, ...]
class Point(var x: Int, var y: Int) {

  def move(dx: Int, dy: Int): Unit = {
    x = x + dx
    y = y + dy
  }
  
  // 重写Any.toString
  override def toString: String =
    s"($x, $y)"
}

//new创建实例
val point1 = new Point(2, 3)
point1.x  // 2
println(point1)  // prints (2, 3)
```

Eg2：
```scala
//默认参数
class Point(var x: Int = 0, var y: Int = 0)
//命名参数
val point2 = new Point(y=2)
println(point2.y)  // prints 2
```

Eg3：
```scala
class Point {
  private var _x = 0
  private var _y = 0
  //类似final
  private val bound = 100

  //Getter
  def x = _x
  //Setter
  def x_= (newValue: Int): Unit = {
    if (newValue < bound) _x = newValue else printWarning
  }

  def y = _y
  def y_= (newValue: Int): Unit = {
    if (newValue < bound) _y = newValue else printWarning
  }
 
  //私有方法
  private def printWarning = println("WARNING: Out of bounds")
}

val point1 = new Point
point1.x = 99
point1.y = 101 // prints the warning
```

# 2. 特质

类似java8接口
* trait
* 支持泛型[A]
* 支持默认实现

```scala
import scala.collection.mutable.ArrayBuffer

trait Pet {
  //抽象字段
  val name: String
}

//构造函数实现name
class Cat(val name: String) extends Pet
class Dog(val name: String) extends Pet

val dog = new Dog("Harry")
val cat = new Cat("Sally")

val animals = ArrayBuffer.empty[Pet]
animals.append(dog)
animals.append(cat)
animals.foreach(pet => println(pet.name))  // Prints Harry Sally
```

