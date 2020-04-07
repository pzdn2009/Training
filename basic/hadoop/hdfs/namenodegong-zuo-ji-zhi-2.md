* 安全模式
* checkpoint时机

# 1. 安全模式

安全模式下，集群属于只读状态。只是保证HDFS元数据信息的访问，而不保证文件的访问。

* 在系统的正常操作期间，NameNode会在内存中保留所有**块位置**的映射信息。
* 在安全模式下，各个DataNode会向NameNode发送最新的块列表信息
* 如果满足“最小副本条件”，NameNode会在30秒钟之后就退出安全模式。所谓的最小副本条件指的是在整个文件系统中99.9%的块满足最小副本级别（默认值：dfs.replication.min=1）。

命令：

```
# 查看是否为安全模式
hadoop dfsadmin -safemode get
# 等待关闭
hadoop dfsadmin -safemode wait
# 退出
hadoop dfsadmin -safemode leave
# 启用
hadoop dfsadmin -safemode enter
```

# 2. checkpoint设置

[hdfs-default.xml]：
```xml
<property>
  <name>dfs.namenode.checkpoint.txns</name>
  <value>1000000</value>
  <description>操作动作次数</description>
</property>

<property>
  <name>dfs.namenode.checkpoint.check.period</name>
  <value>60</value>
  <description> 1分钟检查一次操作次数</description>
</property >
```
* 定时：默认一个小时
* 定量：默认1 million。
