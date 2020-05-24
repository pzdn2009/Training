# Nginx反向代理

* 上游：资源的发源地
* SSL
* upstream相关变量

## 1. upstream

* server： ip:port
* keepalive：和上游保持连接的时间
* least_conn ：负载均衡，最少连接数
* ip_hash ： 负载均衡，ip hash地址


负载均衡：
* Round-bin
* ip hash
* least Connection


## 2. 示例 

```nginx
server ip:port max_fails=5 fail_timeout=5 weight=10
```
参数：
* weight=NUMBER   用于设置服务器的权重。如果没有设置，那么它将会等于 1
* max_fails=NUMBER   该参数用于对后端服务器进行检测，如果达到 NUMBER 次数依然失败，则该 server 会被暂停 fail_timeout 秒，如果没有设置该参数，那么尝试的次数为1，如果设置为 0 则关闭检测。失败的依据是根据 proxy_next_upstream 提供的。
* fail_timeout=TIME   该参数用于设置客户端到达 max_fails 次数后，该server 被暂停的时间。如果没有设置该参数，那么默认为 10秒。
* down：如果为某一个 server 设置了该参数，那么标记了这台 server 将永久离线。通常这个参数与 ip_hash 一同使用。
* backup：该参数在 0.6.7 版本中提供，它是一个备用标识，如果出现所有的非备份服务器全部宕机或繁忙无法接受连接时，那么才会使用本服务器，该参数无法和 ip_hash 指令一起使用。

## 3 upstream相关变量

* $upstream_addr

功能：该变量表示了处理该请求的 upstream 中 server 的地址

* $upstream_cache_status

功能：该变量出现在 Nginx 0.8.3 版本中， 可能的值如下：

MISS - 缓存中未被命中
EXPIRED - 生存期期满，请求被传递到后端服务器
UPDATING - 生存期满，陈旧的响应被使用，因为proxy/fastcgi_cache_use_stale 升级
STALE - 生存期期满，陈旧的响应被使用，因为 proxy/fastcgi_cache_use_stale
HIT - 缓存命中
 

* $upstream_status

功能：该变量为 upstream 中 server 的响应状态

* $upstream_response_time

功能：upstream server 响应的时间，单位为秒，能够精准到毫秒。如果有多个 server 响应回答，那么会用逗号和冒号分隔开

* $upstream_http_$HEADER

功能：HTTP 协议头。例如：$upstream_http_host