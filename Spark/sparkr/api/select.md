# select(x, col, ...)

使用列名或者列表達式，來選擇列的集合。返回新的SparkDataFrame。

Usage：

```r
select(SparkDataFrame, character)
select(SparkDataFrame, Column)
select(SparkDataFrame, list)
```

Eg：
```r
select(df, "*")
select(df, "col1", "col2")
select(df, df$name, df$age + 1)
select(df, c("col1", "col2"))
select(df, list(df$name, df$age + 1))

# Similar to R data frames columns can also be selected using $
df[,df$age]
```