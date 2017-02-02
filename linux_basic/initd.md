# init.d

# 1. init.d目录

包含许多系统各种服务的启动和停止脚本

当你查看/etc目录时，你会发现许多rc#.d形式存在的目录（这里#代表一个指定的初始化级别，范围是0~6）。

在这些目录之下，包含了许多对进程进行控制的脚本。这些脚本要么以"K"开头，要么以"S"开头。以K开头的脚本运行在以S开头的脚本之前。这些脚本放置的地方，将决定这些脚本什么时候开始运行。在这些目录之间，系统服务一起合作，就像运行状况良好的机器一样。如果你在使用Fedora系统，你可以找到这个目录：/etc/rc.d/init.d。实际上无论init.d放在什么地方，它都发挥着相同的作用。
为了能够使用init.d目录下的脚本，你需要有root权限或sudo权限。

每个脚本都将被作为一个命令运行，该命令的结构大致如下所示：

/etc/init.d/service command 选项
comand是实际运行的命令，选项可以有如下几种：
- start
- stop
- reload
- restart
- force-reload

大多数的情况下，你会使用start,stop,restart选项。例如，如果你想关闭网络，你可以使用如下形式的命令：
```
/etc/init.d/networking stop
```

又比如，你改变了网络设置，并且需要重启网络。你可以使用如下命令：
```
/etc/init.d/networking restart
```

init.d目录下常用初始化脚本有：
- networking
- samba
- apache2
- ftpd
- sshd
- dovecot
- mysql

当然，你可能有其他更多常用的脚本，这个取决于你安装了什么linux操作系统。

# 2. /etc/rc.local

可以安全地在里面添加你想在系统启动之后执行的脚本.

eg:
系统启动的时候就可以自动启动服务了：
```
./redis-server /usr/redis/redis63.conf
./redis-server /usr/redis/redis64.conf
./redis-server /usr/redis/redis65.conf
./redis-server /usr/redis/redis66.conf
```

