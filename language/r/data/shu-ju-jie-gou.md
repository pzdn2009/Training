# 数据结构

**向量**

具有相同类型的序列。

c函数来创建：

```r
x<-c(1,3,3,45)
```

**矩阵**

是一个二维数组。matrix函数来创建。

```r
matrix(data = NA, nrow = 1, ncol = 1, byrow = FALSE,
       dimnames = NULL)
```

**数组**

维数可以任意。matrix函数来创建。

**数据框**

二维表，每一列的数据类型必须相同。

```r
data.frame(1, 1:10, sample(L3, 10, replace = TRUE))
```

**列表** 每个元素的数据类型任意。

