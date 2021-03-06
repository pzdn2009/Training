# 软件包管理

## 软件包管理

软件包管理分为两大主流：dpkg——apt——debian,rpm——yum——redhat

## 1. rpm/yum

**rpm**

安装

* rpm -ivh xxx.rpm \# -i install -v verbose -h 进度

升级

* rpm -Uvh xxx.rpm \#-U 无则安装，有则更新
* rpm -Fvh xxx.rpm \#-F 无则不安装，有则升级

查询

* rpm -q xxx \#查询是否安装xxx
* rpm -qa \#查询所有已安装的软件名称
* rpm -qa \| grep xxx \#查找xxx
* rpm -qi \#列出该软件的information
* rpm -ql \#列出该软件的所有的文件与目录所在完整名，即安装路径相关
* rpm -qc \#列出该软件的所有设置文件 /etc/下找
* rpm -qf /etc/yum.conf \#查询指定的文件是属于哪个软件安装的
* rpm -qR \#列出与该软件有关的依赖软件所含的文件
* rpm -qp\[licdR\] \#p表示未安装的某个文件名称

卸载

* rpm -e xxx
* rpm -e xxx --noscripts \#卸载损坏的包

重建数据库

* rpm --rebuild

**yum**

安装

* yum install xxx \#安装xxx

升级

* yum update \#
* yum update xxx
* yum check-update
* yum upgrade

移除

* yum remove xxx \#删除xxx

清除

* yum clean all
* yum clean packages
* yum clean headers
* yum clean oldheaders

搜索

* yum search xxx \#搜索相关软件

列出

* yum list \#所有已安装的软件列表
* yum list xxx
* yum list updates
* yum list installed
* yum list extra

查询

* yum info
* yum info xxx \#xxx的软件功能
* yum info updates
* yum info installed
* yum info extras

仓库

* rpm -- import [http://rpm.livna.org/RPM-LIVNA-GPG-KEY](http://rpm.livna.org/RPM-LIVNA-GPG-KEY)

## 2. dpkg

**dpkg**

安装

* dpkg -i xxx.deb

移除

* dpkg -r xxx.deb

清除

* dpkg -P xxx.deb

列出

* dpkg -L xxx.deb
* dpkg -c xxx.deb

查询

* dpkg -s xxx.deb

**apt-get**

安装

* apt-get install xxx
* apt-get install xxx --reinstall
* apt-get -f install
* apt-get source xxx \#下载xxx的源文件

升级

* apt-get update \#更新源
* apt-get upgrade xxx
* apt-get dist-upgrade
* apt-get dselect-upgrade
* apt-get check

移除

* apt-get remove xxx
* apt-get remove xxx --purge

清除

* apt-get autoclean
* apt-get clean

搜索

* apt-cache search xxx
* apt-cache show xxx

## 3. 源码安装

配置\(configure\)、编译\(make\)、安装\(make install\)。

**configure选项**

--prefix可以配置安装的路径：如

```text
./configure --prefix=/usr/local/test
```

**常见步骤：**

```text
./configure && make && make install
```

安装完成了之后，清理临时文件

```text
make clean
```

卸载软件

```text
make uninstall
```

