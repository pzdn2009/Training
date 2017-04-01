# 回归分析

# 理论

OLS

残差平方和最小

类型：
- 简单线性
- 多项式

# 简单线性回归

用一个量化的解释变量预测一个量化的响应变量。

```r
> fit <- lm(weight ~ height,data=women)
> summary(fit)

Call:
lm(formula = weight ~ height, data = women)

Residuals:
    Min      1Q  Median      3Q     Max 
-1.7333 -1.1333 -0.3833  0.7417  3.1167 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) -87.51667    5.93694  -14.74 1.71e-09 ***
height        3.45000    0.09114   37.85 1.09e-14 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1.525 on 13 degrees of freedom
Multiple R-squared:  0.991,	Adjusted R-squared:  0.9903 
F-statistic:  1433 on 1 and 13 DF,  p-value: 1.091e-14


```

Residuals：残差
Coefficients：相关性
Intercept：截距

```r
> women$weight
 [1] 115 117 120 123 126 129 132 135 139 142 146 150 154 159 164
> fitted(fit)
       1        2        3        4        5        6        7        8        9 
112.5833 116.0333 119.4833 122.9333 126.3833 129.8333 133.2833 136.7333 140.1833 
      10       11       12       13       14       15 
143.6333 147.0833 150.5333 153.9833 157.4333 160.8833 
> residuals(fit)
          1           2           3           4           5           6           7 
 2.41666667  0.96666667  0.51666667  0.06666667 -0.38333333 -0.83333333 -1.28333333 
          8           9          10          11          12          13          14 
-1.73333333 -1.18333333 -1.63333333 -1.08333333 -0.53333333  0.01666667  1.56666667 
         15 
 3.11666667 
> plot(women$height,women$weight)
> abline(fit)

```

![](/assets/RplotLMWomen.png)