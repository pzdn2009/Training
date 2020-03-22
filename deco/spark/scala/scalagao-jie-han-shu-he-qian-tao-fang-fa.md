# Scala-高阶函数和嵌套方法

# 1. 高阶函数

高阶函数是指使用其他函数作为**参数**、或者**返回**一个函数作为结果的函数。

在Scala中函数是“一等公民”，所以允许定义高阶函数。

* 作为参数
* 作为返回值

```scala
object SalaryRaiser {

  //促销：输入salaries，以及一个促销函数Double => Double
  //返回：map过的结果
  private def promotion(salaries: List[Double], promotionFunction: Double => Double): List[Double] =
    salaries.map(promotionFunction)

  def smallPromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, salary => salary * 1.1)

  def bigPromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, salary => salary * math.log(salary))

  def hugePromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, salary => salary * salary)
}
```

Eg2：
```scala
//返回一个函数(String,String)=>String
//两个入参，一个String返回
def urlBuilder(ssl: Boolean, domainName: String): (String, String) => String = {
  val schema = if (ssl) "https://" else "http://"
  (endpoint: String, query: String) => s"$schema$domainName/$endpoint?$query"
}

val domainName = "www.example.com"
def getURL = urlBuilder(ssl=true, domainName)
val endpoint = "users"
val query = "id=1"
val url = getURL(endpoint, query) // "https://www.example.com/users?id=1": String
```

# 2. 嵌套方法

```scala
//定义外部方法
def factorial(x: Int): Int = {
    //定义内部方法
    def fact(x: Int, accumulator: Int): Int = {
      if (x <= 1) accumulator
      else fact(x - 1, x * accumulator)
    }  
    //调用内部方法
    fact(x, 1)
 }

 println("Factorial of 2: " + factorial(2))
 println("Factorial of 3: " + factorial(3))
```