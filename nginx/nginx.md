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