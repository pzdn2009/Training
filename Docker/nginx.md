# Nginx Dockerfile

```
FROM localhost:5000/ubt1404sshd
MAINTAINER pzdn2009 (1050244110@qq.com)

RUN \
 apt-get install -y nginx && \
 rm -rf /var/bin/apt/lists/* && \
 echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
 chown -R www-data:www-data /var/lib/nginx

RUN echo "Asia/Shanghai" > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata

VOLUME ["/etc/nginx/sites-enabled","/etc/nginx/certs","/etc/nginx/conf.d"]

WORKDIR /etc/nginx
 
CMD /usr/sbin/sshd & /usr/sbin/nginx

EXPOSE 80
EXPOSE 433
```

请求：

http://192.168.2.106:32774/ 
其中192.168.2.106是主机的IP，32774是主机的端口，映射到容器的80端口。

即看到如下内容：


<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

