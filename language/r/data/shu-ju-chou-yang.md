# 数据抽样

## 数据抽样

## 1. sample函数

sample\(x, size, replace = FALSE, prob = NULL\)

* x：待抽样对象
* size：待抽样数量
* replace：T：可放回抽样，F：不可放回抽样
* prob：各样本的抽样样本。

## 2. 类别

* 简单随机抽样
  * 有放回抽样

    ```r
    > library(MASS)
    > data(Insurance)
    > sub1<- sample(nrow(Insurance),10,replace=T)
    > sub1
    [1] 46  1 52  8 36 13 19 46 11 61
    > Insurance[sub1,]
     District  Group   Age Holders Claims
    46          3    >2l 25-29      29      2
    1           1    <1l   <25     197     38
    52          4    <1l   >35     316     36
    8           1 1-1.5l   >35    3582    400
    36          3    <1l   >35     648     67
    13          1    >2l   <25      24      4
    19          2    <1l 30-35     151     22
    46.1        3    >2l 25-29      29      2
    11          1 1.5-2l 30-35     355     74
    61          4    >2l   <25       3      0
    ```

    46.1表示重复的46。 设置prob参数如下：

    ```r
    > sub2<-sample(nrow(Insurance),10,replace=T,prob=c(rep(0,nrow(Insurance)-1),1) )
    > sub2
    [1] 64 64 64 64 64 64 64 64 64 64
    > Insurance[sub2,]
     District Group Age Holders Claims
    64          4   >2l >35     114     33
    64.1        4   >2l >35     114     33
    64.2        4   >2l >35     114     33
    64.3        4   >2l >35     114     33
    64.4        4   >2l >35     114     33
    64.5        4   >2l >35     114     33
    64.6        4   >2l >35     114     33
    64.7        4   >2l >35     114     33
    64.8        4   >2l >35     114     33
    64.9        4   >2l >35     114     33
    ```

  * 无放回抽样

    replace设置为F，size的长度不可超过参数x的超过
* 分层抽样

```r
> install.packages("sampling")
> library(sampling)
> sub<-strata(Insurance,stratanames="District",size=c(1,2,3,4),method="srswor")
   District ID_unit   Prob Stratum
6         1       6 0.0625       1
26        2      26 0.1250       2
27        2      27 0.1250       2
34        3      34 0.1875       3
37        3      37 0.1875       3
40        3      40 0.1875       3
49        4      49 0.2500       4
50        4      50 0.2500       4
52        4      52 0.2500       4
58        4      58 0.2500       4
> getdata(Insurance,sub)

> sub<-strata(Insurance,stratanames="District",size=c(1,2,3,4),description=T) # 带描述

> sub<-strata(Insurance,stratanames="District",size=c(1,2,3,4),method="systematic",pik=Insurance$Claims) # 系统抽样，Claims控制抽样概率
```

* 整群抽样

## 3. 抽样的应用

**构造训练集与测试集**

```r
> trainning_ins <- sample(nrow(Insurance),nrow(Insurance)*3/4,replace=F)
> trainning_data <- Insurance[trainning_ins,]
> testing_data <- Insurance[-trainning_ins,]
> dim(trainning_data);dim(testing_data)
[1] 48  5
[1] 16  5
```

