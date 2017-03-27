# SparkR

SparkR is an R package that provides a light-weight frontend to use Apache Spark from R.

A **SparkDataFrame **is a distributed collection of data organized into named columns. 
>類似R中的dataframe

啟動：

```
bin/sparkR --master spark://zhenpengVm:7077
```

啟動會話：
```r
sparkR.session()
```

主要部件：

- SparkDataFrames
- MLLib
