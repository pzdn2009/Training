# Nginx如何BasicAuth?

```nginx
server {
    listen       80;   
    server_name  dysong.com;

    auth_basic   "Login Auth";  
    auth_basic_user_file ~/pass_file;
}
```

创建用户名和密码：

```
yum install httpd-tools -y

[root@sdf234]# htpasswd -c -d ~/pass_file pzdn
New password: 
Re-type new password: 
Adding password for user pzdn
```

浏览：
```
wget --http-user=pzdn --http-passwd=123456 http://dysong.com
curl -u pzdn:123456 -O http://dysong.com
```