# Flink-启动

# 1. word-count

位置：/flink-examples/flink-examples-batch/src/main/java/org/apache/flink/examples/java/wordcount/WordCount.java

>1. set env;
2. read data;
3. transformation;
4. sink;

# 2. 开发环境

java依赖：
```xml
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-java</artifactId>
  <version>1.10.0</version>
</dependency>
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-streaming-java_2.11</artifactId>
  <version>1.10.0</version>
</dependency>
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-clients_2.11</artifactId>
  <version>1.10.0</version>
</dependency>
```

scala依赖：scala_2.11

使用模板生成项目模板：
```
curl https://flink.apache.org/q/quickstart-scala.sh | bash -s 1.10.0

curl https://flink.apache.org/q/quickstart.sh | bash -s 1.10.0
```

连接器：
```xml
<dependency>
    <groupId>org.apache.flink</groupId>
    <artifactId>flink-connector-kafka-0.10_2.11</artifactId>
    <version>1.10.0</version>
</dependency>
```

Hadoop：
一般规则：永远不需要将 Hadoop 依赖项直接添加到你的应用程序中 

