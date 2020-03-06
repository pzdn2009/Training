# 直方图

Histogram。

hist\(x\)

* x：是一个数值变量。
* probability ：使用概率表示。

```r
> par(mfrow = c(1,2))
> hist(Insurance$Claims,main="hist Freq")
> hist(Insurance$Claims,probability = T,main="hist Prob")
```

![](../../../.gitbook/assets/rplothist%20%281%29.png)

意义：

在质量管理中，如何预测并监控产品质量状况?如何对质量波动进行分析?直方图就是一目了然地把这些问题图表化处理的工具。

它通过对收集到的貌似无序的数据进行处理，来反映产品质量的分布情况，判断和预测产品质量及不合格率。

