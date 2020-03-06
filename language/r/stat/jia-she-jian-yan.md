# 假设检验

## 假设检验

在研究中最常见的行为就是对两个组进行比较。

结果变量为连续性的组间比较，并假设呈正态分布。

MASS包，Uscrime数据集。包含了1960年美国47个州的刑罚制度对犯罪率的影响信息。

Prob：：监禁的概率。

So：是否为南方。

## 独立样本的T检验

基本两个总体的均值是相等的这个假设。

```r
> library(MASS)
> t.test(Prob~So,data=UScrime)

    Welch Two Sample t-test

data:  Prob by So
t = -3.8954, df = 24.925, p-value = 0.0006506
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -0.03852569 -0.01187439
sample estimates:
mean in group 0 mean in group 1 
     0.03851265      0.06371269
```

