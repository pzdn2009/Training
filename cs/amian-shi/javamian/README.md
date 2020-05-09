# Java面

* String，StringBuilder，StringBuffer
* 线程池原理
* HashMap原理

# 1. 并发编程面试要点

## 1.1 线程

* volatile的语义，它修饰的变量一定线程安全吗？
 * 可见性
 * 禁止指令重排序
 * 非原子操作即不安全
* ThreadLocal原理，与注意点
 * Local：当前线程所拥有的独立数据副本
 * 存储：ThreadLocalMap，此对象被`Thread.threadLocals`所引用
 * 内存泄漏：key为弱引用，GC时，会导致`key,value --> (null, value)`，而在`get,set,remove`时均做检查。
* 线程的状态
 * NRBWTT
 * NEW：start
 * RUNNABLE：（Ready,Running）
 * BLOCKED：Synchronized
 * WAITING：`Object.wait(),Thread.join(),Locksupport.park()`，反之：`Object.notify,notifyAll(),unpark(Thread)`
 * TIMED_WAITING：`sleep(long),wait(long),join(long),parkNanos(),partUtil()`，反之：`Object.notify,notifyAll(),unpark(Thread)`
 * TERMINATED

## 1.2 线程池

* [线程池原理](/cs/amian-shi/javamian/xian-cheng-chi-yuan-li.md)
 * 线程池的种类，区别和使用场景
 * 线程池如何调优
 * 线程池的最大线程数目根据什么确定

## 1.3 锁

* Synchronized和Lock的区别
 * 可重入
 * 可中断
* CAS是什么，会产生什么问题，怎么解决？
 * campare And swap
 * 内存值进行比较
 * ABA问题：第三者插足
 * 使用版本号解决
* AQS实现原理
* 锁的种类
* 锁的升级
* 公平锁与非公平锁，实现原理
* [JAVA对象头](/cs/amian-shi/javamian/dui-xiang-tou.md)

## 1.4工具包

* LockSupport工具
* countdownLatch
* cyclicbarrier

