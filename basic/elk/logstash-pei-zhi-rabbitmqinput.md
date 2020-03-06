# Logstash配置rabbitmq input

## Logstash配置rabbitmq input

场景：当使用rmq作为数据源输入时，则可以使用rabbitmq输入插件。

官方文档：[https://www.elastic.co/guide/en/logstash/current/plugins-inputs-rabbitmq.html\#plugins-inputs-rabbitmq-password](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-rabbitmq.html#plugins-inputs-rabbitmq-password)

## 1. 主要字段

* ack：是否消息自动确认
* auto\_delete：是否自动删除
* durable：是否持久化
* exchange：
* exchange\_type：
* host:
* id:
* key:
* passive:
* port:
* password:
* queue:
* user:
* vhost:
* ssl:

## 2.示例配置

```text
rabbitmq {
    host => "127.0.01"
    port => 5672
    subscription_retry_interval_seconds => 5
    queue => "APICalls"
    key => "logstash-RMQ"
    user => "admin"
    password => "admin"
    vhost => "VHost_Dev"
    durable => "true"
 }
```

如果配置失败，可以通过以下命令查询日志

```text
cd logstash-5.1.1/logs
tail logstash-plain.log
```

配置成功：

> Successfully started Logstash API endpoint {:port=&gt;9600}

配置失败：

> Attempting to install template {:manage_template=&gt;{"template"=&gt;"logstash-\*", "version"=&gt;50001, "settings"=&gt;{"index.refresh\_interval"=&gt;"5s"}, "mappings"=&gt;{"\_default_"=&gt;{"\_all"=&gt;{"enabled"=&gt;true, "norms"=&gt;false}, "dynamic\_templates"=&gt;\[{"message\_field"=&gt;{"path\_match"=&gt;"message", "match\_mapping\_type"=&gt;"string", "mapping"=&gt;{"type"=&gt;"text", "norms"=&gt;false}}}, {"string\_fields"=&gt;{"match"=&gt;"\*", "match\_mapping\_type"=&gt;"string", "mapping"=&gt;{"type"=&gt;"text", "norms"=&gt;false, "fields"=&gt;{"keyword"=&gt;{"type"=&gt;"key......

