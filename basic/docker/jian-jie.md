# 简介

## 1.Docker是什么

Docker是一个开发，包装，运行应用程序的开放平台。能够提高开发，部署速度。

## 2.什么是Docker引擎

c-s结构的应用程序，部件：

* 服务Daemon，以守护进程的方式运行。服务负责创建和管理Docker对象，比如镜像，容器，网络，数据卷。
* REST API，访问服务
* 命令行接口（CLI）客户端，CLI通过REST API来与Server进行交互。

![](../../.gitbook/assets/dockerengine.png)

## 3.用Docker能做什么？

* 快速、持续分发应用
* 快捷部署和扩展
* 相同硬件上执行更多的负载

## 4.Docker的架构？

C-S架构。服务执行高负载工作，构建、运行、分部容器。客户端通过socket或者REST API访问服务。

![](../../.gitbook/assets/dockerarchtecture%20%281%29.png)

### 4.1 Docker daemon

运行在主机上，使用Docker 客户端与其交互

### 4.2 Docker client

命令行方式与daemon交互。

### 4.3 Docker内部

* **Docker images**

一个只读的模板，用来创建Docker 容器。Dockerfile是镜像的一个描述。

* **Docker container**

镜像的一个实例。可以通过CLI运行、启动、停止、移动或者删除一个容器。

* **Docker registries**

Docker镜像仓库。

* **Docker services**

Docker服务允许Docker云集一起运行。

## 5.Docker镜像怎么工作？

Docker镜像是一个只读模板，用于容器的初始化。

每个镜像有多个**层**（layer）组成。Docker使用UnionFS来讲这些层联合成为一个镜像。UnionFS允许它可以把多个目录\(也叫分支\)内容合并在一起, 而目录的物理位置是分开的。这也是Docker比较轻量级的原因。

Dockerfile给出了镜像的定义。每个镜像从一个基镜像开始。使用FROM关键字标志。

**Dockerfile**指令：

* 指定一个基镜像\(**FROM**\)
* 表明维护人 \(**MAINTAINER**\)
* 运行命令 \(**RUN**\)
* 添加文件和目录 \(**ADD**\)
* 创建环境变量 \(**ENV**\)
* 当启动一个容器示例时，执行的进程\(**CMD**\)

## 6.Docker registry怎么工作？

Docker Hub，公共Docker注册。用于存储。发布，拉取。类似GitHub

Docker Store，Docker镜像的买卖。

## 7. 容器如何工作？

容器使用Linux内核的主机运行。

当执行 docker run命令时，Docker daemon运行一个容器。

```text
$ docker run -i -t ubuntu /bin/bash
```

Steps:

1. 拉取ubuntu镜像：如果本地存在，直接使用本地的镜像；反之，从Docker Hub上拉取。
2. 创建一个新的容器：使用拉取的镜像创建容器。
3. 分配文件系统并挂载一个读写层：os上创建容器，一个可读写的层将添加到镜像。
4. 分配网络/桥接接口：创建一个网络接口，让容器与主机通信。
5. 安装IP地址：从池中找到一个可用的IP地址。
6. 执行指定的进程：运行/bin/bash。
7. 捕获程序输出：本例中配置了标注输出。

## 8.底层技术

* **Namespaces**命名空间

常用的命名空间：pid，net，ipc，mnt，uts

* **Control groups**控制组
* **Union file systems** 联合文件系统
* **Container format** 容器格式化

