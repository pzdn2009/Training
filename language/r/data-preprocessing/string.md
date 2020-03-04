# String

100:10

```r
# 轉為折扣率
getDiscountRate <- function(rateString) {
  splitRet <- as.numeric(unlist(strsplit(rateString,split = ":")))
  return (splitRet[2] / splitRet[1])
}
```

