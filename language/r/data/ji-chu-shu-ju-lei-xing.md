# 基础数据类型

**numberic** 数值型

```r
> x<-c(1,2,3,4)
> x
[1] 1 2 3 4
> class(x)
[1] "numeric"
```

**integer** 整型

```text
> class(as.integer(x))
[1] "integer"
```

**logical** 逻辑型

```r
> x == 1
[1]  TRUE FALSE FALSE FALSE
```

**character** 字符型

```r
> y=c("I","am","Pz")
> class(y)
[1] "character"
> length(y)
[1] 3
> nchar(y)
[1] 1 2 2
> y=="Pz"
[1] FALSE FALSE  TRUE
```

**factor** 因子型

```r
> factor(c(1,1,0,0,1),levels=c(1,0),labels=c("male","female"))
[1] male   male   female female male  
Levels: male female
```

