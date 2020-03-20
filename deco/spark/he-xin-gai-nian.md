Spark架构采用了分布式计算中的Master-Slave模型。
Master是对应集群中的含有Master进程的节点，Slave是集群中含有Worker进程的节点。

![](/assets/spark-ac.png)

组成：

* **Cluster Manager**：在standalone模式中即为Master主节点，控制整个集群，监控worker。在YARN模式中为资源管理器。
* **Worker**：从节点，负责控制计算节点，启动Executor或者Driver。在YARN模式中为NodeManager，负责计算节点的控制。提供CPU、内存、存储等资源。
* **Driver**：运行Application的main()函数并创建SparkContext。将在Work上执行Driver代码。
* **Executor**：执行器，在worker node上执行任务的组件、用于启动线程池运行任务。每个Application拥有独立的**一组**Executor。JVM进程。
* **Task**：发给Executor的最小执行单元。运行在Executor的一个线程上。
* **SparkContext**：整个应用的上下文，控制应用的生命周期。
* **RDD**：Spark的基础计算单元，一组RDD可形成执行的有向无环图RDD Graph。
* **DAG Scheduler**：根据作业（task）构建基于Stage的DAG，并提交Stage给TaskScheduler。
* **TaskScheduler**：将任务（task）分发给Executor执行。
* **SparkEnv**：线程级别的上下文， 存储运行时的重要组件的引用。

