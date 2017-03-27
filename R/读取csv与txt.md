# 读取CSV与TXT

# 1.从CSV格式读取
```r
> write.csv(Insurance,"Ins.csv") # csv数据格式

> head(read.csv("Ins.csv")) # 读取为csv格式
> head(read.table("Ins.csv",header=T,sep=",")) #设置header，sep
```
从CSV到table需要指明header和sep

# 2.从table格式读取
```r
> write.table(Insurance,"Ins.txt") # 空格分隔的文档

> head(read.table("Ins.txt")) #直接读取
> read.csv("Ins.txt",header=T,sep="") # 设置header，sep
```
从table到CSV需要指明header和sep