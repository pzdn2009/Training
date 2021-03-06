# 设置404跳转

Ref：[http://www.yunweipai.com/archives/22238.html](http://www.yunweipai.com/archives/22238.html)

这里说得是**反向代理**404的处理。

如果后台Tomcat处理报错抛出404，想把这个状态叫Nginx反馈给客户端或者重定向到某个连接，配置如下：

```nginx
upstream www {

server 192.168.1.201:7777 weight=20 max_fails=2 fail_timeout=30s;

ip_hash;

}

server {

listen 80;

server_name www.test.com;

root /var/www/test;

index index.html index.htm;



location / {

if ($request_uri ~* ‘^/$’) {

rewrite .* http://www.test.com/index.html redirect;

}

# 关键参数：这个变量开启后，我们才能自定义错误页面，当后端返回404，nginx拦截错误定义错误页面

proxy_intercept_errors on;

proxy_pass http://www;

proxy_set_header HOST $host;

proxy_set_header X-Real-IP $remote_addr;

proxy_set_header X-Forwarded-FOR $proxy_add_x_forwarded_for;

}

error_page 404 /404.html;

location = /404.html {

root /usr/share/nginx/html;

}

}
```

在server内的关键配置点：

* proxy\_intercept\_errors on；打开代理拦截错误开关
* 在server的同级设置error\_page 404 url；
* 如果要针对某个location拦截，那么将error\_page配置在location内部。

