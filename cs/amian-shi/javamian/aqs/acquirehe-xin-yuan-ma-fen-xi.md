# 独占锁核心源码分析

![](/assets/SpinAcquire.jpg)

# 1. acquire—获取入口

```java
public final void acquire(int arg) {
    //获取锁成功，则返回
    //反之，加入队列，等待竞争
    if (!tryAcquire(arg) &&
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
        selfInterrupt();
}
```

## 1.1 tryAcquire——尝试获取

```java
/**
 * Fair version of tryAcquire.  Don't grant access unless
 * recursive call or no waiters or is first.
 */
protected final boolean tryAcquire(int acquires) {
    final Thread current = Thread.currentThread();
    int c = getState();
    if (c == 0) {//未同步时，
        //如果不需要入队
        if (!hasQueuedPredecessors() &&
            compareAndSetState(0, acquires)) {
            setExclusiveOwnerThread(current);
            return true;
        }
    }
    //同步状态被占用时，判断是否可重入
    else if (current == getExclusiveOwnerThread()) {
        int nextc = c + acquires;
        if (nextc < 0)
            throw new Error("Maximum lock count exceeded");
        setState(nextc);
        return true;
    }
    return false;
}
```

## 1.2 addWaiter——生成节点
```java
/**
 * Creates and enqueues node for current thread and given mode.
 *
 * @param mode Node.EXCLUSIVE for exclusive, Node.SHARED for shared
 * @return the new node
 */
private Node addWaiter(Node mode) {
    
    Node node = new Node(Thread.currentThread(), mode);
    // Try the fast path of enq; backup to full enq on failure
    
    //尾指针
    Node pred = tail;
    if (pred != null) {
        //新节点的前驱
        node.prev = pred;
        //CAS更新尾指针（注，这个地方不一定会成功）
        if (compareAndSetTail(pred, node)) {
            //连接后继
            pred.next = node;
            return node;
        }
    }
    
    //尾指针为空，或者CAS失败--》则创建等待队列
    enq(node);
    
    return node;
}
```

## 1.3. enq——节点入队尾

* spin
* 第一次创建链表头结点
* 第二次更新节点。

```java
/**
 * Inserts node into queue, initializing if necessary. See picture above.
 * @param node the node to insert
 * @return node's predecessor
 */
private Node enq(final Node node) {
    for (;;) { //自旋
        Node t = tail;
        if (t == null) { // Must initialize
            // 新建一个头结点，头指针和尾指针一样
            if (compareAndSetHead(new Node()))
                tail = head;
        } else {
            node.prev = t;
            //设置尾指针，CAS成功，则返回t
            if (compareAndSetTail(t, node)) {
                t.next = node;
                return t;
            }
        }
    }
}
```

## 1.4 acquireQueued——等待队列中自旋获取

```java
/**
 * Acquires in exclusive uninterruptible mode for thread already in
 * queue. Used by condition wait methods as well as acquire.独占非中断模式获取锁，针对已经在等待队列里面的节点。
 *
 * @param node the node
 * @param arg the acquire argument
 * @return {@code true} if interrupted while waiting
 */
final boolean acquireQueued(final Node node, int arg) {
    boolean failed = true;
    try {
        boolean interrupted = false;
        for (;;) { //自旋
            //获取前驱
            final Node p = node.predecessor();
            //如果是前驱是头结点，说明node要做好准备了，尝试获取
            if (p == head && tryAcquire(arg)) {
                //成功：设置当前节点为头结点
                setHead(node);
                //释放直接的头结点
                p.next = null; // help GC
                failed = false;
                return interrupted;
            }
            //判断是否要park，park函数是将当前调用Thread阻塞，
            //而unpark函数则是将指定线程Thread唤醒。即让出CPU。

            //park检查和中断检查
            //如果可以的话，则进行Park，并更新中断标记
            if (shouldParkAfterFailedAcquire(p, node) &&
                parkAndCheckInterrupt())
                interrupted = true;
        }
    } finally {
        //如果失败，则取消获取
        if (failed)
            cancelAcquire(node);
    }
}
```

## 1.5 shouldParkAfterFailedAcquire——获取不到是否应该park

