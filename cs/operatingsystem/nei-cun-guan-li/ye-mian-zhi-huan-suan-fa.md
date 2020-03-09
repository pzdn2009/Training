# 页面替换算法

* 最佳置换算法(Optimal Page Replacement Algorithm)
* 最近不常使用算法(Not Recently Used Replacement Algorithm)
* 先进先出页面置换算法(First-In,First-Out Page Replacement Algorithm)
* 时钟替换算法(Clock Page Replacement Algorithm)
* 最久未使用算法(LRU Page Replacement Algorithm)


## 最佳置换算法(Optimal Page Replacement Algorithm)

最佳置换算法是将**未来最久不使用**的页替换出去，这听起来很简单，但是无法实现。但是这种算法可以作为衡量其它算法的基准。

## 最近不常使用算法(Not Recently Used Replacement Algorithm)

这种算法给每个页一个标志位，R表示最近被访问过，M表示被修改过。定期对R进行清零。这个算法的思路是首先淘汰那些未被访问过R=0的页，其次是被访问过R=1,未被修改过M=0的页，最后是R=1,M=1的页。

## 先进先出页面置换算法(First-In,First-Out Page Replacement Algorithm)

这种算法的思想是淘汰在内存中最久的页，这种算法的性能接近于随机淘汰。并不好。
改进型FIFO算法(Second Chance Page Replacement Algorithm)
这种算法是在FIFO的基础上，为了避免置换出经常使用的页，增加一个标志位R，如果最近使用过将R置1，当页将会淘汰时，如果R为1，则不淘汰页，将R置0.而那些R=0的页将被淘汰时，直接淘汰。这种算法避免了经常被使用的页被淘汰。

## 时钟替换算法(Clock Page Replacement Algorithm)

虽然改进型FIFO算法避免置换出常用的页，但由于需要经常移动页，效率并不高。因此在改进型FIFO算法的基础上，将队列首位相连形成一个环路，当缺页中断产生时，从当前位置开始找R=0的页，而所经过的R=1的页被置0，并不需要移动页。如图6所示。
最久未使用算法(LRU Page Replacement Algorithm)
LRU算法的思路是淘汰最近最长未使用的页。这种算法性能比较好，但实现起来比较困难。

## 最久未使用算法(LRU Page Replacement Algorithm)

LRU算法的思路是淘汰最近最长未使用的页。这种算法性能比较好，但实现起来比较困难。

Ref：https://blog.csdn.net/qq_15437629/article/details/79304190