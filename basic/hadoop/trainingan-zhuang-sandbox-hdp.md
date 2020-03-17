# Training-安装sandbox-hdp

# 1. 缘起


当学到Hive教程的时候，看到有可视化查询Zepperlin Notebook，联想到Python Notebook的方便，于是计划安装此界面。

刚好前几天在微信读书读了一本《企业大数据平台构建 架构与实现》(Author：朱凯)，详细讲了Ambari的安装与配置，也就有了这个尝试的过程。

我阿里云有一台机器4core16g，又不想打乱之前的机器环境，于是寻找Docker版本的。找到了一个github的项目https://github.com/sequenceiq/docker-ambari，

通过教程，很方便地启动了1master，2salve，Ambari总算docker方式启动了，但是在安装过程中，没有采用Local Repository的方式，为什么不用Local Repository方式呢？因为下载HDP-2.6.3的包，也是10+G，还要部署httpd服务作为本地yum源，感觉很折腾。决定联网安装，public-repo-1.hortonworks.com，最终，网络问题，折腾了几天，以多次失败收场，但是整个过程还是有收获的。

知道了hortonworks，于是去docker hub 和 hortonworks 去搜索，发现有个sandbox的东西，还是单机版，那接着尝试，阿里云上开启了sandbox的安装之路，最后还是失败，原因是我的磁盘空间不够，剩余9G，但是至少需要20G。

忍痛放弃，回到本地Mac，规划了10Gmem + 4core。走上了累死本地机器的路。pull 镜像，一个晚上，还是没有成功，于是寻找国内镜像，最终阿里镜像地址拯救了我，不到30分钟的时候，整个镜像就起来了。

耗时那么多天，终于成功了，必须记下来。

![](/assets/Ambari-Local.png)

# 2. Docker环境与加速

由于多次下载尝试，均失败了，考虑使用国内镜像加速。

* Docker环境
* 阿里镜像加速

## 2.1 Docker环境

我本地机器是Mac，已经具备了Docker环境。

```
pzdn$ docker -v
Docker version 19.03.5, build 633a0ea
```

## 2.2 阿里云镜像加速

主要参考了以下两个链接：
https://www.jianshu.com/p/81bf5efff8e0 [——获得Ali镜像地址]
https://blog.csdn.net/weixin_41806245/article/details/80466066     
[——Docker配置]

![配置镜像加速地址](/assets/Ali-Docker-Acc.png)

```
vim /Users/pzdn/.docker/daemon.json

{ "experimental":false,
 "debug":true,
 "registry-mirrors":["https://*****.mirror.aliyuncs.com"]
}
```

配置好了，速度杠杠的。

# 3. 拉取镜像

官方地址：https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html

下载还要注册cloudera，我直接使用了gmail注册了（翻墙的必要性）。

![](/assets/Amb-Version-select.png)

我选择了2.6.5的，下载下来执行：

```
sh docker-deploy-hdp265.sh 
```


# 4. 启动

参考：https://blog.csdn.net/zuochao_2013/article/details/71339934

* 登录8080发现用户名密码不对
* 登录4200，(root,hadoop)进入Container，执行`ambari-admin-password-reset`修改Ambari的密码。
* 重新登录8080，就可以进去了，一大个盘就出来了
* 慢慢等本地的服务启动完毕。

 
