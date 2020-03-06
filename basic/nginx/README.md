# Nginx

## Nginx

## V1

S1：500设置，404设置，双备，查看日志和分析日志（工具），nginx-rrd，client IP转发，GoAccess。监控。安装第三方插件。http到https，https到http。

timeout，keepalive，server，location，匹配，反向代理，upstream，proxy\_pass，个数参数，代理cookie，多个upstream，变量定义与使用，CGI（Fast CGI、SCGI、uWSGI），内置变量参数$request\_uri，X-Real-IP，X-Forwarded-For。

### V1.0

1. 配置文件结构，server，location；
2. 404设置
3. upstream

## 一个简单的代理配置

本地地址localhost:9100将被代理

```text
upstream eshead {
    server localhost:9100;
}
```

对外端口为82，proxy\_pass为真正地址

```text
server {
        listen       82;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm;
            proxy_pass  http://eshead;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
}
```

