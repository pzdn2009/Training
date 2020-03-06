# 簡介

## 簡介

Spark SQL，Spark Streaming，MLLib，GraphX

Spark Core

獨立調度器，YARN，Mesos

## 啟動

### 啟動master

```text
./sbin/start-master.sh
```

打開[http://localhost:8080/可以查看到：](http://localhost:8080/可以查看到：)

```text
Spark Master at spark://zhenpengVm56:7077
```

### 啟動slave

在Slave節點上：

```text
./sbin/start-slave.sh <master-spark-URL>
```

在MASTER的8080可以看到有new node

### 啟動參數

| Argument參數 | Meaning意義 |
| :--- | :--- |
| -h HOST, --host HOST | 監聽的主機名 |
| -p PORT, --port PORT | 監聽的端口，MASTER默認7077,worker隨機。 |
| --webui-port PORT | webUI端口，MASTER8080，worker8081 |
| -c CORES, --cores CORES | Spark應用可用的CPU處理器數，默認全部，此參數只對worker有效。 |
| -m MEM, --memory MEM | 應用可用的內存數，默認物理內存-1GB，此參數只對worker有效。 |
| -d DIR, --work-dir DIR | 作業輸出日誌和空間擴容目錄，默認SPARK\_HOME/work，只對worker有效。 |
| --properties-file FILE | 自定義Spark屬性文件路徑，默認為conf/spark-defaults.conf |

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
