# 4.获取预测模型

## 4.获取预测模型

## 多元线性回归

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
Multiple R-squared:  0.3731,    Adjusted R-squared:  0.3215 
F-statistic: 7.223 on 15 and 182 DF,  p-value: 2.444e-12
```

**辅助变量**

R处理名义变量，K个水平，生成K-1个辅助变量，这些辅助变量为0或1,。当辅助变量为1时，表示因子值出现。如果K-1个辅助变量取值都为0，则表明因子变量的取值为第K个剩余的值。

**假设检验** 对于回归方程的系数，R显示它的估计值（Estimate）和标准误差（Std. Error），标准误差指这些系数的变化程度。用t检验来检验这些系数为0的假设。t = Estimate / Std. Error.

Pr\(&gt;\|t\|\) 表示系数为0这一假设被拒绝的概率。

$$R^2$$

R^2表示模型与数据的吻合度，越接近1，表明模型拟合得越好。

```r
> anova(lm.a1)
Analysis of Variance Table

Response: a1
           Df Sum Sq Mean Sq F value    Pr(>F)    
season      3     85    28.2  0.0905 0.9651944    
size        2  11401  5700.7 18.3088  5.69e-08 ***
speed       2   3934  1967.2  6.3179 0.0022244 ** 
mxPH        1   1329  1328.8  4.2677 0.0402613 *  
mnO2        1   2287  2286.8  7.3444 0.0073705 ** 
Cl          1   4304  4304.3 13.8239 0.0002671 ***
NO3         1   3418  3418.5 10.9789 0.0011118 ** 
NH4         1    404   403.6  1.2963 0.2563847    
oPO4        1   4788  4788.0 15.3774 0.0001246 ***
PO4         1   1406  1405.6  4.5142 0.0349635 *  
Chla        1    377   377.0  1.2107 0.2726544    
Residuals 182  56668   311.4                      
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

可以看出，season对模型拟合误差的贡献最小。

```r
> lm2.a1 <- update(lm.a1,.~.-season)
> summary(lm2.a1)
```

更新之后，可以看到R^2有所提高。

比较两组模型的差别：

```r
> anova(lm.a1,lm2.a1)
Analysis of Variance Table

Model 1: a1 ~ season + size + speed + mxPH + mnO2 + Cl + NO3 + NH4 + oPO4 + 
    PO4 + Chla
Model 2: a1 ~ size + speed + mxPH + mnO2 + Cl + NO3 + NH4 + oPO4 + PO4 + 
    Chla
  Res.Df   RSS Df Sum of Sq      F Pr(>F)
1    182 56668                           
2    185 57116 -3   -447.62 0.4792 0.6971
```

F检验对两个模型进行方差分析。

```r
> final.lm <- step(lm.a1) #Akaike信息标准搜索模型进行向后消元。
```

结果R^2仍然不理想，，表明对假定为线性模型是不合适的

## 回归树

```r
library(rpart)
data("algae")
algae <- algae[-manyNAs(algae),]
rt.a1 <- rpart(a1~.,data=algae[,1:12])
rt.a1
prettyTree(rt.a1)
```

