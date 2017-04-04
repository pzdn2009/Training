# 4.获取预测模型

```r
> data("algae")
> algae <- algae[-manyNAs(algae),]
> clean.algae <- knnImputation(algae,k=10)
> lm.a1 <- lm(a1~.,data=clean.algae[,1:12])

> summary(lm.a1)

Call:
lm(formula = a1 ~ ., data = clean.algae[, 1:12])

Residuals:
    Min      1Q  Median      3Q     Max 
-37.679 -11.893  -2.567   7.410  62.190 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)   
(Intercept)  42.942055  24.010879   1.788  0.07537 . 
seasonspring  3.726978   4.137741   0.901  0.36892   
seasonsummer  0.747597   4.020711   0.186  0.85270   
seasonwinter  3.692955   3.865391   0.955  0.34065   
sizemedium    3.263728   3.802051   0.858  0.39179   
sizesmall     9.682140   4.179971   2.316  0.02166 * 
speedlow      3.922084   4.706315   0.833  0.40573   
speedmedium   0.246764   3.241874   0.076  0.93941   
mxPH         -3.589118   2.703528  -1.328  0.18598   
mnO2          1.052636   0.705018   1.493  0.13715   
Cl           -0.040172   0.033661  -1.193  0.23426   
NO3          -1.511235   0.551339  -2.741  0.00674 **
NH4           0.001634   0.001003   1.628  0.10516   
oPO4         -0.005435   0.039884  -0.136  0.89177   
PO4          -0.052241   0.030755  -1.699  0.09109 . 
Chla         -0.088022   0.079998  -1.100  0.27265   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 17.65 on 182 degrees of freedom
Multiple R-squared:  0.3731,	Adjusted R-squared:  0.3215 
F-statistic: 7.223 on 15 and 182 DF,  p-value: 2.444e-12
```

辅助变量

假设检验

$$R^2$$

