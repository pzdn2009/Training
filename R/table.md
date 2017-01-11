# table

table统计数据的频数。

The table command is one of **the most basic summary statistics** functions in R, it runs through the vector you gave it and simply **counts **the occurrence of each value in it。

就是按照不同的因子來統計頻數的。很类似group by 。

eg:

```
# 类别为table
> class(table(seq(1,10)))
[1] "table"

## 0,1
> table(train$Survived)

## Check the design:
with(warpbreaks, table(wool, tension))
table(state.division, state.region)

# simple two-way contingency table
# 处理连续值 
with(airquality, table(cut(Temp, quantile(Temp)), Month))
```