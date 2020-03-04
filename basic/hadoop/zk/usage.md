# Usage

## Usage

## 1.安装、运行

下载：

[http://mirrors.cnnic.cn/apache/zookeeper/zookeeper-3.4.9/](http://mirrors.cnnic.cn/apache/zookeeper/zookeeper-3.4.9/)

[http://mirrors.aliyun.com/apache/zookeeper/zookeeper-3.4.9/](http://mirrors.aliyun.com/apache/zookeeper/zookeeper-3.4.9/)

```text
tar vxf zookeeper-3.4.9 #解压
cd zookeeper-3.4.9

export ZOOKEEPER_INSTALL=$(pwd) #配置安装目录
export PATH=$PATH:$ZOOKEEPER_INSTALL/bin

cd cfg
mv zoo_sample.cfg zoo.cfg #修改配置文件

cd ../bin
./zkServer.sh start
echo ruok | nc localhost 2181
```

返回imok即表示启动成功。

## 2.命令行工具

```text
$ bin/zkCli.sh -server 127.0.0.1:2181

[zk: 127.0.0.1:2181(CONNECTED) 2] help
ZooKeeper -server host:port cmd args
 connect host:port
 get path [watch]
 ls path [watch]
 set path data [version]
 rmr path
 delquota [-n|-b] path
 quit
 printwatches on|off
 create [-s] [-e] path data acl #-s 顺序节点，-e临时节点。默认创建持久节点。
 stat path [watch]
 close
 ls2 path [watch]
 history
 listquota path
 setAcl path acl
 getAcl path
 sync path
 redo cmdno
 addauth scheme auth
 delete path [version]
 setquota -n|-b val path
```

操作：

```text
ls /
create /zk_test
ls /

get /zk_test
my_data
cZxid = 0xa
ctime = Sat Oct 29 00:26:24 PDT 2016
mZxid = 0xa
mtime = Sat Oct 29 00:26:24 PDT 2016
pZxid = 0xa
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 0

set /zk_test junk
get /zk_test
delete /zk_test
```

## 3. 配置

* clientPort：默认2181，服务器对外的端口。
* dataDir：数据持久化目录。
* tickTime：ZK的滴答时间，默认为3000ms，代表一个时间单元。比如超时是2~10个单元。
* dataLogDir：事务日志目录。尽量给事务日志配置一个单独的挂载点或磁盘，以提高性能。
* initLimit：等待Fllower启动的时间。默认为10，即10\*3000ms。初始化有数据同步的动作。
* syncLimit：Fllower与Leader之间的心跳的时间，默认为5个tickTime。
* server.id=host:port:port：服务器.id=主机，第一个端口用于Follow与Leader通信与数据同步，第二个端口用于Leader选举的端口。

