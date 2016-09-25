# 1.目标

- 安装Docker软件
- 运行容器
- Docker Hub，创建DockerHub账号
- 创建镜像和运行容器
- 创建自己的镜像
- 发布镜像

# 2.基础使用教程

**Hello world**
```
docker version #查看docker的安装版本
docker run hello-world #安装hello-world镜像，从docker hub下载，并运行
docker ps -a #查看docker容器实例进程，-a所有，包括退出状态的
```

**docker/whalesay**

docker hub 地址：https://hub.docker.com/

```
docker run docker/whalesay cowsay boo #运行docker/whalesay镜像，CMD为cowsay boo
docker images #查看镜像
docker run docker/whalesay cowsay boo-boo #传入boo-boo
```
**构建自己的镜像**
```
mkdir mydockerbuild
cd mydockerbuild
touch Dockerfile
vim Dockerfile #创建并编辑Dockerfile
```

_Dockerfile_
```
FROM docker/whalesay:latest #指定基镜像
RUN apt-get -y update && apt-get install -y fortunes #install
CMD /usr/games/fortune -a | cowsay #容器实例时，运行的命令
```
构建并运行
```
docker build -t docker-whale . #从当前目录进行构建，-t，Repository Name with an optional tagName,eg:latest.
docker run docker-whale #运行
```

**Tag, push, and pull**

```
docker images #显示所有镜像，找到需要tag的镜像——IMAGE ID字段
docker tag {IMAGE ID} pzdn2009/docker-whale:latest #IMAGE ID本地镜像名称，Account/ImageName。
docker login #登录到docker hub
docker push pzdn2009/docker-whale #推送到hub

docker rmi -f 7d9495d03763 #安装镜像ID删除
docker rmi -f docker-whale #按照镜像名称删除

docker run pzdn2009/docker-whale #从远程拉取运行
```

# 3.示例教程

## 3.1 Hello world
```
docker run -t -i ubuntu /bin/bash #在容器内启动/bin/bash
docker run -d ubuntu /bin/sh -c "while true;\
         do echo hello world; sleep 1; done"
docker ps # CONTAINER ID为f0bbb2636e53，容器短ID。NAME为trusting_fermat
docker logs trusting_fermat #查看容器日志
docker stop trusting_fermat #停止容器
docker ps #查看容器进程，即看不到trusting_fermat了
```
解释：
>-t :为容器分配一个伪终端或者终端

>-i :允许交互，使用stdin

>-d :守护进程运行，可以使用docker ps查看，通常返回容器ID。即f0bbb2636e53df51c6be82e22d7f7f304abd4f13a8d8ce0d83ad6d94c7cfd70a，长格式的。

# 3.2 运行Python web app

Docker CLI 格式
>\# Usage: [sudo] docker [subcommand] [flags] [arguments] ..

>\# Example:

>$ docker run -i -t ubuntu /bin/bash

帮助的使用
```
docker version #查看docker的版本
docker --help #查看帮助，即可打印所有的命令
docker attach --help #查看具体命令attach的帮助
```

Steps:
```
docker run -d -P training/webapp python app.py #端口映射
docker ps -l #最后一个容器
docker run -d -p 80:5000 training/webapp python app.py
#指定端口映射，0.0.0.0:32769->5000/tcp 
# Docker NAME: backstabbing_poincare
#visit http://localhost:32769 ,Hello world

docker port backstabbing_poincare [5000] #查看或者指定端口
docker logs -f backstabbing_poincare #查看请求日志
docker top backstabbing_poincare #输出容器的进程

docker inspect backstabbing_poincare #查看底层信息
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' backstabbing_poincare #查看指定的配置

docker stop backstabbing_poincare #停止容器
docker start backstabbing_poincare #启动容器
docker rm backstabbing_poincare #删除容器
```

解释：
>-P :暴露端口，主机端口范围32768 to 61000.

>-f :tail -f类似。


## 3.3 构建自己的Docker镜像

一般的拉取
```
docker images
docker run -t -i ubuntu:14.04 /bin/bash
docker pull centos
docker run -t -i centos /bin/bash
```

搜索并拉取
```
docker search sinatra #搜索
docker pull training/sinatra #拉取
```
_Dockerfile_
```
FROM ubuntu:14.04
MAINTAINER pzdn2009 1050244110@qq.com
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
```

build,tag,push
```
docker build -t pzdn2009/sinatra:v2 . #构建
docker run -t -i pzdn2009/sinatra:v2 /bin/bash #运行看一下
docker tag 0fc48b937557 pzdn2009/sinatra:devel #重新标记
docker push pzdn2009/sinatra #发布
```
