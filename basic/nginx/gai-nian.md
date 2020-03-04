# 概念

Ref:[http://www.cnblogs.com/jun1019/p/6260682.html](http://www.cnblogs.com/jun1019/p/6260682.html)

## 配置文件结构

```text
section {
   directive parameters;
}
```

配置http服务器的主要段

1. http参数部分。如连接超时时间、压缩、缓冲等
2. upstream 负载均衡设置.
3. server 配置虚拟主机.

   可以包含多个location location 用来url定位，把特殊的路径或文件再次定位，如image目录单独处理

## server

虚拟主机，就是把一台**物理服务器**划分成多个“虚拟”的服务器server，每一个虚拟主机都可以有独立的**域名和独立的目录**

nginx的虚拟主机就是通过nginx.conf中server节点指定的，想要设置多个虚拟主机，配置多个server节点即可。

* server\_name reg；虚拟主机名
* listen port；监听端口
* autoindex on；开启目录访问
* index index.htm；设置首页
* root /home/www/a/；设置物理根目录

server\_name示例：

Point：正则表达式以波浪线开头

```text
server_name ~^www\d+\.test\.com$; 
server_name test.com www.test.com *.test.com;
```

## Location

location可以嵌套

```text
location [modifier] uri {...}
location @name {...}
```

修饰符：

* = 精确匹配
* ~ 区分大小写
* ~\* 不区分大小写
* ^~ 最佳location

## upstream

上游，有发源的意思。故上游服务器指的产生内容的服务器。

如nginx+tomcat tomcat是上游服务器。在nginx中有配置upstream,就是配置上游服务器集群，如应用服务器tomcat

最重要的是使用location的proxy\_pass。

用于负载均衡的配置。

```text
upstream name {
  server uri;
}

upstream blog {
 server 192.168.80.121:80;
 server 192.168.80.122:80;
 server 192.168.80.123:80 backup;
}
```

使用：

\`\`\`conf location / { proxy\_pass [http://blog](http://blog) }

