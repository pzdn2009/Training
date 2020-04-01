* yarn简介
* yarn组件结构
* [yarn提交过程分析](/basic/hadoop/yarn/yarnti-jiao-guo-cheng-fen-xi.md)
* [yarn调度器](/basic/hadoop/yarn/yarn-schedulers.md)
* [yarn参数配置](/basic/hadoop/yarn/yarncan-shu-pei-zhi.md)

# 1. yarn简介

YARN是一个集群资源的管理与任务调度的分布式框架。

Hadoop YARN: A framework for job scheduling and cluster resource management.

![](/assets/yarn.webp)

在Yarn中我们把`job`的概念换成了`application`。

# 2. YARN组件架构

![](/assets/yarn2.webp)

Yarn主要由以下几个组件组成：

1. ResourceManager：Global（全局）的进程
2. NodeManager：运行在每个节点上的进程
3. ApplicationMaster：`Application-specific`（应用级别）的进程
 - Scheduler：是ResourceManager的一个组件
 - Container：节点上一组`CPU`和`内存`资源

## 2.1 Container

Container是Yarn框架的计算单元，是具体执行应用`task`（如map task、reduce task）的基本单位。

Container和集群节点的关系是：一个节点会运行多个Container，但一个Container不会跨节点。

一个Container就是一组分配的系统资源，现阶段只包含两种系统资源（之后可能会增加磁盘、网络等资源）：

1. CPU core
2. Memory in MB


>既然一个Container指的是具体节点上的计算资源，这就意味着Container中必定含有计算资源的位置信息：计算资源位于哪个机架的哪台机器上。
所以我们在请求某个Container时，其实是向某台机器发起的请求，请求的是这台机器上的CPU和内存资源。

任何一个job或application必须运行在一个或多个Container中，在Yarn框架中，ResourceManager只负责告诉ApplicationMaster哪些Containers可以用，ApplicationMaster还需要去找NodeManager请求分配具体的Container。

## 2.2 Node Manager

NodeManager进程运行在集群中的节点上，每个节点都会有自己的NodeManager。

NodeManager是一个`slave`服务：它负责接收ResourceManager的资源分配请求，分配具体的Container给应用。同时，它还负责监控并报告Container使用信息给ResourceManager。通过和ResourceManager配合，NodeManager负责整个Hadoop集群中的资源分配工作。

ResourceManager是一个全局的进程，而NodeManager只是每个节点上的进程，管理这个节点上的资源分配和监控运行节点的健康状态。下面是NodeManager的具体任务列表：

- 接收ResourceManager的请求，分配Container给应用的某个任务

- 和ResourceManager交换信息以确保整个集群平稳运行。ResourceManager就是通过收集每个NodeManager的报告信息来追踪整个集群健康状态的，而NodeManager负责监控自身的健康状态。

- 管理每个Container的生命周期

- 管理每个节点上的日志

- 执行Yarn上面应用的一些额外的服务，比如MapReduce的shuffle过程

当一个节点启动时，它会向ResourceManager进行注册并告知ResourceManager自己有多少资源可用。在运行期，通过NodeManager和ResourceManager协同工作，这些信息会不断被更新并保障整个集群发挥出最佳状态。

NodeManager只负责管理自身的Container，它并不知道运行在它上面应用的信息。负责管理应用信息的组件是ApplicationMaster，在后面会讲到。

## 2.3 Resource Manager

ResourceManager主要有两个组件：`Scheduler`和`ApplicationManager`。

Scheduler是一个资源调度器，它主要负责协调集群中各个应用的资源分配，保障整个集群的运行效率。Scheduler的角色是一个纯调度器，它只负责调度Containers，不会关心应用程序监控及其运行状态等信息。同样，它也不能重启因应用失败或者硬件错误而运行失败的任务

Scheduler是一个可插拔的插件，它可以调度集群中的各种队列、应用等。在Hadoop的MapReduce框架中主要有两种Scheduler：Capacity Scheduler和Fair Scheduler，关于这两个调度器后面会详细介绍。

另一个组件ApplicationManager主要负责接收job的提交请求，为应用分配第一个Container来运行ApplicationMaster，还有就是负责监控ApplicationMaster，在遇到失败时重启ApplicationMaster运行的Container。

## 2.4 Application Master

ApplicationMaster的主要作用是向ResourceManager申请资源并和NodeManager协同工作来运行应用的各个任务然后跟踪它们状态及监控各个任务的执行，遇到失败的任务还负责重启它。

在MR1中，JobTracker即负责job的监控，又负责系统资源的分配。而在MR2中，资源的调度分配由ResourceManager专门进行管理，而每个job或应用的管理、监控交由相应的分布在集群中的ApplicationMaster，如果某个ApplicationMaster失败，ResourceManager还可以重启它，这大大提高了集群的拓展性。

在MR1中，Hadoop架构只支持MapReduce类型的job，所以它不是一个通用的框架，因为Hadoop的JobTracker和TaskTracker组件都是专门针对MapReduce开发的，它们之间是深度耦合的。Yarn的出现解决了这个问题，关于Job或应用的管理都是由ApplicationMaster进程负责的，Yarn允许我们自己开发ApplicationMaster，我们可以为自己的应用开发自己的ApplicationMaster。这样每一个类型的应用都会对应一个ApplicationMaster，一个ApplicationMaster其实就是一个类库。这里要区分ApplicationMaster*类库和ApplicationMaster实例*，一个ApplicationMaster类库何以对应多个实例，就行java语言中的类和类的实例关系一样。总结来说就是，每种类型的应用都会对应着一个ApplicationMaster，每个类型的应用都可以启动多个ApplicationMaster实例。所以，在yarn中，是每个job都会对应一个ApplicationMaster而不是每类。

# 3. Yarn 框架相对于老的 MapReduce 框架什么优势呢？

- 这个设计大大减小了 ResourceManager 的资源消耗，并且让监测每一个 Job 子任务 (tasks) 状态的程序分布式化了，更安全、更优美。

- 在新的 Yarn 中，ApplicationMaster 是一个可变更的部分，用户可以对不同的编程模型写自己的 AppMst，让更多类型的编程模型能够跑在 Hadoop 集群中，可以参考 hadoop Yarn 官方配置模板中的 ``mapred-site.xml`` 配置。

- 对于资源的表示以内存为单位 ( 在目前版本的 Yarn 中，没有考虑 cpu 的占用 )，比之前以剩余 slot 数目更合理。

- 老的框架中，JobTracker 一个很大的负担就是监控 job 下的 tasks 的运行状况，现在，这个部分就扔给 ApplicationMaster 做了，而 ResourceManager 中有一个模块叫做 ApplicationsManager，它是监测 ApplicationMaster 的运行状况，如果出问题，会将其在其他机器上重启。

- Container 是 Yarn 为了将来作资源隔离而提出的一个框架。这一点应该借鉴了 Mesos 的工作，目前是一个框架，仅仅提供 java 虚拟机内存的隔离 ,hadoop 团队的设计思路应该后续能支持更多的资源调度和控制 , 既然资源表示成内存量，那就没有了之前的 map slot/reduce slot 分开造成集群资源闲置的尴尬情况。

Ref：https://www.jianshu.com/p/f9a3900ab944