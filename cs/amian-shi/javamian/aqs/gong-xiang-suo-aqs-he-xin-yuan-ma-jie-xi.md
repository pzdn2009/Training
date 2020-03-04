# 共享锁AQS核心源码解析

最主要的区别在于同一时刻能否有多个线程同时获取到同步状态.

在唤醒时候，多个等待共享锁的线程同时唤醒。

## acquireShared——获取共享锁

```java
public final void acquireShared(int arg) {
        if (tryAcquireShared(arg) < 0)
            doAcquireShared(arg);
    }
```

## tryAcquireShared——尝试获取共享锁

```java
protected final int tryAcquireShared(int unused) {
    /*
     * Walkthrough:
     * 1. If write lock held by another thread, fail.
     * 2. Otherwise, this thread is eligible for
     *    lock wrt state, so ask if it should block
     *    because of queue policy. If not, try
     *    to grant by CASing state and updating count.
     *    Note that step does not check for reentrant
     *    acquires, which is postponed to full version
     *    to avoid having to check hold count in
     *    the more typical non-reentrant case.
     * 3. If step 2 fails either because thread
     *    apparently not eligible or CAS fails or count
     *    saturated, chain to version with full retry loop.
     */
    Thread current = Thread.currentThread();
    int c = getState();
    //写锁被别的线程占有，直接失败
    if (exclusiveCount(c) != 0 &&
        getExclusiveOwnerThread() != current)
        return -1;

    //读锁的数量
    int r = sharedCount(c);
    //不需要阻塞读
    if (!readerShouldBlock() &&
        r < MAX_COUNT &&
        compareAndSetState(c, c + SHARED_UNIT)) {

        //
        if (r == 0) {
            firstReader = current;
            firstReaderHoldCount = 1;
        } else if (firstReader == current) {
            firstReaderHoldCount++;
        } else {
            HoldCounter rh = cachedHoldCounter;
            if (rh == null || rh.tid != getThreadId(current))
                cachedHoldCounter = rh = readHolds.get();
            else if (rh.count == 0)
                readHolds.set(rh);
            rh.count++;
        }
        return 1;
    }
    return fullTryAcquireShared(current);
}
```

## fullTryAcquireShared

## doAcquireShared——入队

```java
private void doAcquireShared(int arg) {
        //将当前节点加入到等待队列中去并且设为共享模式
        final Node node = addWaiter(Node.SHARED);
        boolean failed = true;
        try {
            boolean interrupted = false;
            for (;;) {
                final Node p = node.predecessor();  //获取当前节点的前驱节点
                if (p == head) {    //如果前驱结点是首节点
                    int r = tryAcquireShared(arg);  //尝试获取资源
                    if (r >= 0) {   //获取资源成功
                        //设置node节点为首节点
                        setHeadAndPropagate(node, r);//源码解析p2-1
                        p.next = null; // help GC
                        if (interrupted)    //如果等待过程有打断此时补上
                            selfInterrupt();
                        failed = false;
                        return;
                    }
                }
                //shouldParkAfterFailedAcquire(p, node)方法清除当前节点之前waitStatus>0的节点(已经标注被取消的节点)。。。
                //并返回false，返回false意味着当前节点无法被挂起，继续进行自旋操作直到可以被挂起或者成为首节点
                //如果返回true，则直接被挂起
                //parkAndCheckInterrupt方法的作用是挂起当前线程，此时从自旋中退出
                if (shouldParkAfterFailedAcquire(p, node) &&
                    parkAndCheckInterrupt())    //继续挂起
                    interrupted = true;
            }
        } finally {
            if (failed) //如果循环获取失败则取消排队
                cancelAcquire(node);//取消正在进行的获取
        }
    }
```

当前节点的前驱节点是头结点，尝试获取同步资源并唤醒被挂起的node节点

一个是当当前节点的前驱节点不是头结点，则清除无效的前驱节点并尝试挂起当前线程

## setHeadAndPropagate

```java
private void setHeadAndPropagate(Node node, int propagate) {
        Node h = head; // Record old head for check below
        setHead(node);//设置node为头节点

        //这里需要特别注意，h是node成为首节点之前的首节点，当h的状态为PRPPAGATE的时候可以进行唤起操作，但是
        //在唤起操作中取的是node节点，并不是这个h了
        if (propagate > 0 || h == null || h.waitStatus < 0 ||
                (h = head) == null || h.waitStatus < 0) {   //如果可以继续传播
            Node s = node.next; //获取node节点的下一个节点
            if (s == null || s.isShared())  //如果s是空或者是共享模式
                doReleaseShared();  //P3
        }
    }
```

## doReleaseShared

```java
private void doReleaseShared() {
        for (;;) {
            Node h = head;
            if (h != null && h != tail) {   //当前节点不为空且不为尾节点(表示不止一个节点)
                int ws = h.waitStatus;  //获取首节点的状态
                if (ws == Node.SIGNAL) {    //如果头节点可以唤起后继结点
                    if (!compareAndSetWaitStatus(h, Node.SIGNAL, 0)) //将h节点的状态更改为0，然后唤醒h节点
                        continue;            // 循环复查
                    unparkSuccessor(h); //P3-1:唤醒当前节点的下一个可用节点
                }
                //将waitStatus设置为可传播
                else if (ws == 0 &&
                        !compareAndSetWaitStatus(h, 0, Node.PROPAGATE))
                    continue;                // loop on failed CAS
            }
            if (h == head)                   // loop if head changed
                break;
        }
    }
```

