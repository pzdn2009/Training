# 必要条件

![](../../../.gitbook/assets/osdeadlock.jpeg)

* 互斥：每个资源要么已经分配给了一个进程，要么就是可用的。
* 占有和等待：已经得到了某个资源的进程可以再请求新的资源。
* 不可抢占：已经分配给一个进程的资源不能强制性地被抢占，它只能被占有它的进程显式地释放。
* 环路等待：有两个或者两个以上的进程组成一条环路，该环路中的每个进程都在等待下一个进程所占有的资源。

主要有以下四种方法：

* 鸵鸟策略
* 死锁检测与死锁恢复
* 死锁预防
* 死锁避免

