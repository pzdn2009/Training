# 常见的Log 文件

Ref：https://www.cnblogs.com/hukey/p/10108811.html

# /var/log/message

记录了系统日常的一些操作，内核态和用户态的信息都有，对于这个文件，我们日常只需要关注一些错误和告警信息。

```sh
egrep -ri 'error|warn' /var/log/messages
```

# /var/log/boot.log

该日志记录的是系统启动的过程。

可以通过该日志，查看某些服务启动成功或者失败。

# /var/log/lastlog

不用直接查看该日志文件，通过命令：**lastlog** 查看：

# /var/log/secure

主要记录用户登录认证。例如，sshd会将所有信息记录（其中包括失败登录）在这里。

如果出现大量： authentication failure;   就表示有程序或者人为在尝试登录。可以通过加强 ssh 或者 iptables 来管控登录次数。

# /var/log/btmp

记录Linux**登陆失败**的用户、时间以及远程IP地址

该文件是一个二进制保存的文件，直接使用 **lastb** 命令查看。如果该日志文件过大，可以清空该文件。

# /var/log/wtmp

该日志文件永久记录每个用户登录、注销及系统的启动、停机的事件，使用last命令查看

直接使用 last 命令查看。

使用wtmp可以找出谁正在登陆进入系统，谁使用命令显示这个文件或信息等。

# /var/log/maillog 

包含来着系统运行电子邮件服务器的日志信息。例如，sendmail日志信息就全部送到这个文件中。

# /var/log/yum.log 

包含使用yum安装的软件包信息。

# /var/log/cron 

每当cron进程开始一个工作时，就会将相关信息记录在这个文件中。

# /var/log/dmesg 

包含内核缓冲信息（kernel ring buffer）。在系统启动时，会在屏幕上显示许多与硬件有关的信息。可以用dmesg查看它们。

# /var/log/daemon.log 

包含各种系统后台守护进程日志信息。

# /var/log/sa/

包含每日由sysstat软件包收集的sar文件。