# 安裝elasticsearch-head

## 安裝elasticsearch-head

head插件是用來管理Elasticsearch集群的。

* [https://github.com/mobz/elasticsearch-head\#running-with-built-in-server](https://github.com/mobz/elasticsearch-head#running-with-built-in-server)

## 1. 不同的版本

* 5.x:版本

  不支持插件安装。要安装为独立的服务。

* 2.x – 4.x版本:

  ```text
  sudo elasticsearch/bin/plugin install mobz/elasticsearch-head
  ```

* 1.x:版本

  ```text
  sudo elasticsearch/bin/plugin -install mobz/elasticsearch-head/1.x
  ```

* 0.9版本:

  ```text
  sudo elasticsearch/bin/plugin -install mobz/elasticsearch-head/0.9
  ```

## 2. 5.x独立服务

* elasticsearch中启用跨域

  ```text
  # elasticsearch.yml
  http.cors.enabled : true  
  http.cors.allow-origin: "/.*/"
  http.cors.allow-headers : "X-Requested-With,X-Auth-Token,Content-Type, Content-Length, Authorization, kbn-version"
  ```

* git clone git://github.com/mobz/elasticsearch-head.git
* cd elasticsearch-head
* npm install
* grunt server
* open [http://localhost:9100/](http://localhost:9100/)

## 3. 遇到的问题

### 3.1 git clone很慢

git clone 会复制整个gitdb，故采用下载压缩吧的方式。

下载压缩吧之后使用unzip命令解压缩

### 3.2 npm install 很慢

一开始是没有npm，直接

```text
yum install npm
```

之后试了很多次都很慢，网上搜索了一下，使用taobao的镜像就很快了。

```text
npm install --registry https://registry.npm.taobao.org
npm install -g grunt-cli --registry=http://registry.npm.taobao.org # 安装grunt-cli
```

### 3.3 下载phantomjs-2.1.1-linux-x86\_64.tar.bz2 很慢

下载的时候卡死了，然后下下载了

```text
wget https://github.com/Medium/phantomjs/releases/download/v2.1.1/phantomjs-2.1.1-linux-x86_64.tar.bz2 /tmp/phantomjs/phantomjs-2.1.1-linux-x86_64.tar.bz2
```

重新解压执行npm install 即可。

