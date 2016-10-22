# Compose简介

Compose是一个定义和运行多个容器Docker应用的工具。通过一个文件来配置应用服务。所以，通过一个单一命名，就可以创建和启动配置的所有服务。

基本的三个步骤：

1. 使用Dockerfile定义应用的环境；

2. 使用docker-compose.yml来定义服务

3. 运行docker-compose


示例:
```
version: '2'
services:
  web:
    build: .
  ports:
  - "5000:5000"
  volumes:
  - .:/code
  - logvolume01:/var/log
  links:
  - redis
  redis:
    image: redis
  volumes:
logvolume01: {}
```

特征：单一主机上的多隔离环境

