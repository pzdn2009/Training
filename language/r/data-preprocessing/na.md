# NA值处理

NA：Not Available。

```r
is.na(x) #是否为NA

sum(is.na(x)) #统计NA的个数

x[x$age ==99] <- NA #重编码某些值为NA：

sum(x,na.rm=T) #统计时排除NA

na.omit(df) #删除NA所在的行（对于某些NA值行很少的数据集很有用）
```

