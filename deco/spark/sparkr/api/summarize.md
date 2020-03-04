# summarize

根據指定的列列表計算聚合。

agg\(x, ...\)

summarize\(x, ...\)

參數

* x: DF或者分組數據 GroupedData.
* ...: further arguments to be passed to or from other methods.

Eg：

```text
df2 <- agg(df, <column> = <aggFunction>)  #字符串就表示函數名稱，前面的列名作為參數。
df2 <- agg(df, newColName= aggFunction(column))  #非字符串：函數與列名一起。

df2 <- agg(df, age = "sum")  # new column name will be created as 'SUM(age#0)'
df3 <- agg(df, ageSum = sum(df$age)) # Creates a new column named ageSum
df4 <- summarize(df, ageSum = max(df$age))
```

