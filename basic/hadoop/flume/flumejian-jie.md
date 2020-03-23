# Flume简介

Flume 是Apache旗下的一款开源、高可靠、高扩展、容易管理、支持客户扩展的数据采集系统。 Flume使用JRuby来构建，所以依赖Java运行环境。

Ref：https://blog.csdn.net/qq_29229567/article/details/88029001

![](https://img-blog.csdnimg.cn/20190228145500742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI5MjI5NTY3,size_16,color_FFFFFF,t_70)

每一个agent都由Source，Channel和Sink组成。

* Source：Source负责接收输入数据，并将数据写入管道。Flume的Source支持HTTP，JMS，RPC，NetCat，Exec，Spooling Directory。其中Spooling支持监视一个目录或者文件，解析其中新生成的事件。

* Channel：Channel 存储，缓存从source到Sink的中间数据。可使用不同的配置来做Channel，例如内存，文件，JDBC等。使用内存性能高但不持久，有可能丢数据。使用文件更可靠，但性能不如内存。

* Sink：Sink负责从管道中读出数据并发给下一个Agent或者最终的目的地。Sink支持的不同目的地种类包括：HDFS，HBASE，Solr，ElasticSearch，File，Logger或者其它的Flume Agent。


Flume在source和sink端都使用了transaction机制保证在数据传输中没有数据丢失。

Source上的数据可以复制到不同的通道上。每一个Channel也可以连接不同数量的Sink。这样连接不同配置的Agent就可以组成一个复杂的数据收集网络。通过对agent的配置，可以组成一个路由复杂的数据传输网络。

Flume中传输的内容定义为事件(Event)，事件由Headers(包含元数据，Meta Data)和Payload组成。
