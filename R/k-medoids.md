# K-Medoids

k-means是每次选簇的均值作为新的中心，迭代直到簇中对象分布不再变化。
其缺点是对于离群点是敏感的，因为一个具有很大极端值的对象会扭曲数据分布。

PAM（partitioning around medoid，围绕中心点的划分）是具有代表性的k-medoids算法。

它最初随机选择k个对象作为中心点，该算法反复的用非代表对象（非中心点）代替代表对象，试图找出更好的中心点，以改进聚类的质量。

# 示例

```
## generate 25 objects, divided into 2 clusters.
x <- rbind(cbind(rnorm(10,0,0.5), rnorm(10,0,0.5)),
           cbind(rnorm(15,5,0.5), rnorm(15,5,0.5)))
pamx <- pam(x, 2)
pamx # Medoids: '7' and '25' ...
summary(pamx)
plot(pamx)
```