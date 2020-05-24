# IO

关键字：read系统调用，write系统调用，内核缓冲区，进程缓冲区，SocketIO，文件IO，阻塞，同步，多路复用，Reactor，Selector，epoll，文件描述符。

* 阻塞IO：内核IO彻底操作完成之后，才能到达用户空间执行用户操作。
* 同步IO：用户空间的线程是主动发起IO的一方。
* 异步IO：内核是主动发起IO的一方。

* 内核：奶茶店
* 用户：路人甲，路人乙，路人丙
* 数据：奶茶
* 读：拿奶茶
* 写：付钱

四种IO：
* **BIO**：Blocking IO，同步阻塞IO
 * 路人甲去奶茶店拿奶茶，要一直等带奶茶店做好
 * 等待奶茶做好：内核准备
 * 等待拿到奶茶：复制
* **NBIO**：Non-Blocking IO，同步非阻塞IO
 * 路人甲去奶茶店拿奶茶，奶茶店直接告诉一个结果，Y，表示，可以拿走，N，表示，奶茶还没有做好。
 * 自旋
 * 有数据了，阻塞得拿走
* **IO多路复用**：IO Multiplexing。
 * 查询IO的就绪状态
 * 选择器注册、就绪状态轮训、获得就绪并系统调用、复制
 * select/epoll系统调用：阻塞
 * IO操作：read,write，阻塞
* AIO：Asynchronous IO，异步IO，类似回调。
 * read, 离开
 * 内核：准备、复制
 * signal通知 用户线程
 * 用户线程得到信号，去完成。
 
```
ulimit -n
```

## Java NIO

两类IO：
* NIO：New IO
 * 任意位置，面向缓冲区
 * 非阻塞（多路复用）
* OIO：Old IO
 * 顺序读取，面向流
 * 阻塞

三大核心组件：
* Channel
 * 与Buffer打交道
 * read,write
* Buffer
* Selector
 * IO事件

## Buffer

* 抽象类
* 8种：ByteBuffer，IntBuffer。。。，MappedByteBuffer
* 属性：
```java
    // Invariants: mark <= position <= limit <= capacity
    //保存position
    private int mark = -1;
    //数据的位置 = [0,limit-1]
    //和读写模式有关，具有移动性
    private int position = 0;
    //读写的上界，实际的数据量
    private int limit;
    //最大容量，数据的个数
    private int capacity;
```
* 方法：allocate,put,flip,get,rewind，mark(pos),reset(pos)
* 分为读写模式：write --> flip --> read --> clean/compact --> write

## Channel

四种主要的：
* FileChannel
* SocketChannel
 * TCP读写
* ServerSocketChannel
 * 服务端
 * 监听TCP
 * 创建SocketChannel
* DatagramChannel
 * UDP读写
 
## Selector

* 多路，多Channel
