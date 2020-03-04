# arrange

對DF進行排序。

arrange\(x, col, ..., decreasing = FALSE\)

orderBy\(x, col, ...\)

* x 待排序的DF
* col 列名或列對象
* ... 額外的排序字段
* decreasing 是否倒序

```r
> df2<-arrange(df,"waiting",decreasing=T)
> head(df2)
```

