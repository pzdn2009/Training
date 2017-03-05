# HDFS

# 1. 概念与组成

HDFS以流式数据访问模式来存储超大文件。一次写入、多次读取。

组成：

- block：默认为64M。每个块分为多个分块（chunk），chunk作为独立的存储单元。
- namenode：Manager，管理文件系统的命名空间。记录每个文件中各个块所在的数据节点信息。
- datanode：Worker，存储并检索数据块，且定期向namenode发送他们所存储的块的列表。
- CLI：提供对HDFS的访问。

# 2. 文件读写图

Read：
![](/assets/hd3.png)

Write：
![](/assets/hd4.png)