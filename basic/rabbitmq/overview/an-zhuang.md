# 安裝

## 安裝

Steps:

* 下載服務器包
* 解壓
* 設置文件目錄
* 啟動RabbitMQ服務器
* 檢查服務狀態

## 下載服務器包 & 解壓

```text
wget http://www.rabbitmq.com.releases/rabbitmq-server/v2.7.0/rabbitmq-server-generic-unix-2.7.0.tar.gz

tar -vxzf rabbitmq-server-generic-unix-2.7.0.tar.gz

yum install erlang 
apt-get install erlang

mkdir -p /var/log/rmq
mkdir -p /var/lib/rmq/mnesia/rmq

sbin/rabbitmq-server
sbin/rabbitmqctl status
```

