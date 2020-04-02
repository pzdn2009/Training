# Flink-简介

* 大数据架构
* Flink优势
* Flink架构

# 1. 大数据架构

* labmda架构
* 有状态流式计算架构

## 1.1 Labmda架构

Lambda架构的目标是设计出一个能满足实时大数据系统关键特性的架构，包括有：
* 高容错
* 低延时
* 可扩展

Lambda架构整合**离线计算**和**实时计算**，融合**不可变性**（Immunability），读写分离和复杂性隔离等一系列架构原则，可集成Hadoop，Kafka，Storm，Spark，Hbase等各类大数据组件。

>查询公式：
```
Query = Function(All Data)
```

>群理论：
满足结合律=>半群Semigroups
满足单位元=>幺半群Monoids
满足逆运算=>群Groups
满足交换律=>交换群Commutative

>批流结合的可能性：结合律，中间结果/状态存储下来。
MonoidsMerge(B,S)=View.

架构分层：Serving Layer、Speed Layer、Batch Layer。
![](https://img-blog.csdn.net/20150523220916124)

## 1.2 Kappa 架构

统一为流式架构，如利用Kafka的日志文件顺序性。

## 1.3 有状态流式计算架构

![](/assets/flink-state.png)

* 状态：中间结果、可以理解为增量计算。

# 2. Flink优势

* 同时支持高吞吐、低延迟、高性能
* 支持Event Time概念
 * 三种时间：Event Time、Ingestion Time、Process Time
* 支持有状态计算
* 支持高度灵活的时间窗口操作
* 快照Snapslot容错
* JVM实现独立内存管理，GC
* Save Points实现快照

# 3. Flink架构

* 组件栈
* 架构图

## 3.1 组件栈

* API & Libs：批流API & 库
* Runtime Core：运行时
* Deploy：多种部署方式

![](/assets/flink-stack.png)

## 3.2 架构图

架构图：

![](/assets/flink-arct2.png)

任务提交流程（yarn模式）:


![](/assets/flink-arct.jpeg)



## 4. 参考

Ref：

[用于实时大数据处理的Lambda架构](https://blog.csdn.net/brucesea/article/details/45937875)

[函数式编程 - 有趣的Monoid（单位半群）](https://juejin.im/post/5bf44299f265da615d724485)

[Monoids for Java Developers](https://medium.com/@johnmcclean/monoids-for-java-developers-98e2ba94f708)