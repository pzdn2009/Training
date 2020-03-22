# Scala-案例类和模式匹配

# 1. 案例类

* 不可变
* 无new：因为案例类有一个默认的apply方法来负责对象的创建
* public
* 比较
 * 值类型
 * ==
* 拷贝：copy

Eg1：
```scala
//定义案例类，三个参数
case class Message(sender: String, recipient: String, body: String)

//对象
val message2 = Message("jorge@catalonia.es", "guillaume@quebec.ca", "Com va?")
//对象
val message3 = Message("jorge@catalonia.es", "guillaume@quebec.ca", "Com va?")

//比较
val messagesAreTheSame = message2 == message3  // true
```

Eg2：
```scala
//声明
case class Message(sender: String, recipient: String, body: String)
val message4 = Message("julien@bretagne.fr", "travis@washington.us", "Me zo o komz gant ma amezeg")
//拷贝，只改变一个值
val message5 = message4.copy(sender = message4.recipient, recipient = "claire@bourgogne.fr")
message5.sender  // travis@washington.us
message5.recipient // claire@bourgogne.fr
message5.body  // "Me zo o komz gant ma amezeg"
```
# 2. 模式匹配

模式匹配是检查某个值（value）是否匹配某一个模式的机制，一个成功的匹配同时会将匹配值解构为其组成部分。它是Java中的switch语句的升级版，同样可以用于替代一系列的 if/else 语句。

* `x match { case* }`
* 守卫模式 `case X if <bool expression>`
* 仅匹配类型 `case t: Type => exprs`

Eg：
```scala
def showNotification(notification: Notification): String = {
  
  //进行模式匹配
  notification match {
    //匹配到EMAIL类型
    case Email(sender, title, _) =>
      s"You got an email from $sender with title: $title"
    //短信类型
    case SMS(number, message) =>
      s"You got an SMS from $number! Message: $message"
    //语音
    case VoiceRecording(name, link) =>
      s"you received a Voice Recording from $name! Click the link to hear it: $link"
  }
}

//守卫
def showImportantNotification(notification: Notification, importantPeopleInfo: Seq[String]): String = {
  notification match {
    //指定匹配发件人
    case Email(sender, _, _) if importantPeopleInfo.contains(sender) =>
      "You got an email from special someone!"
    case SMS(number, _) if importantPeopleInfo.contains(number) =>
      "You got an SMS from special someone!"
    case other =>
      showNotification(other) // nothing special, delegate to our original showNotification function
  }
}

val someSms = SMS("12345", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")

println(showNotification(someSms))  
// prints You got an SMS from 12345! Message: Are you there?

println(showNotification(someVoiceRecording))
//// you received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

Eg2：
```scala
abstract class Device
case class Phone(model: String) extends Device {
  def screenOff = "Turning screen off"
}
case class Computer(model: String) extends Device {
  def screenSaverOn = "Turning screen saver on..."
}

def goIdle(device: Device) = device match {
  case p: Phone => p.screenOff
  case c: Computer => c.screenSaverOn
}
```
