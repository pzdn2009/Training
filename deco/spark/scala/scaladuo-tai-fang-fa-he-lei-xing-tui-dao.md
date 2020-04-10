# Scala-多态方法和类型推导

# 1. 多态方法

```scala
//泛型方法[A]
//A：类型参数
//x：值参数
def listOfDuplicates[A](x: A, length: Int): List[A] = {
  if (length < 1)
    Nil
  else
    x :: listOfDuplicates(x, length - 1)
}
println(listOfDuplicates[Int](3, 4))  // List(3, 3, 3, 3)
println(listOfDuplicates("La", 8))  // List(La, La, La, La, La, La, La, La)
```

* `::` 表示将左侧的元素添加到右侧的列表中。

* 上例中第一次调用方法时，我们显式地提供了`类型参数 [Int]`。 因此第一个参数必须是 Int 类型，并且返回类型为 List[Int]。
* 第二次，"La" 是一个 String，因此编译器知道 A 必须是 String。

# 2. 类型推导

```scala
//字符串
val businessName = "Montreux Jazz Café"

//编译器可以推断出方法的返回类型为 Int，因此不需要明确地声明返回类型
def squareOf(x: Int) = x * x

case class MyPair[A, B](x: A, y: B)
val p = MyPair(1, "scala") // type: MyPair[Int, String]

def id[T](x: T) = x
val q = id(1)              // type: Int

Seq(1, 3, 4).map(x => x * 2)  // List(2, 6, 8)
```

>通常认为，公开可访问的 API 成员应该具有显示类型声明以增加可读性。 因此，我们建议你将代码中向用户公开的任何 API 明确指定类型。