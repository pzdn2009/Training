# 接入层

充当流量入口。

最典型的就是游Nginx分发。

至于Nginx之前是什么？

* Nginx直接部署在ECS——简化、单机。
* SLB直接将流量导入两台Nginx。
* 或者KeepAlive+Nginx

