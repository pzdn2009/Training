# Linux下通过端口杀死进程

```shell
netstat -nlp #查看所有端口
netstat -nlp | grep :3306 #查找3306端口
netstat -nlp | grep :3306 | awk '{print $7}' #读取出端口号
netstat -nlp | grep :3306 | awk '{print $7}' | awk -F"/" '{ print $1 }'
netstat -nlp | grep :3306 | awk '{print $7}' | awk -F"/" '{ print $1 }' | kill
```

## netstat

Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.打印网络连接，路由表，接口统计，掩码连接，广播关系。

- n Show numerical addresses instead of trying to determine symbolic host, port or user names.
- l listening
- p PID/Program Name

```
[root@some pz]# netstat -nlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name   
tcp        0      0 0.0.0.0:111                 0.0.0.0:*                   LISTEN      1057/rpcbind        
tcp        0      0 0.0.0.0:10000               0.0.0.0:*                   LISTEN      1508/perl           
tcp        0      0 0.0.0.0:22                  0.0.0.0:*                   LISTEN      1341/sshd           
tcp        0      0 127.0.0.1:631               0.0.0.0:*                   LISTEN      1225/cupsd          
tcp        0      0 127.0.0.1:25                0.0.0.0:*                   LISTEN      1417/master         
tcp        0      0 0.0.0.0:57983               0.0.0.0:*                   LISTEN      1075/rpc.statd      
tcp        0      0 0.0.0.0:7300                0.0.0.0:*                   LISTEN      1500/../bin/redis-s 
```

## awk

1. AWK读取输入文件一次一行。 
2. 对于每一行，它匹配在给定的顺序模式，如果匹配，执行相应的动作。 

print 是awk打印指定内容的主要命令，打印：
```
awk '{print}'  /etc/passwd
awk '{print $0}'  /etc/passwd 

# 不输出passwd的内容，而是输出相同个数的空行，进一步解释了awk是一行一行处理文本
awk '{print " "}' /etc/passwd                                           

# 输出相同个数的a行，一行只有一个a字母
awk '{print "a"}'   /etc/passwd
awk -F":" '{print $1}'  /etc/passwd 

# 将每一行的前二个字段，分行输出，进一步理解一行一行处理文本
awk -F: '{print $1; print $2}'   /etc/passwd

# 输出字段1,3,6，以制表符作为分隔符
awk  -F: '{print $1,$3,$6}' OFS="\t" /etc/passwd
```