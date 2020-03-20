# Deploying

* [Spark-submit](/deco/spark/deploying/spark-submit.md)
* [Deploying-yarn](/deco/spark/deploying/deploying-yarn.md)

# 1. 集群管理模式

* Standalone – a simple cluster manager included with Spark that makes it easy to set up a cluster.
* Apache Mesos – a general cluster manager that can also run Hadoop MapReduce and service applications.
* Hadoop YARN – the resource manager in Hadoop 2.
* Kubernetes – an open-source system for automating deployment, scaling, and management of containerized applications.

# 2. 提交

使用` spark-submit`脚本

# 3. 监控

4040端口

# 4. 术语

Term|Meaning
--|--
Application|User program built on Spark. Consists of a driver program and executors on the cluster.
Application jar	|A jar containing the user's Spark application. In some cases users will want to create an "uber jar" containing their application along with its dependencies. The user's jar should never include Hadoop or Spark libraries, however, these will be added at runtime.
Driver program	|The process running the main() function of the application and creating the SparkContext
Cluster manager	|An external service for acquiring resources on the cluster (e.g. standalone manager, Mesos, YARN)
Deploy mode|	Distinguishes where the driver process runs. In "cluster" mode, the framework launches the driver inside of the cluster. In "client" mode, the submitter launches the driver outside of the cluster.
Worker node	|Any node that can run application code in the cluster
Executor|	A process launched for an application on a worker node, that runs tasks and keeps data in memory or disk storage across them. Each application has its own executors.
Task	|A unit of work that will be sent to one executor
Job|	A parallel computation consisting of multiple tasks that gets spawned in response to a Spark action (e.g. save, collect); you'll see this term used in the driver's logs.
Stage	|Each job gets divided into smaller sets of tasks called stages that depend on each other (similar to the map and reduce stages in MapReduce); you'll see this term used in the driver's logs.