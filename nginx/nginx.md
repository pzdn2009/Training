# Nginx

# V1
S1：500设置，404设置，双备，查看日志和分析日志（工具），nginx-rrd，client IP转发，GoAccess。监控。安装第三方插件。http到https，https到http。

timeout，keepalive，server，location，匹配，反向代理，upstream，proxy_pass，个数参数，代理cookie，多个upstream，变量定义与使用，CGI（Fast CGI、SCGI、uWSGI），内置变量参数$request_uri，X-Real-IP，X-Forwarded-For。



# 一个简单的代理配置

本地地址localhost:9100将被代理

```
upstream eshead {
    server localhost:9100;
}
```

对外端口为82，proxy_pass为真正地址
```
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