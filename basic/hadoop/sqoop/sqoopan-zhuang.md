# Sqoop安装

下载地址：https://mirrors.tuna.tsinghua.edu.cn/apache/sqoop/1.4.7/

配置SQOOP_HOME:
```
export $SQOOP_HOME=/usr/local/sqoop
export PATH=$PATH:$SQOOP_HOME/bin
```

## 1. 配置env

cp文件：/conf/sqooop-env.sh

```sh
# Set Hadoop-specific environment variables here.

#Set path to where bin/hadoop is available
export HADOOP_COMMON_HOME=/usr/local/hadoop

#Set path to where hadoop-*-core.jar is available
export HADOOP_MAPRED_HOME=/usr/local/hadoop

#set the path to where bin/hbase is available
export HBASE_HOME=

#Set the path to where bin/hive is available
export HIVE_HOME=/usr/local/hive

#Set the path for where zookeper config dir is
export ZOOCFGDIR=
```

# 2. 配置mysql驱动

```
cp /usr/local/hive/lib/mysql-connector-java-5.1.48-bin.jar \
/usr/local/sqoop/lib/
```

# 3. 启动

```
sqoop help
```
