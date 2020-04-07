# HDFS

## HDFS

* [HDFSCLI操作](/basic/hadoop/hdfs/hdfscli-cao-zuo.md)
* [HDFS读写数据原理](/basic/hadoop/hdfs/hdfs-du-xie-shu-ju-yuan-li.md)
* [NameNode工作机制](/basic/hadoop/hdfs/namenode-gong-zuo-ji-zhi.md)
* [DataNode工作机制](/basic/hadoop/hdfs/datanode-gong-zuo-ji-zhi.md)
* [HDFSJAVAAPI](/basic/hadoop/hdfs/hdfsjavaapi.md)
* [NameNode工作机制2](/basic/hadoop/hdfs/namenodegong-zuo-ji-zhi-2.md)
* [HDFS-参数配置](/basic/hadoop/hdfs/hdfscan-shu-pei-zhi.md)
* [hadoop-sh](/basic/hadoop/hdfs/hadoop-sh.md)

Maven依赖：

```markup
<!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common -->
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-common</artifactId>
    <version>2.7.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-hdfs -->
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-hdfs</artifactId>
    <version>2.7.2</version>
</dependency>
```

## 1. 概念与组成

**分而治之**：将大文件、大批量文件，分布式存放在大量服务器上，以便于采取分而治之的方式对海量数据进行运算分析；

特点：

* 文件系统——用于存储文件，通过统一的命名空间——目录树来定位文件，Eg：`hdfs://namenode:port/dir/**/file.data`；
* 分布式——具备副本；
* block：默认为128M（2.x版本）。每个块分为多个分块（chunk），chunk作为独立的存储单元；
* namenode：Manager，管理文件系统的命名空间。记录每个文件中各个块所在的数据节点信息；
* datanode：Worker，存储并检索数据块，且定期向namenode发送他们所存储的块的列表；
* HDFS是设计成适应一次写入，多次读出的场景，且不支持文件的修改；
* CLI：提供对HDFS的访问。

## 2. 工作机制

1. 两大角色：NameNode、DataNode \(Secondary Namenode\)
2. NameNode负责管理整个文件系统的元数据\(整个hdfs文件系统的目录树和每个文件的block信息\)
3. DataNode 负责管理用户的文件数据块
4. 文件会按照固定的大小（blocksize）切成若干块后分布式存储在若干台datanode上 每一个文件块可以有多个副本，并存放在不同的datanode上
5. Datanode会**定期**向Namenode汇报自身所保存的文件block信息，而namenode则会负责保持文件的副本数量
6. HDFS的内部工作机制对客户端保持**透明**，客户端请求访问HDFS都是通过向`namenode`申请来进行

## 3. FileSystemAPI

使用FileSystemAPI来显示Hadoop中的文件： 读取数据：

```java
import java.io.InputStream;

import java.net.URI;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.*;
import org.apache.hadoop.io.IOUtils;

public class FileSystemCat {
    public static void main(String[] args) throws Exception {
        String uri = args[0];
        Configuration conf = new Configuration();

        //根据地址创建一个流
        FileSystem fs = FileSystem. get(URI.create (uri), conf);
        InputStream in = null;
    try {
            in = fs.open( new Path(uri));
            IOUtils.copyBytes(in, System.out, 4096, false);
        } finally {
            IOUtils.closeStream(in);
        }
    }
}
```

Usage:

```text
hadoop FileSystemCat hdfs://localhost/user/tom/quangle.txt
```

## 4. CLI

```bash
hadoop fs -ls /
hadoop fs -lsr
hadoop fs -mkdir /user/hadoop
hadoop fs -put a.txt /user/hadoop/
hadoop fs -get /user/hadoop/a.txt /
hadoop fs -cp src dst
hadoop fs -mv src dst
hadoop fs -cat /user/hadoop/a.txt
hadoop fs -rm /user/hadoop/a.txt
hadoop fs -rmr /user/hadoop/a.txt
hadoop fs -text /user/hadoop/a.txt
hadoop fs -copyFromLocal localsrc dst #与hadoop fs -put功能类似。
hadoop fs -moveFromLocal localsrc dst #将本地文件上传到hdfs，同时删除本地文件。
```

## 5.java 接口

功能：

* 从Hadoop URL中读取数据
* 将本地的数据复制到Hadoop（写入）
* 新建目录
* 展示文件状态信息
* 列出文件
* 删除数据

```text
IOUtils.closeStream()
IOUtils.copyBytes()
FileSytem.get
FileStatus
```

## 6. 序列化

序列化：将结构化对象转为字节流。

Hadoop使用Writable来封装数据。

具体类型的序列化：IntWritable、Text、ByteWritable、ObjectWritable、NullWritable、MD5Hash、ArraryWritable、MapWritable、SortedMapWritable。

序列化框架：WritableSerialization

