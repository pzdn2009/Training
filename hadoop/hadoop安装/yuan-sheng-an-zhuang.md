# Hadoop原生安装

# 1. 下载

官网：http://hadoop.apache.org/

下载地址：
1. https://archive.apache.org/dist/hadoop/common/ （Apache列表）
2. http://apache.fayea.com/hadoop/common/（镜像）
3. 阿里云

eg:
下载1.2.1版本
![](/assets/hd2.png)

# 2.安装方式

Hadoop安装有如下三种方式：

- 单机模式：安装简单，几乎不用作任何配置，但仅限于调试用途；
- 伪分布模式：在单节点上同时启动NameNode、DataNode、JobTracker、TaskTracker、Secondary Namenode等5个进程，模拟分布式运行的各个节点；
- 完全分布式模式：正常的Hadoop集群，由多个各司其职的节点构成

# 3. 伪分布式的安装

地址：
http://hadoop.apache.org/docs/r1.2.1/single_node_setup.html  

## 3.1 配置

conf/core-site.xml:HDFS地址
```
<configuration>
     <property>
         <name>fs.default.name</name>
         <value>hdfs://localhost:9000</value>
     </property>
     <property>
         <name>hadoop.tmp.dir</name>
          <value>/home/pzdn/hadoop-1.2.1/tmp</value>
      </property>
</configuration>
```
```
sudo chmod g-w /home/pzdn/hadoop-1.2.1/tmp # 755
```

conf/hdfs-site.xml: 副本
```
<configuration>
     <property>
         <name>dfs.replication</name>
         <value>1</value>
     </property>
</configuration>
```
conf/mapred-site.xml:MR地址
```
<configuration>
     <property>
         <name>mapred.job.tracker</name>
         <value>localhost:9001</value>
     </property>
</configuration>
```

## 3.2 SSH
```
$ ssh localhost
# 如上上述命令失败，则执行以下两行
$ ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa 
$ cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
```

## 3.3 启动

```
$ bin/hadoop namenode -format #格式化HDFS
$ bin/start-all.sh #启动所有

pzdn@ubuntu:~/hadoop-1.2.1$ jps #查看进程
5757 JobTracker
5671 SecondaryNameNode
6062 Jps
5518 DataNode
5905 TaskTracker
5353 NameNode
```

## 3.4 管理

JobTracker ：
http://localhost:50030/jobtracker.jsp

NameNode ：
http://localhost:50070/

# 4. 其他

1. 需具备java环境.

