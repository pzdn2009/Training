![](/assets/hd6.png)

# 1. 服务端组件

* **Driver组件**：该组件包括Complier、Optimizer和Executor，它的作用是将HiveQL（类似SQL）语句进行解析、编译优化，生成执行计划，然后调用底层的mapreduce计算框架；
* **Metastore组件**：元数据服务组件，这个组件存储Hive的元数据，Hive的元数据存储在关系数据库里，Hive支持的关系数据库有`derby`和`mysql`。元数据对于Hive十分重要，因此Hive支持把metastore服务独立出来，安装到远程的服务器集群里，从而解耦Hive服务和metastore服务，保证Hive运行的健壮性；
* **Thrift服务**：thrift是facebook开发的一个软件框架，它用来进行可扩展且跨语言的服务的开发，Hive集成了该服务，能让不同的编程语言调用hive的接口。

# 2. 客户端组件

* CLI：command line interface，命令行接口。
* Thrift客户端：上面的架构图里没有写上Thrift客户端，但是Hive架构的许多客户端接口是建立在thrift客户端之上，包括`JDBC和ODBC接口`。
* WEBGUI：Hive客户端提供了一种通过网页的方式访问hive所提供的服务。这个接口对应Hive的`hwi`组件（hive web interface），使用前要启动hwi服务。

## 2.1 交互方式
* CLI
* JDBC/ODBC
* Hive命令：`hive -e 'sql'`
* hiveserver2：启动`hiveserver2`，通过`beeline` 远程连接

