# HDFSCLI操作

```text
hadoop fs -help #查看全部命令
```

常用参数介绍：

* -help。

  功能：输出这个命令参数手册

* -ls 功能：显示目录信息 示例： `hadoop fs -ls hdfs://hadoop-server01:9000/` 备注：这些参数中，所有的hdfs路径都可以简写 –&gt;`hadoop fs -ls /` 等同于上一条命令的效果
* -mkdir 功能：在hdfs上创建目录 示例：`hadoop fs -mkdir -p /aaa/bbb/cc/dd`
* -moveFromLocal 功能：从本地剪切粘贴到hdfs 示例：hadoop fs - moveFromLocal /home/hadoop/a.txt /aaa/bbb/cc/dd
* -moveToLocal 功能：从hdfs剪切粘贴到本地 示例：hadoop fs - moveToLocal /aaa/bbb/cc/dd /home/hadoop/a.txt
* –appendToFile 功能：追加一个文件到已经存在的文件末尾 示例：hadoop fs -appendToFile ./hello.txt hdfs://hadoop-server01:9000/hello.txt 可以简写为： Hadoop fs -appendToFile ./hello.txt /hello.txt
* -cat 功能：显示文件内容 示例：hadoop fs -cat /hello.txt
* -tail 功能：显示一个文件的末尾 示例：hadoop fs -tail /weblog/access\_log.1
* -text 功能：以字符形式打印一个文件的内容 示例：hadoop fs -text /weblog/access\_log.1
* -chgrp
* -chmod
* -chown 功能：这三个命令跟linux文件系统中的用法一样，对文件所属权限 示例： hadoop fs -chmod 666 /hello.txt hadoop fs -chown someuser:somegrp /hello.txt
* -copyFromLocal 功能：从本地文件系统中拷贝文件到hdfs路径去 示例：hadoop fs -copyFromLocal ./jdk.tar.gz /aaa/
* -copyToLocal 功能：从hdfs拷贝到本地 示例：hadoop fs -copyToLocal /aaa/jdk.tar.gz
* -cp 功能：从hdfs的一个路径拷贝hdfs的另一个路径 示例： hadoop fs -cp /aaa/jdk.tar.gz /bbb/jdk.tar.gz.2
* -mv 功能：在hdfs目录中移动文件 示例： hadoop fs -mv /aaa/jdk.tar.gz /
* -get 功能：等同于copyToLocal，就是从hdfs下载文件到本地 示例：hadoop fs -get /aaa/jdk.tar.gz
* -getmerge 功能：合并下载多个文件 示例：比如hdfs的目录 /aaa/下有多个文件:log.1, log.2,log.3,… hadoop fs -getmerge /aaa/log.\* ./log.sum
* -put 功能：等同于copyFromLocal 示例：hadoop fs -put /aaa/jdk.tar.gz /bbb/jdk.tar.gz.2
* -rm 功能：删除文件或文件夹 示例：hadoop fs -rm -r /aaa/bbb/
* -rmdir 功能：删除空目录 示例：hadoop fs -rmdir /aaa/bbb/ccc
* -df 功能：统计文件系统的可用空间信息 示例：hadoop fs -df -h /
* -du 功能：统计文件夹的大小信息 示例： hadoop fs -du -s -h /aaa/\*
* -count 功能：统计一个指定目录下的文件节点数量 示例：hadoop fs -count /aaa/
* -setrep 功能：设置hdfs中文件的副本数量 示例：hadoop fs -setrep 3 /aaa/jdk.tar.gz

管理员：

```text
hadoop dfsadmin -report #报告
hadoop dfsadmin -safemode enter | leave | get | wait
hadoop dfsadmin -setBalancerBandwidth 1000
```

