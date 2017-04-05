# 聚类模型及评价

问题：

- 识别具有相同购买模式的顾客群体
- 识别在相同地区或者相同顾客群里受欢迎的商品
- 识别所有讨论的类似事情的新闻项

方法：
- kmeans

评判：
- WSS
- BSS
- WSS/BSS



示例：

```
wget https://archive.ics.uci.edu/ml/machine-learning-databases/00236/seeds_dataset.txt 
```
```r
seeds<- read.table(path="seeds_dataset.txt")
set.seed(1)
str(seeds)

# Group the seeds in three clusters
km_seeds <- kmeans(seeds, 3)

# Color the points in the plot based on the clusters
plot(length ~ compactness, data = seeds,col=km_seeds$cluster)

# Print out the ratio of the WSS to the BSS
km_seeds$tot.withinss/km_seeds$betweenss
```