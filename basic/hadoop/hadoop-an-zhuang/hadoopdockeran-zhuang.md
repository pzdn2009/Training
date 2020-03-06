# HadoopDocker安装

Ref：[https://github.com/kiwenlau/hadoop-cluster-docker](https://github.com/kiwenlau/hadoop-cluster-docker)

有很多种方式，这里先参考kiwenlau的docker化方式。

## 安装启动

1. pull docker image

   `sudo docker pull kiwenlau/hadoop:1.0`

2. clone github repository

   `git clone https://github.com/kiwenlau/hadoop-cluster-docker`

3. create hadoop network

   `sudo docker network create --driver=bridge hadoop`

4. start container

```text
cd hadoop-cluster-docker
sudo ./start-container.sh
output:

start hadoop-master container...
start hadoop-slave1 container...
start hadoop-slave2 container...
root@hadoop-master:~# 
start 3 containers with 1 master and 2 slaves
you will get into the /root directory of hadoop-master container
```

1. start hadoop

   `./start-hadoop.sh`

2. run wordcount

```text
output

input file1.txt:
Hello Hadoop

input file2.txt:
Hello Docker

wordcount output:
Docker    1
Hadoop    1
Hello    2
```

## Arbitrary size Hadoop cluster

扩容缩容

1. pull docker images and clone github repository
2. rebuild docker image `sudo ./resize-cluster.sh 5`

   specify parameter &gt; 1: 2, 3.. this script just rebuild hadoop image with different slaves file, which pecifies the name of all slave nodes。用于生成slaves的数量。

3. start container

   sudo ./start-container.sh 5

   use the same parameter as the step 2

4. run hadoop cluster

   do 5~6 like section A

