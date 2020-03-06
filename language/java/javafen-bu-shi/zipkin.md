# Zipkin

```java
wget -O zipkin.jar  'https://search.maven.org/remote_content?g=io.zipkin.java&a=zipkin-server&v=LATEST&c=exec'  

java -jar zipkin.jar
```

9411為默認端口

## zipkin的架构与核心概念

将数据发送到zipkin的已检测应用程序中的组件称为**Reporter**。

它通过几种传输方式之一将跟踪数据发送到zipkin收集器，zipkin收集器将跟踪数据保存到存储器。

稍后，存储由API查询以向UI提供数据。为了保证服务的调用链路跟踪，zipkin使用传输ID，例如，当正在进行跟踪操作并且它需要发出传出http请求时，会添加一些headers信息以传播ID，但它不能用于发送详细信息（操作名称、数据等）。

### A、Span

基本工作单元，一次链路调用创建一个span，通过一个64位ID标识它，span通过还有其他的数据，例如描述信息，时间戳，key-value对的\(Annotation\)tag信息，parent-id等,其中parent-id 可以表示span调用链路来源，通俗的理解span就是一次请求信息。

### B、 Trace

类似于树结构的Span集合，表示一条调用链路，存在唯一标识。

### C、 Annotation

注解,用来记录请求特定事件相关信息\(例如时间\)，通常包含四个注解信息： （1）cs - ClientStart,表示客户端发起请求 （2）sr - Server Receive,表示服务端收到请求 （3）ss - Server Send,表示服务端完成处理，并将结果发送给客户端 （4）cr - Client Received,表示客户端获取到服务端返回信息

### D、 Transport

```text
    收集被trace的services的spans，并且传输给zipkin的collector，有三个主要传输：HTTP，Kafka和Scribe。
```

### E、 Collector

```text
   zipkincollector会对一个到来的被trace的数据（span）进行验证、存储并设置索引。
```

### F、 Storage

```text
    存储，zipkin默认的存储方式为in-memory，即不会进行持久化操作。如果想进行收集数据的持久化，可以存储数据在Cassandra，因为Cassandra是可扩展的，有一个灵活的模式，并且在Twitter中被大量使用，我们使这个组件可插入。除了Cassandra，我们原生支持ElasticSearch和MySQL。其他后端可能作为第三方扩展提供。
```

### G、QueryService

```text
    一旦数据被存储和索引，我们需要一种方法来提取它。查询守护程序提供了一个用于查找和检索跟踪的简单JSON API，此API的主要使用者是WebUI。
```

### H、 WebUI

```text
    展示页面，提供了一个漂亮的界面来查看痕迹。 Web UI提供了一种基于服务，时间和注释查看trace的方法（通过Query Service）。注意：在UI中没有内置的身份验证。
```

Ref：[http://blog.csdn.net/qq\_21387171/article/details/53787019](http://blog.csdn.net/qq_21387171/article/details/53787019)

