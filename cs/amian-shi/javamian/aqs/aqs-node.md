# AQS-Node

* 链表结构.
* CLH锁是一个自旋锁，能确保无饥饿性，提供先来先服务的公平性。
* 包含：独占模式 & 共享模式


```java
//The wait queue is a variant of a "CLH" (Craig, Landin, and  Hagersten) lock queue.
static final class Node {
    // 标记一个结点（对应的线程）在共享模式下等待
    static final Node SHARED = new Node(); 
    
    // 标记一个结点（对应的线程）在独占模式下等待
    static final Node EXCLUSIVE = null; 
    
    // waitStatus的值，表示该结点（对应的线程）已被取消
    static final int CANCELLED = 1;
    // waitStatus的值，表示后继结点（对应的线程）需要被唤醒 
    static final int SIGNAL = -1; 
    // waitStatus的值，表示该结点（对应的线程）在等待某一条件
    static final int CONDITION = -2; 
    // waitStatus的值，表示有资源可用，新head结点需要继续唤醒后继结点（共享模式下，多线程并发释放资源，而head唤醒其后继结点后，需要把多出来的资源留给后面的结点；设置新的head结点时，会继续唤醒其后继结点）
    static final int PROPAGATE = -3; 
    
    // 等待状态，取值范围，-3，-2，-1，0，1
    volatile int waitStatus; 
    volatile Node prev; // 前驱结点
    volatile Node next; // 后继结点
    volatile Thread thread; // 结点对应的线程
    
    // 等待队列里下一个等待条件的结点
    Node nextWaiter; 

    final boolean isShared() {
        return nextWaiter == SHARED;
    }

    // 前驱结点
    final Node predecessor() throws NullPointerException { 
        Node p = prev;
        if (p == null)
            throw new NullPointerException();
        else
            return p;
    }

    Node() { // 初始化head或share标记结点
    }

    Node(Thread thread, Node mode) { // 同步队列
        this.nextWaiter = mode;
        this.thread = thread;
    }

    Node(Thread thread, int waitStatus) { // 等待队列
        this.waitStatus = waitStatus;
        this.thread = thread;
    }
}
```