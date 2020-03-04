# 1.加载数据到R

特征： season, size , speed ,mxPH ,mnO2 , Cl, NO3, NH4 , oPO4 , PO4 ,Chla , a1 , a2 , a3 , a4 , a5 ,a6, a7

```r
install.package(DMwR)
library(DMwR)
head(algae)
```

```r
  season  size  speed mxPH mnO2     Cl    NO3     NH4    oPO4     PO4 Chla   a1   a2   a3  a4   a5   a6  a7
1 winter small medium 8.00  9.8 60.800  6.238 578.000 105.000 170.000 50.0  0.0  0.0  0.0 0.0 34.2  8.3 0.0
2 spring small medium 8.35  8.0 57.750  1.288 370.000 428.750 558.750  1.3  1.4  7.6  4.8 1.9  6.7  0.0 2.1
3 autumn small medium 8.10 11.4 40.020  5.330 346.667 125.667 187.057 15.6  3.3 53.6  1.9 0.0  0.0  0.0 9.7
4 spring small medium 8.07  4.8 77.364  2.302  98.182  61.182 138.700  1.4  3.1 41.0 18.9 0.0  1.4  0.0 1.4
5 autumn small medium 8.06  9.0 55.350 10.416 233.700  58.222  97.580 10.5  9.2  2.9  7.5 0.0  7.5  4.1 1.0
6 winter small   high 8.25 13.1 65.750  9.248 430.000  18.250  56.667 28.4 15.1 14.6  1.4 0.0 22.5 12.6 2.9
```

