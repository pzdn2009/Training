# 私有仓库

## 1. 创建

使用registry镜像

```text
sudo docker run -d -P 5000:5000 registry
sudo docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry registry

pzdn@ubuntu:~$ docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry --name=localRegistry registry
53c779ce4f481b36588e5a2c13957e6e59323fb6c4ba583931add579f825350b
```

## 2. 管理

push:

```text
sudo docker images
sudo docker tag ubuntu:14.04 localhost:5000/ubuntu:14.04
sudo docker images
sudo docker push localhost:5000/ubuntu:14.04
sudo docker pull localhost:5000/ubuntu:14.04
```

## 3. 配置insecure-registries

编辑：/etc/docker/daemon.json

```javascript
{
  "registry-mirrors": ["https://qm4r8mwd.mirror.aliyuncs.com","http://57326c54.m.daocloud.io"],
  "hosts": [
        "unix:///var/run/docker.sock"
   ],
   "insecure-registries":["ip:5000"]
}
```

重启服务：

```bash
systemctl daemon-reload
systemctl restart docker
```

