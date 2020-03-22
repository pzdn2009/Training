# Training-单词统计

实验准备：

![](/assets/SparkSQL.png)

# 1. Data

一共尝试四种：

* 准备txt格式 Y
* MySQL准备一个表
* Hive准备一个表
* JSON格式

# 2. Read

* 使用JSON
* MySQL
* HiveTable
* textFile Y

# 3. Compute

* 文件读取，分割，RDD Y
* RDD到DF
* 分组

```scala
object SparkWordCount {
  /**
    *
    * @param args 0: 数据源 1: 输出路径
    */
  def main(args: Array[String]): Unit = {
    val conf = new SparkConf()

    val sc = new SparkContext(conf)

    val textFile = sc.textFile(args(0))
    val wc = textFile.flatMap(line => line.split(" ")).map((_, 1)).reduceByKey(_ + _)
    val sorted = wc.map(x => (x._2, x._1)).sortByKey().map(x => (x._2, x._1))
    wc.saveAsTextFile(args(1))

    sc.stop()
  }
}
```

# 4. Write

* HiveTable
* Mysql
* HDFS Y

# 5. Submit

* 编译jar包 `mvn package` Y
* 上传jar包 
* 运行submit脚本
 * Local模式
 * 单机模式
 * 集群模式
 * 集群管理员 YARN Y
 
# 6. 问题
 
1. spark-submit 报 NoClassDefFoundError 解决——需保证版本一致`brew install scala@2.11`https://blog.csdn.net/u013054888/article/details/54600229
2. 提交时9000不对，查看配置`fs.defaultFS`，hdfs://sandbox-hdp.hortonworks.com:8020
3. spark-submit 提交任务到yarn集群报错，去掉程序的配置。https://www.jianshu.com/p/1679c2ea08ab
4. So a job which has 32 reducers will have files named part-r-00000 to part-r-00031, one for each reducer task.