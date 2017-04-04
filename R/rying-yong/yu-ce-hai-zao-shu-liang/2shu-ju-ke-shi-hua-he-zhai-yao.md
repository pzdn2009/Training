# 2. 数据可视化和摘要

# 摘要

```r
> summary(algae)
```

结果：
```r
    season       size       speed         mxPH            mnO2              Cl               NO3        
 autumn:40   large :45   high  :84   Min.   :5.600   Min.   : 1.500   Min.   :  0.222   Min.   : 0.050  
 spring:53   medium:84   low   :33   1st Qu.:7.700   1st Qu.: 7.725   1st Qu.: 10.981   1st Qu.: 1.296  
 summer:45   small :71   medium:83   Median :8.060   Median : 9.800   Median : 32.730   Median : 2.675  
 winter:62                           Mean   :8.012   Mean   : 9.118   Mean   : 43.636   Mean   : 3.282  
                                     3rd Qu.:8.400   3rd Qu.:10.800   3rd Qu.: 57.824   3rd Qu.: 4.446  
                                     Max.   :9.700   Max.   :13.400   Max.   :391.500   Max.   :45.650  
                                     NA's   :1       NA's   :2        NA's   :10        NA's   :2       
      NH4                oPO4             PO4              Chla               a1              a2               a3        
 Min.   :    5.00   Min.   :  1.00   Min.   :  1.00   Min.   :  0.200   Min.   : 0.00   Min.   : 0.000   Min.   : 0.000  
 1st Qu.:   38.33   1st Qu.: 15.70   1st Qu.: 41.38   1st Qu.:  2.000   1st Qu.: 1.50   1st Qu.: 0.000   1st Qu.: 0.000  
 Median :  103.17   Median : 40.15   Median :103.29   Median :  5.475   Median : 6.95   Median : 3.000   Median : 1.550  
 Mean   :  501.30   Mean   : 73.59   Mean   :137.88   Mean   : 13.971   Mean   :16.92   Mean   : 7.458   Mean   : 4.309  
 3rd Qu.:  226.95   3rd Qu.: 99.33   3rd Qu.:213.75   3rd Qu.: 18.308   3rd Qu.:24.80   3rd Qu.:11.375   3rd Qu.: 4.925  
 Max.   :24064.00   Max.   :564.60   Max.   :771.60   Max.   :110.456   Max.   :89.80   Max.   :72.600   Max.   :42.800  
 NA's   :2          NA's   :2        NA's   :2        NA's   :12                                                         
       a4               a5               a6               a7        
 Min.   : 0.000   Min.   : 0.000   Min.   : 0.000   Min.   : 0.000  
 1st Qu.: 0.000   1st Qu.: 0.000   1st Qu.: 0.000   1st Qu.: 0.000  
 Median : 0.000   Median : 1.900   Median : 0.000   Median : 1.000  
 Mean   : 1.992   Mean   : 5.064   Mean   : 5.964   Mean   : 2.495  
 3rd Qu.: 2.400   3rd Qu.: 7.500   3rd Qu.: 6.925   3rd Qu.: 2.400  
 Max.   :44.600   Max.   :44.400   Max.   :77.600   Max.   :31.600  
```

# 可视化
## mxpH

```r
> par(mfrow=c(1,2))
> hist(algae$mxPH,probability = T,xlab='',
       main='Hist of max pH value',ylim = 0:1)
> lines(density(algae$mxPH,na.rm=T))

rug(jitter(algae$mxPH))
qq.plot(algae$mxPH,main='QQ plot of max PH')
```

![](/assets/RplotMaxPHHISTQQ.png)

>解释：
Q-Q图，回执变量值和正态分布的理论分位数（实线）的散点图，同时，给出95%置信区间的带状图（虚线）。从图中可以看出， 变量有几个小的值明显在95%置信区间之外，它们不服从正态分布。

## oPO4
```r
> boxplot(algae$oPO4,ylab="pPO4") #绘制箱线图
> rug(jitter(algae$oPO4),side = 2) #left rug
> abline(h=mean(algae$oPO4,na.rm=T),lty=2) #add Straight line
```
![](/assets/RplotoPo4Box.png)

>解释：
oPO4的分布集中在较小的观测值周围，因为分布为正片。大部分水样的oPO4值比较低，但有几个特别高。

## NH4

```r
> plot(algae$NH4,xlab= "" )
> abline(h=mean(algae$NH4,na.rm=T),lty=1,col="red") #均值
> abline(h=mean(algae$NH4,na.rm=T)+
         sd(algae$NH4,na.rm=T),lty=2,col='green') #一个标准差
> abline(h=median(algae$NH4,na.rm=T),lty=1,col='blue',lwd=2) #中位数
> identify(algae$NH4)
警告: 已经找到了最近的点
[1]  20  88 153
```

![](/assets/RplotNH4.png)

## size~a1

```r
> library(lattice)
> bwplot(size~a1,data=algae,ylab='River Size',xlab='Algal A1')
```
![](/assets/RplotbwPlotsizeA1.png)
