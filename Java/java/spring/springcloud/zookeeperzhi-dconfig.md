# Zookeeper之DConfig

用来实现分布式的数据中心。

配置要求：不能配置在application.properties中，一定要配置在bootstrap.yml中，具体不清楚为什么这样。

```properties
spring:
  cloud:
    zookeeper:
      enabled: true
      connect-string: 172.18.21.144:2184,172.18.21.144:2185,172.18.21.144:2186
      config:
          enabled:true
          watcher:false
  application:
    name: cmswebapi
```

* Configuration is stored in the **/config** namespace by default. 
* 会基于application和环境profile来创建节。
如：
```
config/testApp,dev
config/testApp
config/application,dev
config/application
```