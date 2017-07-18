# 安装Docker

## 1. Ubuntu安装

**方式一**：

官方教程：https://docs.docker.com/engine/installation/linux/ubuntulinux/

中文翻译：
http://www.cnblogs.com/zzcit/p/5845717.html 

**方式二**：

如果使用阿里云source，直接执行
```
sudo apt-get install -y docker.io
ln -sf /usr/bin/docker.io /usr/local/bin/docker
sed -i `$acomplete -F _docker docker` /etc/bash_completion.d/docker.io
```

简单一点即：
```
sudo apt-get update
sudo apt-get install docker.io
sudo ln -sf /urs/bin/docker.io /usr/local/bin/docker
```


## 2.Ubuntu升级

```
sudo apt-get install apt-transport-https

# Add the Docker repository key to your local keychain
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

# Add the Docker repository to your apt sources list.
sudo sh -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"

# update your sources list
sudo apt-get update

# 之后通过下面命令来安装最新版本的docker：
apt-get install -y lxc-docker

# 以后更新则：
apt-get update -y lxc-docker
ln -sf /usr/bin/docker /usr/local/bin/docker
```

## 3.Centos

**centos7**

```
sudo yun install docker
sudo service docker start
sudo chkconfig docker on
```

源碼和bin下載地址：
https://github.com/moby/moby/releases
```shell
tar xzvf /path/to/<FILE>.tar.gz
sudo cp docker/* /usr/bin/
sudo dockerd &
sudo docker run hello-world
```
## 4.MAC

下载boot2docker来安装。
将包含 docker，docker-machine，docker-compose。

## 5.Docker Group

```
sudo groupadd docker
sudo usermod -aG docker $USER
sudo service docker restart
```

就不会提示一下信息：
>Cannot connect to the Docker daemon. Is 'docker daemon' running on this host?

原理：高版本的docker会将docker用户组设置为root权限的。

## 5. cn 鏡像
```shell
$ docker pull registry.docker-cn.com/myname/myrepo:mytag
$ docker pull registry.docker-cn.com/library/ubuntu:16.04

$ docker --registry-mirror=https://registry.docker-cn.com daemon

或者修改：/etc/docker/daemon.json
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```