# 运行级别

## 运行级别

## 1. 运行级别

linux有七个运行级别\(runlevel\)

* 0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
* 1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
* 2：多用户状态\(没有NFS\)
* 3：完全的多用户状态\(有NFS\)，登陆后进入控制台命令行模式
* 4：系统未使用，保留
* 5：X11控制台，登陆后进入图形GUI模式
* 6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

> 在目录/etc/init.d下有许多服务器脚本程序，一般称为服务\(service\)，如cron,cups；
>
> 在/etc/下有7个名为rcN.d的目录，对应系统的7个运行级别；
>
> rcN.d目录下都是一些符号链接文件，这些链接文件都指向init.d目录下的service脚本文件，命名规则为K+nn+服务名或S+nn+服务名，其中nn为两位数字；
>
> 系统会根据指定的运行级别进入对应的rcN.d目录，并按照文件名顺序检索目录下的链接文件 对于以K开头的文件，系统将终止对应的服务 对于以S开头的文件，系统将启动对应的服务

命令：

```text
runlevel #查看当前系统的运行级别
init N # 进入其他运行级别，另外init0为关机，init 6为重启系统
```

## 2.Ubuntu Vs CentOs

### 2.1 查看运行级别

```text
runlevel # ubuntu
who -r # centos
```

### 2.2 设置开机运行级别

**Ubuntu**在/etc/init/rc-sysinit.conf中设置

```text
env DEFAULT_RUNLEVEL=2
```

在/etc/inittab中设置，Ubuntu默认没有该文件，因此创建该文件，添加

```text
id:3:initdefault:
```

即可；

**CentOS** 在/etc/inittab中设置

```text
id:3:initdefault:
```

即可。

### 2.3 级别解析

Ubuntu

* 0：关机； 
* 1：单用户模式； 
* 2-5：多用户模式； 
* 6：重启；

CentOS

* 0：关机； 
* 1：单用户； 
* 2：多用户、没网络； 
* 3：多用户有网络，但是为文本模式； 
* 4：系统未用； 
* 5：图形界面； 
* 6：重启；

## 3. 改变

* 命令行

  ```text
  init ［0123456］
  ```

* 文件： /etc/inittab

  ```text
  id:[0-6]]:initdefault:
  ```

