# 1. 官方教程

**_Dockerfile_**
```
FROM ubuntu:16.04
MAINTAINER Sven Dowideit <SvenDowideit@docker.com>
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:screencast' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config 

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
```

解释：
>RUN echo 'root:screencast' | chpasswd，修改密码为screencast

>sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config ,运行Root使用SSH登录

>

# 2.构建运行
```
docker build -t eg_sshd . 
docker run -d -P --name test_sshd eg_sshd 
docker port test_sshd 22  
$ ssh root@192.168.1.2 -p 49154 
# The password is ``screencast``. 
root@f38c87f2a42d:/# 
```

# 3.来自网络

```
FROM ubuntu:14.04
MAINTAINER pzdn2009 <1050244110@qq.com>

RUN echo "deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse" > /etc/apt/sources.list
RUN echo "deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse" >> /etc/apt/sources.list

RUN apt-get update
RUN echo "Asia/Shanghai" > /etc/timezone && \
 dpkg-reconfigure -f noninteractive tzdata

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

ADD authorized_keys /root/.ssh/authorized_keys

# 取消pam限制
RUN sed -ri 's@session\s*required\s*pam_loginuid.so@#session required pam_loginuid.so@g' /etc/pam.d/sshd

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
```
authorized_keys来自：
```
ssh-kengen -t rsa
cat ~/.ssh/id_rsa.pub > authorized_keys
```