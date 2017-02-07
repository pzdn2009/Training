# systemctl用法

systemctl 是系统服务管理器命令，它实际上将 service 和 chkconfig 这两个命令组合到一起。

_**格式**：systemctl 动作 服务名.service_

动作：
- start
- stop
- restart
- status
- enable
- disable
- is-enabled

命令片段-1：
```
systemctl start nfs-server.service #启动
systemctl enable nfs-server.service #自动启动
systemctl disable nfs-server.service #停止开机自动启动
systemctl status nfs-server.service #查看当前服务的状态
systemctl status httpd.service

systemctl restart nfs-server.service #重启服务
systemctl list -units --type=service #查看所有已启动的服务
systemctl restart httpd.service #重启服务
systemctl is-active httpd.service #是否 Active
```

命令片段-2：#彻底关闭防火墙
```
sudo systemctl status  firewalld.service
sudo systemctl stop firewalld.service          
sudo systemctl disable firewalld.service

```


