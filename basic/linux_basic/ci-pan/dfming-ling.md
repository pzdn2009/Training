# df命令

含义：report file system disk space usage.

格式：**df \[OPTION\]... \[FILE\]...**

Options：

* -h, --human-readable

  print sizes in human readable format \(e.g., 1K 234M 2G\).

* -i, --inodes

  list inode information instead of block usage.

* -t, --type=TYPE

  limit listing to file systems of type TYPE.

* -l, --local

  limit listing to local file systems. 显示本地的分区的磁盘空间使用率,如果服务器nfs了远程服务器的磁盘,那么在df上加上-l后系统显示的是过滤nsf驱动器后的结果。

解释：

显示指定磁盘文件的可用空间。如果没有文件名被指定，则所有当前被挂载的文件系统的可用空间将被显示。默认情况下，磁盘空间将以 1KB 为单位进行显示，除非环境变量 POSIXLY\_CORRECT 被指定，那样将以512字节为单位进行显示。

实践：

* 显示磁盘使用情况
* 以inode模式来显示磁盘使用情况
* 显示指定类型磁盘
* 列出各文件系统的i节点使用情况
* 列出文件系统的类型
* 以更易读的方式显示目前磁盘空间和使用情况 

```bash
df
df -i
df -t ext3
df -ia
df -T
df -h
df -lh
```

