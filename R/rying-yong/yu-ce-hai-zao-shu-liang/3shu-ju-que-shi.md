# 3. 数据缺失

- 剔除
- 变量间的关系填补

# 剔除

```r
> nrow(algae[!complete.cases(algae),])
> algae2 <- na.omit(algae)
```

# 用最高频率值来填补NA

均值：正态分布
中位数（数值）：偏态分布
众数（名义）：偏态分布

```r
> algae[48,"mxPH"] <- mean(algae$mxPH,na.rm=T)
> algae[is.na(algae$Chla),"Chla"] <-median(algae$Chla,na.rm=T)
```

# 通过变量关系填补

通过相关性找出相关度比较高的变量，然后线性拟合。
分析相关性：
```r
> symnum(cor(algae[,4:18],use="complete.obs"))
     mP mO Cl NO NH o P Ch a1 a2 a3 a4 a5 a6 a7
mxPH 1                                         
mnO2    1                                      
Cl         1                                   
NO3           1                                
NH4           ,  1                             
oPO4    .  .        1                          
PO4     .  .        * 1                        
Chla .                  1                      
a1         .        . .    1                   
a2   .                  .     1                
a3                               1             
a4      .           . .             1          
a5                                     1       
a6            .  .                     .  1    
a7                                           1 
attr(,"legend")
[1] 0 ‘ ’ 0.3 ‘.’ 0.6 ‘,’ 0.8 ‘+’ 0.9 ‘*’ 0.95 ‘B’ 1
``
可以看出PO4与oPO4强相关。

采用线性拟合：
```r
> data("algae")
> algae <- algae[-manyNAs(algae),]
> lm(PO4~oPO4,data=algae)

Call:
lm(formula = PO4 ~ oPO4, data = algae)

Coefficients:
(Intercept)         oPO4  
     42.897        1.293  

> fillPO4 <- function(oP) {}
> fillPO4 <- function(oP) {
+   if(is.na(oP)) {
+      return (NA)
+   }
+   else return (42.897 + 1.293*oP)
+ }
> algae[is.na(algae$PO4),"PO4"] <- sapply(algae[is.na(algae$PO4),"oPO4"],fillPO4)

```
## 通过相似性来填补

```r
> algae <- knnImputation(algae,k=10)
```