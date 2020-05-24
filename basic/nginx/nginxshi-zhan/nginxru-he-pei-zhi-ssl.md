# Nginx如何配合SSL

* 范围：针对Server
* 配置：ssl_开头

```nginx
server {

  listen    443;
  server_name  XXX.com;
  #定义服务器的默认网站根目录位置
  root /web/www/website/dist;  
  #设定本虚拟主机的访问日志
  access_log  logs/nginx.access.log  main;

  # ssl目录，用来放置证书，和 nginx.conf同级
  ssl on;
  ssl_certificate ssl/aliyun.pem;
  ssl_certificate_key ssl/aliyun.key;
  ssl_session_timeout 5m;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers  ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4
  ssl_prefer_server_ciphers on;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  
  # 剩余的配置...
}
```