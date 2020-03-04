# DF操作

## DF 操作

## 1. 顯示行，列

```r
> df <- as.DataFrame(faithful)
17/03/27 20:15:09 WARN ObjectStore: Failed to get database global_temp, returning NoSuchObjectException

> head(select(df,df$eruptions))
  eruptions    
1     3.600
2     1.800
3     3.333
4     2.283
5     4.533
6     2.883
> head(filter(df,df$waiting<50))
  eruptions waiting
1     1.750      47
2     1.750      47
3     1.867      48
4     1.750      48
5     2.167      48
6     2.100      49
```

## 2.分組，聚合

```r
> head(summarize(groupBy(df, df$waiting), count = n(df$waiting)))
[Stage 8:===========================>                              
(8 +[Stage 8:========================================>                
(12 +[Stage 8:=====================================================>   
(16 +                                                                       
+[Stage 10:====================================================>   (41 +
      waiting count
1      70     4
2      67     1
3      69     2
4      88     6
5      49     5
6      64     4

> head(arrange(waiting_counts, desc(waiting_counts$count)))
       waiting count
1      78    15
2      83    14
3      81    13
4      77    12
5      82    12
6      79    10
```

## 3. 操作列

```r
> df$waiting_secs <- df$waiting * 60
> head(df)
  eruptions waiting waiting_secs
1     3.600      79         4740
2     1.800      54         3240
3     3.333      74         4440
4     2.283      62         3720
5     4.533      85         5100
6     2.883      55         3300
```

