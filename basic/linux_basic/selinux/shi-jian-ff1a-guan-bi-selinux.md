# 实践：关闭SELinux

为什么要关闭selinux？ 因为在linux操作系统中默认开启了防火墙，SELinux也处于启动状态，一般状态为enforing。致使很多服务端口默认是关闭的。

## 永久方法

修改`/etc/selinux/config`文件中改为：`SELINUX=disabled` ，然后重启。

## 临时方法

使用命令：`setenforce 0`

```text
setenforce 1 设置SELinux 成为enforcing模式
setenforce 0 设置SELinux 成为permissive模式
```

