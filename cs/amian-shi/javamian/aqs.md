# AQS 源码详解

# 类图分析

## 全景图

![](/assets/JUC.locks.all.png)


## ReetrantLock

![](/assets/ReentrantLock.png)
* 有三个内部类：主要是公平锁和非公平锁的实现
* 两把锁均是AQS
* 出现大量的`sync.methodXXX`，如果归类，那么就是代理模式的范畴


## ReetrantReadWriteLock

![](/assets/ReentrantReadWriteLock.png)

## 等待队列

属性：
```java
/**
 * Head of the wait queue, lazily initialized.  Except for
 * initialization, it is modified only via method setHead.  Note:
 * If head exists, its waitStatus is guaranteed not to be
 * CANCELLED.
 */
private transient volatile Node head;

/**
   队尾，只能通过enq方法添加一个节点
 */
private transient volatile Node tail;

/**
 * 同步状态
 */
private volatile int state;
```

* 本质上是一个双端队列，head & tail;
* 使用双向链表实现
* 链表有一个头节点概念

