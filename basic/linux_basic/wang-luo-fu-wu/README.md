# 网络、服务

## 网络、服务

* [mail服务](mailfu-wu.md)

## 1.网路

* 网络理论
* ifconfig命令
* iptables命令

## 2.服务

service与daemon，通常不做特别的区别，daemon类似Windows系统的服务。daemon的命名，通常以d结尾。

按启动类型分：

> 1. stand\_alone：自己启动
> 2. super daemon：需要一个管控daemon来启动，懒加载模式，完成之后释放资源。

按工作形态分：

> 1. signal-control：只要有客户端请求，就立即启动。
> 2. interval-control：每隔一段时间去做，如crontab

查看服务与端口的对应

```text
cat /etc/services
```

## 3.计划

crontab：定时。 格式：分 时 日 月 周 命令

```text
crontab -e #编辑cron数据
crontab -l #列出当前用户的cron
crontab -r #删除当前用户的cron
```

