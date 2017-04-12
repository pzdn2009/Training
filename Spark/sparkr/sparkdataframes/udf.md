# UDF

# dapply

在每個DF分區上應用函數。參數：DF，輸出：data.frame. Schema 指定了結果的數據類型.

```r
> schema <- structType(structField("eruptions", "double"), structField("waiting", "double"),
+                      structField("waiting_secs", "double"))
> df1 <- dapply(df, function(x) { x <- cbind(x, x$waiting * 60) }, schema)
> head(collect(df1))
  eruptions waiting waiting_secs
1     3.600      79         4740
2     1.800      54         3240
3     3.333      74         4440
4     2.283      62         3720
5     4.533      85         5100
6     2.883      55         3300
> ?collect
> cl <- collect(df1)
> View(cl)
```

# dapplyCollect


應用函數并收集每個分區的結果。

輸出：data.frame

```r
ldf <- dapplyCollect(
         df,
         function(x) {
           x <- cbind(x, "waiting_secs" = x$waiting * 60)
         })
head(ldf, 3)
```