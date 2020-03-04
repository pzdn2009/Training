# Monitor

## Monitor

每个对象都有一个Monitor对象相关联，Monitor对象中记录了持有锁的线程信息、等待队列等。Monitor对象包含以下三个字段：

* \_owner 记录当前持有锁的线程
* \_EntryList 是一个队列，记录所有阻塞等待锁的线程
* \_WaitSet 也是一个队列，记录调用 wait\(\) 方法并还未被通知的线程

当线程持有锁的时候，线程id等信息会拷贝进owner字段，其余线程会进入阻塞队列entrylist，当持有锁的线程执行wait方法，会立即释放锁进入waitset，当线程释放锁的时候，owner会被置空，公平锁条件下，entrylist中的线程会竞争锁，竞争成功的线程id会写入owner，其余线程继续在entrylist中等待。

## Monitor与Synchronized

对于Synchronized的同步代码块，JVM会在进入代码块之前加上**monitorenter** ，如果进入monitor成功，线程便获取了锁，一个对象的monitor同一时刻只能被一个线程锁占有；

对于同步方法，JVM会讲方法设置 **ACC\_SYNCHRONIZED** 标志，调用的时候 JVM 根据这个标志判断是否是同步方法。

采用Synchronized给对象加锁会使线程阻塞，因而会造成线程状态的切换，而线程状态的切换必须要操作系统来执行，因此需要将用户态切换为内核态，这个切换的过程是十分耗时的都需要操作系统来帮忙，有可能比用户执行代码的时间还要长。

