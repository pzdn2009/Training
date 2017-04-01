# kmeans

所在包：stats包

# 1. 理论

**算法简介**
>选择K个点作为初始质心  
>repeat  
>    将每个点指派到最近的质心，形成K个簇  
>    重新计算每个簇的质心  
>until 簇不发生变化或达到最大迭代次数  

**欧几里得距离**

**余弦相似度**

**SSE**
SSE（Sum of Squares for Error）

缺点：
对极端值敏感。

#2. 官方示例
```r
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

#3. iris示例

ref:http://blog.csdn.net/yucan1001/article/details/23123043

```r
newiris <- iris;  
newiris$Species <- NULL;  #对训练数据去掉分类标记  
kc <- kmeans(newiris, 3);  #分类模型训练  
fitted(kc);  #查看具体分类情况  
table(iris$Species, kc$cluster);  #查看分类概括  
	  
#聚类结果可视化   
plot(newiris[c("Sepal.Length", "Sepal.Width")], col = kc$cluster, pch = as.integer(iris$Species));  #不同的颜色代表不同的聚类结果，不同的形状代表训练数据集的原始分类情况。  
points(kc$centers[,c("Sepal.Length", "Sepal.Width")], col = 1:3, pch = 8, cex=2);  
```

# 4. 函数

kmeans(x, centers, iter.max = 10, nstart = 1, algorithm = c("Hartigan-Wong", "Lloyd", "Forgy","MacQueen"), trace=FALSE)

# 5. 尋找最小的K

```r
# The dataset school_result is pre-loaded

# Set random seed. Don't remove this line.
set.seed(100)

# Explore the structure of your data
str(school_result)

# Initialise ratio_ss 
ratio_ss <- rep(0,7)

# Finish the for-loop. 
for (k in 1:7) {
  
  # Apply k-means to school_result: school_km
  school_km <- kmeans(school_result,k,nstart=20)
  
  # Save the ratio between of WSS to TSS in kth element of ratio_ss
 ratio_ss[k]<- school_km$tot.withinss / school_km$totss
  
}

# Make a scree plot with type "b" and xlab "k"
plot(ratio_ss, type='b',xlab='k')

```

# 6. kmeans cars example

```r
# The cars data frame is pre-loaded

# Set random seed. Don't remove this line
set.seed(1)

# Group the dataset into two clusters: km_cars
km_cars <- kmeans(cars, 2)

# Add code: color the points in the plot based on the clusters
plot(cars,col= km_cars$cluster)

# Print out the cluster centroids
km_cars$centers

# Replace the ___ part: add the centroids to the plot
points(km_cars$centers, pch = 22, bg = c(1, 2), cex = 2)
```

![](/assets/RkmeansCar.png)