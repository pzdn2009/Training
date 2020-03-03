# AQS-FairSync

* 公平锁同步

```java
/**
 * Sync object for fair locks
 */
static final class FairSync extends Sync {
    private static final long serialVersionUID = -3000897897090466540L;

    final void lock() {
        acquire(1);
    }

    /**
     * Fair version of tryAcquire.  Don't grant access unless
     * recursive call or no waiters or is first.
     */
    protected final boolean tryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        //读取同步状态
        int c = getState();
        if (c == 0) { //未同步
            //如果不需要入队，则CAS修改同步状态
            if (!hasQueuedPredecessors() &&
                compareAndSetState(0, acquires)) {
                //设置独占拥有线程
                setExclusiveOwnerThread(current);
                //获取成功
                return true;
            }
        }
        //已同步，判断是否独占拥有线程
        else if (current == getExclusiveOwnerThread()) {
            //判断是否继续重入
            int nextc = c + acquires;
            if (nextc < 0)
                throw new Error("Maximum lock count exceeded");
            setState(nextc);
            return true;
        }
        
        return false;
    }
}
```


```java

//aqs
public final boolean hasQueuedPredecessors() {
    // The correctness of this depends on head being initialized
    // before tail and on head.next being accurate if the current
    // thread is first in queue.
    Node t = tail; // Read fields in reverse initialization order
    Node h = head;
    Node s;
    return h != t &&
        ((s = h.next) == null || s.thread != Thread.currentThread());
}
```
* 队头和队尾为空，还不存在，返回false；
* 队头不等与队尾，说明有节点存在，继续：
 * 如果只有一个节点，即头结点，需要入队，返回true；
 * 如果有多个节点，头节点的下一个节点，不是自己，需要入队，返回true。
 
 