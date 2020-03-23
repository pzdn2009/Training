
https://github.com/apache/spark/tree/master/examples

## 层次

* Spark SQL，Spark Streaming，MLLib，GraphX
* Spark Core
* 獨立調度器，YARN，Mesos


## Ambari

```
export SPARK_HOME=/usr/hdp/current/spark2-client
export SPARK_CONF_DIR=/etc/spark2/conf
```

## 啟動



## 連接到集群

```text
./bin/spark-shell --master spark://IP:PORT
```

## 資源調度

```scala
val conf = new SparkConf()
  .setMaster(...)
  .setAppName(...)
  .set("spark.cores.max", "10")
val sc = new SparkContext(conf)
```

## 從RStudio啟動

必須有SPARK\_HOME 環境變量

通過以下判斷及啟動

```r
if (nchar(Sys.getenv("SPARK_HOME")) < 1) {
  Sys.setenv(SPARK_HOME = "/home/spark")
}
library(SparkR, lib.loc = c(file.path(Sys.getenv("SPARK_HOME"), "R", "lib")))
sparkR.session(master = "local[*]", sparkConfig = list(spark.driver.memory = "2g"))
```

