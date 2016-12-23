# Logstash配置rabbitmq input

场景：当使用rmq作为数据源输入时，则可以使用rabbitmq输入插件。

官方文档：https://www.elastic.co/guide/en/logstash/current/plugins-inputs-rabbitmq.html#plugins-inputs-rabbitmq-password

# 1. 主要字段
- ack：是否消息自动确认
- auto_delete：是否自动删除
- durable：是否持久化
- exchange：
- exchange_type：
- host:
- id:
- key:
- passive:
- port:
- password:
- queue:
- user:
- vhost:
- ssl:

# 2.示例配置
```
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
```
cd logstash-5.1.1/logs
tail logstash-plain.log
```

配置成功：
>Successfully started Logstash API endpoint {:port=>9600}

配置失败：
>Attempting to install template {:manage_template=>{"template"=>"logstash-*", "version"=>50001, "settings"=>{"index.refresh_interval"=>"5s"}, "mappings"=>{"_default_"=>{"_all"=>{"enabled"=>true, "norms"=>false}, "dynamic_templates"=>[{"message_field"=>{"path_match"=>"message", "match_mapping_type"=>"string", "mapping"=>{"type"=>"text", "norms"=>false}}}, {"string_fields"=>{"match"=>"*", "match_mapping_type"=>"string", "mapping"=>{"type"=>"text", "norms"=>false, "fields"=>{"keyword"=>{"type"=>"key......