# Volume最佳实践

Ref：[https://www.cnblogs.com/edisonchou/p/docker\_volumes\_introduction.html](https://www.cnblogs.com/edisonchou/p/docker_volumes_introduction.html)

## 挂载方式

* volumes：存储在`/var/lib/docker/volumes`，匿名卷；
* bind mounts：绑定主机的任一目录

## volumes方式

```text
docker volume create edc-nginx-vol
docker volume ls
docker volume inspect edc-nginx-vol
docker volume rm edc-nginx-vol
```

挂载点：`/var/lib/volumes/edc-nginx-vol/_data`

```text
docker run -d -it --name=edc-nginx -p 8800:80 -v edc-nginx-vol:/usr/share/nginx/html nginx
```

* -v 代表挂载数据卷，这里使用自定数据卷edc-nginx-vol，并且将数据卷挂载到 /usr/share/nginx/html 

## bind mounts方式

即常见的host:container方式。

host:container说明：

* host机器的目录路径必须为**全路径**\(准确的说需要以/或~/开始的路径\)，不然docker会将其当做volume而不是volume处理
* 如果host机器上的目录不存在，docker会自动创建该目录
* 如果container中的目录不存在，docker会自动创建该目录
* **如果container中的目录已经有内容，那么docker会使用host上的目录将其覆盖掉**

