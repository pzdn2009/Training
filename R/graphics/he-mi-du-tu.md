# 核密度图

```r
> par(mfrow=c(1,1))
> plot(d)
> polygon(d,col="green",border = "blue")
> rug(jitter(mtcars$mpg))
```

![](/assets/RplotDensity.png)