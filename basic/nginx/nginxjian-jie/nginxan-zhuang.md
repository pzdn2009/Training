# Nginx安装

## 1. 使用yum安装

```text
sudo yum -y install nginx 
cat /etc/passwd #
```

安装完毕之后，可以看到

nginx:x:985:979:Nginx web server:/var/lib/nginx:/sbin/nologin

将当前用户加入nginx组

```text
su -l 
usermod -aG nginx pzdn 
su -l pzdn
```

## 2. nginx命令

Usage: nginx \[-?hvVtTq\] \[-s signal\] \[-c filename\] \[-p prefix\] \[-g directives\]

选项：

* -?,-h         : 帮助
* -v            : 版本
* -V            : 版本和配置选项

  ```text
  --prefix=/usr/share/nginx 
  --sbin-path=/usr/sbin/nginx 
  --modules-path=/usr/lib64/nginx/modules 
  --conf-path=/etc/nginx/nginx.conf 
  --error-log-path=/var/log/nginx/error.log 
  --http-log-path=/var/log/nginx/access.log 
  --lock-path=/run/lock/subsys/nginx 
  --user=nginx 
  --group=nginx
  ......
  ```

* -t : 测试配置文件

  成功的示例

  ```text
  [pzdn@CentOS7 nginx]$ sudo nginx -t
  nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
  nginx: configuration file /etc/nginx/nginx.conf test is successful
  ```

  失败的示例

  ```text
  [pzdn@CentOS7 nginx]$ nginx -t
  nginx: [alert] could not open error log file: open() "/var/log/nginx/error.log" failed (13: Permission denied)
  2016/12/23 10:35:57 [warn] 21299#0: the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:5
  nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
  2016/12/23 10:35:57 [emerg] 21299#0: open() "/run/nginx.pid" failed (13: Permission denied)
  nginx: configuration file /etc/nginx/nginx.conf test failed
  ```

* -T            : test configuration, dump it and exit
* -q            : suppress non-error messages during configuration testing
* -s signal     : 发送信号给主进程: stop, quit, reopen, reload
* -p prefix     : 设置前缀路径\(default: /usr/share/nginx/\)
* -c filename   : 设置配置文件 \(default: /etc/nginx/nginx.conf\)
* -g directives : 设置配置文件以外的全局指令

## 3. 命令示例

### 3.1 测试指定的文件

```text
[pzdn@CentOS7 nginx]$ sudo nginx -t -c /etc/nginx/nginxKibana.conf 
nginx: the configuration file /etc/nginx/nginxKibana.conf syntax is ok
nginx: configuration file /etc/nginx/nginxKibana.conf test is successful
```

### 3.2 重启

```text
[pzdn@CentOS7 nginx]$ sudo nginx -s reload
```

ps方式重启

```text
ps -ef | grep nginx
kill -HUP pid
```

### 3.3 启动

```text
/usr/sbin/nginx -c /etc/nginx/nginxKibana.conf
```

