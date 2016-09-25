
# 1. 安装

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

# 2.升级

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