* 下载安装
* mysql安装
* 配置mysql
* 启动

# 1. 下载

对于2.x.y的版本，选择2.3.6，Master节点安装，如下：

```
wget https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-2.3.6/apache-hive-2.3.6-bin.tar.gz

tar -vczf apache-hive-2.3.6-bin.tar.gz
mv apache-hive-2.3.6-bin /usr/local/hive
```

# 2. 环境配置

配置环境变量：
```
cat >> ~/.bashrc #ubuntu,  centos,bash_profile

export HIVE_HOME=/home/hadoop/apache-hive-2.3.6-bin
export PATH=$PATH:$JAVA_HOME/bin:$HIVE_HOME/bin

source ~./bashrc
```

hadoop下创建hive所用文件夹：

修改权限：

mysql-connector:



# 3. mysql安装

# 4. mysql配置

# 5. hive conf

修改hive-env.sh

增加hive-site.xml

# 6. 启动

./schematool -dbType mysql -initSchema hive hive
hive --service metastore 