```java
/**
 * Checks and updates status for a node that failed to acquire.
 * Returns true if thread should block. This is the main signal
 * control in all acquire loops.  Requires that pred == node.prev.
 *
 * @param pred node's predecessor holding status
 * @param node the node
 * @return {@code true} if thread should block
 */
private static boolean shouldParkAfterFailedAcquire(Node pred, Node node) {
    //前驱等待状态
    int ws = pred.waitStatus;
    if (ws == Node.SIGNAL)
        /*
         * This node has already set status asking a release
         * to signal it, so it can safely park.
         */
        //所谓安全停靠点，指的是，当前结点的前驱结点waitStatus = -1(SIGNAL)，下一个就是它了。
        return true;
    if (ws > 0) {
        //跳过所有已取消的前驱节点
        do {
            node.prev = pred = pred.prev;
        } while (pred.waitStatus > 0);
        pred.next = node;
    } else {
        /*
         * waitStatus must be 0 or PROPAGATE.  Indicate that we
         * need a signal, but don't park yet.  Caller will need to
         * retry to make sure it cannot acquire before parking.
         */
         //如果前节点不处于取消状态，则设为signal -1
        compareAndSetWaitStatus(pred, ws, Node.SIGNAL);
    }
    return false;
}
```

## 1.6 parkAndCheckInterrupt

```java
private final boolean parkAndCheckInterrupt() {
        LockSupport.park(this);//阻塞该线程，至此该线程进入等待状态，等着unpark和interrupt叫醒他
        return Thread.interrupted();//叫醒之后返回该线程是否在中断状态， 并会清除中断记号。

    }
```

## 1.7 cancelAcquire——取消获取

```java
private void cancelAcquire(Node node) {
    
    if (node == null)
        return;

    //擦除线程
    node.thread = null;

    // Skip cancelled predecessors
    //跳过已取消的前驱
    Node pred = node.prev;
    while (pred.waitStatus > 0)
        node.prev = pred = pred.prev;

    // predNext is the apparent node to unsplice. CASes below will
    // fail if not, in which case, we lost race vs another cancel
    // or signal, so no further action is necessary.
    Node predNext = pred.next;

    //取消
    node.waitStatus = Node.CANCELLED;

    //最后一个节点，设置队尾
    if (node == tail && compareAndSetTail(node, pred)) {
        compareAndSetNext(pred, predNext, null);
    } else {
        // If successor needs signal, try to set pred's next-link
        // 如果后继需要唤醒，尝试连接
        // so it will get one. Otherwise wake it up to propagate.
        int ws;
        //前驱不是头结点 && 前驱已经SIGNAL，或者即将是
        if (pred != head &&
            ((ws = pred.waitStatus) == Node.SIGNAL ||
             (ws <= 0 && compareAndSetWaitStatus(pred, ws, Node.SIGNAL))) &&
            pred.thread != null) {
            Node next = node.next;
            if (next != null && next.waitStatus <= 0)
                compareAndSetNext(pred, predNext, next); //连接起来
        } else {
            //唤醒后继
            unparkSuccessor(node);
        }

        node.next = node; // help GC
    }
}
```

## 2.7 unparkSuccessor——唤醒后继

```java
/**
 * Wakes up node's successor, if one exists.
 *
 * @param node the node
 */
private void unparkSuccessor(Node node) {
    /*
     * If status is negative (i.e., possibly needing signal) try
     * to clear in anticipation of signalling.  It is OK if this
     * fails or if status is changed by waiting thread.
     */
    int ws = node.waitStatus;
    if (ws < 0)
        compareAndSetWaitStatus(node, ws, 0);

    /*
     * Thread to unpark is held in successor, which is normally
     * just the next node.  But if cancelled or apparently null,
     * traverse backwards from tail to find the actual
     * non-cancelled successor.
     */
    Node s = node.next;
    if (s == null || s.waitStatus > 0) {
        s = null;
        for (Node t = tail; t != null && t != node; t = t.prev)
            if (t.waitStatus <= 0)
                s = t;
    }
    //从后面开始找到非取消的后继，
    //unPark这个后继
    if (s != null)
        LockSupport.unpark(s.thread);
}
```

## 2.8 release——释放锁

```java
//AQS的release
public final boolean release(int arg) {
        if (tryRelease(arg)) {
            Node h = head;
            if (h != null && h.waitStatus != 0)
                unparkSuccessor(h);//唤醒后继节点
            return true;
        }
        return false;
 }
 
//c=0,才完全
protected final boolean tryRelease(int releases) {
    //同步状态
    int c = getState() - releases;
    
    //当前线程要一致
    if (Thread.currentThread() != getExclusiveOwnerThread())
        throw new IllegalMonitorStateException();
    boolean free = false;
    if (c == 0) {
        free = true;
        setExclusiveOwnerThread(null);
    }
    setState(c);
    return free;
}
```