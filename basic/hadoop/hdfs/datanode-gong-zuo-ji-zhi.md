* 工作职责
* 工作流程
* 数据检验
* 文件结构

# 1. 工作职责

* 存储数据（数据 + 元数据（数据块的长度，块数据的校验和，以及时间戳））
* 定期上报Block信息
* 定期检查数据

# 2. 工作流程

![](/assets/HadoopDataNode.png)

1. 一个数据块在 datanode 上以文件形式存储在磁盘上，包括两个文件，一个是数据本身，一个是元数据包括数据块的长度，块数据的校验和，以及时间戳。

2.  DataNode 启动后向 namenode 注册， 通过后，周期性（1 小时） 的向 namenode 上报所有的块信息。

3. 心跳是每 3 秒一次，心跳返回结果带有 namenode 给该 datanode 的命令如复制块数据到另一台机器，或删除某个数据块。 如果超过 10 分钟没有收到某个 datanode 的心跳，则认为该节点不可用。

4. 集群运行中可以安全加入和退出一些机器。

注意点：
* NameNode不会发起到DataNode的请求。

* DataNode之间还会相互通信，执行数据块复制任务。

# 3. 数据检验

1. 当 DataNode 读取 block 的时候，它会计算 checksum。

2. 如果计算后的 checksum，与 block 创建时值不一样，说明 block 已经损坏。

3. client 读取其他 DataNode 上的 block。

4. datanode 在其文件创建后周期验证 checksum。

# 4. 掉线时限参数设置

DN 进程死亡或者网络故障造成 DN 无法与 NN 通信， NN 不会立即把该节点判定为死亡。 

HDFS 默认的超时时长为 10 分钟+30 秒。如果定义超时时间为 timeout，则超时时长的计算公式为：

```xml
timeout = 2 * dfs.namenode.heartbeat.recheck - interval + 
10 * dfs.heartbeat.interval

<property>
    <name>dfs.namenode.heartbeat.recheck-interval</name>
    <value>300000</value>
</property>
<property>
    <name>dfs.heartbeat.interval </name>
    <value>3</value>
</property>
```

# 5. DN的文件结构

current目录：
```
$ {dfs.data.dir}/current/VERSION
$ {dfs.data.dir}/current/blk_<id_1>
$ {dfs.data.dir}/current/blk_<id_1>.meta
$ {dfs.data.dir}/current/blk_<id_2>
$ {dfs.data.dir}/current/blk_<id_2>.meta
$ {dfs.data.dir}/current/....
$ {dfs.data.dir}/current/subdir0/
$ {dfs.data.dir}/current/subdir1/
$ {dfs.data.dir}/current/....          
$ {dfs.data.dir}/current/subdir63/
```
* 版本文件；
* block文件，数量由`dfs.DataNode.numblocks`控制；
* block元数据文件；

