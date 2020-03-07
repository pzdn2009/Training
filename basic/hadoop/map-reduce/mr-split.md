# MR-split

![](/assets/hadoop-mp.x-png)

split的目的：将数据切小，然后对应一个Map任务去计算。

# 1. 术语

* **输入分片（Input Split）**：在进行map计算之前，mapreduce会根据输入文件计算输入分片（input split），每个输入分片（input split）针对一个map任务，输入分片（input split）存储的并非数据本身，而是一个分片长度和一个记录数据的位置的数组。-->**（offset,length）**

# 2. 分片的计算

mapred-site.xml中：
```java
#最小长度
mapred.min.split.size
#最大长度
mapred.max.split.size

Math.max(minSize, Math.min(maxSize, blockSize));
```
* maxsize（切片最大值）：
  
  参数如果调得比blocksize小，则会让切片变小，而且就等于配置的这个参数的值

* minsize （切片最小值）：

  参数调的比blockSize大，则可以让切片变得比blocksize还大

# 3. 计算过程 

`InputFormat`，MapReduce框架可以做到：
1. 验证作业的输入的正确性；
2. 把输入文件切分成多个逻辑InputSplits，并把每一个InputSplit分别分发给一个单独的MapperTask；
3. 提供RecordReader的实现，这个RecordReader从指定的InputSplit中正确读出一条一条的Ｋ－Ｖ对，这些Ｋ－Ｖ对将由我们写的Mapper方法处理。

![](/assets/MPSplit.png)

Ref:https://blog.csdn.net/yumi6666/article/details/82526276?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task
