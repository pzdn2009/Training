# Graphics

## Graphics

## 概述

**轴须图rug plot**，向每个数据点添加一个小的随机值，以避免重叠的点产生影响。

```r
rug(jitter())
```

**abline**，添加直线到plot中。

**plot\(x,y,…\)** Generic function for plotting of R objects.

* x：必须。坐标结构，或者有plot方法的R对象。
* y：可选。
* …:
  * type
  * "p" for points,
  * "l" for lines,
  * "b" for both,
  * "c" for the lines part alone of "b",
  * "o" for both ‘overplotted’,
  * h" for ‘histogram’ like \(or ‘high-density’\) vertical lines,
  * "s" for stair steps,
  * "S" for other steps, see ‘Details’ below,
  * "n" for no plotting.
  * main 
  * sub
  * xlab
  * ylab

## 详细

* 直方图
* 盒须图 箱线图
* 核密度图
* Q-Q图

