* 下载安装
* 环境配置
* mysql安装与配置
* hive conf
* 启动

# 1. 下载

对于2.x.y的版本，选择2.3.6，Master节点安装，如下：

```
# 下载hive2.3.6
wget https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-2.3.6/apache-hive-2.3.6-bin.tar.gz

# 解压
tar -vczf apache-hive-2.3.6-bin.tar.gz

# 移到/usr目录
mv apache-hive-2.3.6-bin /usr/local/hive
```

# 2. 环境配置

**配置环境变量**：

```
# 添加HIVE_HOME & HIVE/bin
cat >> ~/.bashrc

export HIVE_HOME=/home/hadoop/apache-hive-2.3.6-bin
export PATH=$PATH:$JAVA_HOME/bin:$HIVE_HOME/bin

source ~./bashrc
```

**hadoop下创建hive所用文件夹**：

```
# 数据库目录
hadoop fs -mkdir -p /user/hive/warehouse
# 临时文件
hadoop fs -mkdir -p /user/hive/tmp
# 
hadoop fs -mkdir -p /user/hive/log
hadoop fs -chmod -R 777 /user/hive/warehouse
hadoop fs -chmod -R 777 /user/hive/tmp
hadoop fs -chmod -R 777 /user/hive/log
```

**mysql-connector**:

```
# 下载connector
wget http://ftp.ntu.edu.tw/MySQL/Downloads/Connector-J/mysql-connector-java-5.1.48.tar.gz

# 解压
tar -vzxf mysql-connector-java-5.1.48.tar.gz
cd mysql-connector-java-5.1.48.tar.gz
# 复制到hive/lib目录
cp mysql-connector-java-5.1.48-bin.jar /usr/local/hive/lib/
```

# 3. mysql安装 & 配置

安装：

```
apt-get install mysql-server
service mysql start
```

创建数据库：

```
mysql -uroot -p

# 创建数据库
create database metastore;

# 用户名和密码，权限
grant all on metastore.* to hive@'%'  identified by 'hive';
grant all on metastore.* to hive@'localhost'  identified by 'hive';

# 刷新权限
flush privileges;
```

# 4. hive conf

**修改hive-env.sh**：

```
# env文件
cp hive-env.sh.template hive-env.sh
# 编辑3项
vim hive-en.sh

export HADOOP_HOME=/usr/local/hadoop
export HIVE_CONF_DIR=/usr/local/hive/conf
export HIVE_AUX_JARS_PATH=/usr/local/hive/lib
```

**增加hive-site.xml**:
```
mkdir -p /usr/local/hive/tmp
cd /usr/local/hive/conf
touch hive-site.xml
vim hive-site.xml
```

内容：
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<configuration>
   <property>
       <name>hive.exec.scratchdir</name>
       <value>/usr/local/hive/tmp</value>
   </property>
   <property>
       <name>hive.metastore.warehouse.dir</name>
       <value>/user/hive/warehouse</value>
   </property>
   
   <!-- 配置 MySQL 数据库连接信息 -->
   <property>
       <name>javax.jdo.option.ConnectionURL</name>
       <value>jdbc:mysql://localhost:3306/metastore?createDatabaseIfNotExist=true&amp;characterEncoding=UTF-8&amp;useSSL=false</value>
    </property>
    <property>
       <name>javax.jdo.option.ConnectionDriverName</name>
       <value>com.mysql.jdbc.Driver</value>
    </property>
    <property>
       <name>javax.jdo.option.ConnectionUserName</name>
       <value>hive</value>
    </property>
    <property>
       <name>javax.jdo.option.ConnectionPassword</name>
       <value>hive</value>
    </property>
</configuration>
```

# 5. 启动

```
# 初始化
./schematool -dbType mysql -initSchema hive hive

# 启动
hive --service metastore 
```
