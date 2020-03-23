# 集群模式

* master & worker
* 人工启动集群
* 脚本启动集群

# 1. 人工

## 1.1 启动

master节点，啟動master:
```text
./sbin/start-master.sh
```

会绑定7077端口。

slave节点，啟動slave：

```text
./sbin/start-slave.sh <master-spark-URL>
```

## 1.2 啟動參數

| Argument參數 | Meaning意義 |
| :--- | :--- |
| -h HOST, --host HOST | 監聽的主機名 |
| -p PORT, --port PORT | 監聽的端口，MASTER默認7077,worker隨機。 |
| --webui-port PORT | webUI端口，MASTER8080，worker8081 |
| -c CORES, --cores CORES | Spark應用可用的CPU處理器數，默認全部，此參數只對worker有效。 |
| -m MEM, --memory MEM | 應用可用的內存數，默認物理內存-1GB，此參數只對worker有效。 |
| -d DIR, --work-dir DIR | 作業輸出日誌和空間擴容目錄，默認SPARK_HOME/work，只對worker有效。 |
| --properties-file FILE | 自定義Spark屬性文件路徑，默認為conf/spark-defaults.conf |

## 1.3 停止

```
stop-slave.sh
stop-master.sh
```

# 2. 脚本

* master 通过SSH能够方位slave
* master 的spark/conf创建一个`slaves`文件，一行一个，包含slaves的主机名或IP地址
* 参数：默认使用spark-env.sh配置

```
start-all.sh
stop-all.sh
```