# Scala-运算符和传名参数

# 1. 运算符

```scala
//Vec类
case class Vec(x: Double, y: Double) {
  //定义运算符
  def +(that: Vec) = Vec(this.x + that.x, this.y + that.y)
}

val vector1 = Vec(1.0, 1.0)
val vector2 = Vec(2.0, 2.0)

val vector3 = vector1 + vector2
vector3.x  // 3.0
vector3.y  // 3.0
```