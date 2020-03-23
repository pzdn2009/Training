# Scala-单例对象和正则表达式

# 1. 单例对象

* object
* 静态导入
* 伴生对象，伴生类
* 静态转发：JAVA调用Scala

当一个单例对象和某个类共享一个名称时，这个单例对象称为 伴生对象。 

同理，这个类被称为是这个单例对象的伴生类。

类和它的伴生对象可以互相访问其私有成员。

使用伴生对象来定义那些在伴生类中不依赖于实例化对象而存在的成员变量或者方法。


```scala
//伴生类
class Email(val username: String, val domainName: String)

//伴生对象
object Email {
  def fromString(emailString: String): Option[Email] = {
    emailString.split('@') match {
      case Array(a, b) => Some(new Email(a, b))
      case _ => None
    }
  }
}

//static方式调用
val scalaCenterEmail = Email.fromString("scala.center@epfl.ch")
//模式匹配
scalaCenterEmail match {
  case Some(email) => println(
    s"""Registered an email
       |Username: ${email.username}
       |Domain name: ${email.domainName}
     """)
  case None => println("Error: could not parse email")
}
```

在 Java 中 `static` 成员对应于 Scala 中的`伴生对象的普通成员`。

在 Java 代码中调用伴生对象时，伴生对象的成员会被定义成伴生类中的 static 成员。这称为 `静态转发`。这种行为发生在当你自己没有定义一个伴生类时。

# 2. 正则表达式

* `scala.util.matching.Regex`
* `"".r`
* `findFirstMatchIn`
* `findAllMatchIn`
* `group(1)`，捕获

```scala
//导入包
import scala.util.matching.Regex

//定义模式
val keyValPattern: Regex = "([0-9a-zA-Z-#() ]+): ([0-9a-zA-Z-#() ]+)".r

//多行字符串输入
val input: String =
  """background-color: #A03300;
    |background-image: url(img/header100.png);
    |background-position: top center;
    |background-repeat: repeat-x;
    |background-size: 2160px 108px;
    |margin: 0;
    |height: 108px;
    |width: 100%;""".stripMargin

//遍历所有匹配，使用group来获取Capture
for (patternMatch <- keyValPattern.findAllMatchIn(input))
  println(s"key: ${patternMatch.group(1)} value: ${patternMatch.group(2)}")
  ```