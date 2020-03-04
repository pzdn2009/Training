# 集群安装

Ref:[http://blog.csdn.net/u014039577/article/details/47746629。](http://blog.csdn.net/u014039577/article/details/47746629。) Zookeeper服务有两个角色，一个是leader，负责写服务和数据同步，剩下的是follower，提供读服务，leader失效后会在follower中重新选举新的leader。

三台机器：

/etc/hosts

```text
172.17.6.142 logsrv02  
172.17.6.148 logsrv03  
172.17.6.149 logsrv04
```

修复$ZK\_HOME/conf/zoo.cfg

```text
# The number of milliseconds of each tick  
tickTime=2000  
# The number of ticks that the initial   
# synchronization phase can take  
initLimit=10  
# The number of ticks that can pass between   
# sending a request and getting an acknowledgement  
syncLimit=5  
# the directory where the snapshot is stored.  
dataDir=/tmp/zookeeper  
# the port at which the clients will connect  
clientPort=2181  
server.1=logsrv02:2888:3888  
server.2=logsrv03:2888:3888  
server.3=logsrv04:2888:3888
```

设置PID文件：

```text
[root@logsrv02 zookeeper]# echo "1" > /tmp/zookeeper/myid  
[root@logsrv03 zookeeper]# echo "2" > /tmp/zookeeper/myid  
[root@logsrv04 zookeeper]# echo "3" > /tmp/zookeeper/myid
```

启动集群：

```text
[root@logsrv02 zookeeper-3.3.6]# bin/zkServer start  
[root@logsrv03 zookeeper-3.3.6]# bin/zkServer start  
[root@logsrv04 zookeeper-3.3.6]# bin/zkServer start
```

查看集群：

```text
[root@logsrv02 zookeeper-3.3.6]# bin/zkServer.sh status  
JMX enabled by default  
Using config: /usr/local/jiang/zookeeper-3.3.6/bin/../conf/zoo.cfg  
Mode: follower  

[root@logsrv03 zookeeper-3.3.6]# bin/zkServer.sh status  
JMX enabled by default  
Using config: /usr/local/jiang/zookeeper-3.3.6/bin/../conf/zoo.cfg  
Mode: follower  

[root@logsrv04 zookeeper-3.3.6]# bin/zkServer.sh status  
JMX enabled by default  
Using config: /usr/local/zookeeper-3.3.6/bin/../conf/zoo.cfg  
Mode: leader
```

