# Hive-Training-单词统计

* 环境
* 数据
* 计算
* 展示

# 1. 环境

* hadoop
* hive

## 1.1 hadoop环境

* 确保已经安装了HDFS；
* master，slave1,slave2;
* 打开50070，8088查看；
* [详见Hadoop安装](/basic/hadoop/hadoop-an-zhuang/README.md)

## 1.2 hive环境

* Hive版本：2.3.6,master节点安装
* [详见Hive安装](/basic/hadoop/hive/hivean-zhuang.md)

# 2. 数据

# 3. 计算

```
# 创建内部表
create table word_count(line string);
# 加载数据
load data inpath 'input/file1.txt' into table work_count;
# 存储计算结果
create table word_count_result as select word, count(1) as count from (select explode (split (line, ' ')) as word from word_count) w group by word order by word;
# 结果
hive -e "select * from hdb.word_count_result" > /tmp/out.txt
```

# 4. 展示


