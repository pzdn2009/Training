# sudo命令

含义：execute a command as another user.

格式： sudo -h \| -K \| -k \| -V sudo -v \[-AknS\] \[-a type\] \[-g group\] \[-h host\] \[-p prompt\] \[-u user\] sudo -l \[-AknS\] \[-a type\] \[-g group\] \[-h host\] \[-p prompt\] \[-U user\] \[-u user\] \[command\] sudo \[-AbEHnPS\] \[-a type\] \[-C num\] \[-c class\] \[-g group\] \[-h host\] \[-p prompt\] \[-r role\] \[-t type\] \[-T timeout\] \[-u user\] \[VAR=value\] \[-i \| -s\] \[command\] sudoedit \[-AknS\] \[-a type\] \[-C num\] \[-c class\] \[-g group\] \[-h host\] \[-p prompt\] \[-T timeout\] \[-u user\] file ...

选项：

* -l 列出权限

相关命令：

* su 切换身份
* sudoedit 

免sudo：

> 1. 编辑/etc/sudoers
>
> \#\# Allow root to run any commands anywhere root ALL=\(ALL\) ALL 添加：

user1 ALL=\(root\) NOPASSWD: /usr/sbin/useradd, /usr/sbin/usermod

