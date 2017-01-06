# kmeans

所在包：stats包


#1. 官方示例
```
require(graphics)

# a 2-dimensional example
x <- rbind(matrix(rnorm(100, sd = 0.3), ncol = 2),
           matrix(rnorm(100, mean = 1, sd = 0.3), ncol = 2))
colnames(x) <- c("x", "y")
(cl <- kmeans(x, 2))
plot(x, col = cl$cluster)
points(cl$centers, col = 1:2, pch = 8, cex = 2)
```
![](/assets/kmeansOfficial.png) 

#2. iris示例

ref:http://blog.csdn.net/yucan1001/article/details/23123043

```
newiris <- iris;  
newiris$Species <- NULL;  #对训练数据去掉分类标记  
kc <- kmeans(newiris, 3);  #分类模型训练  
fitted(kc);  #查看具体分类情况  
table(iris$Species, kc$cluster);  #查看分类概括  
	  
#聚类结果可视化   
plot(newiris[c("Sepal.Length", "Sepal.Width")], col = kc$cluster, pch = as.integer(iris$Species));  #不同的颜色代表不同的聚类结果，不同的形状代表训练数据集的原始分类情况。  
points(kc$centers[,c("Sepal.Length", "Sepal.Width")], col = 1:3, pch = 8, cex=2);  
```

# 3. 函数

kmeans(x, centers, iter.max = 10, nstart = 1, algorithm = c("Hartigan-Wong", "Lloyd", "Forgy","MacQueen"), trace=FALSE)
