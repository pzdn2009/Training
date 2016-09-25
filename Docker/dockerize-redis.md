
# 1. 创建一个Redis容器

**_Dockerfile_**
```
FROM ubuntu:14.04
RUN apt-get update && apt-get install -y redis-server
EXPOSE 6379
ENTRYPOINT ["/usr/bin/redis-server"]
```
构建
```
docker build -t pzdn2009/redis .
```
# 2. 运行服务

```
docker run --name redis -d pzdn2009/redis
```
解释：
>--name redis，将容器命名为redis

# 3. 创建web应用容器
```
docker run --link redis:db -i -t ubuntu:14.04 /bin/bash
sudo apt-get update
sudo apt-get install redis-server
sudo service redis-server stop

env | grep DB_

redis-cli -h $DB_PORT_6379_TCP_ADDR
```
解释：
>--link redis:db，当前容器链接到redis，并采用db为名称，后面采用DB_来查询env配置。

# 4. 再次启动

```
docker start compassionate_ramanujan
docker attach compassionate_ramanujan
redis-cli -h $DB_PORT_6379_TCP_ADDR
```