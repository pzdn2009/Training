# R分析

**RStudio简介**

Rstudio：https://www.rstudio.com/products/rstudio/download/
使用免費版的就夠用了。

功能：History，Environment，Plot，Dataset View，CLI，Script，GIT。

收費的是BS架構，可以看到Dataset顯示的js。


# 1. 查看数据
```r
str
head
tail
dim
names
summary
class
levels:显示各个水平
length
attributes
nrow
ncol
```

# 2. 数据预处理

##抽样

##NA值处理

判断
```r
is.na
```

取众数
```r
# Since many passengers embarked at Southampton, we give them the value S.
all_data$Embarked[c(62, 830)] <- "S"
```

取中位数
```
all_data$Fare[1044] <- median(all_data$Fare, na.rm = TRUE)
```

取拟合回归值（连续变量）
```r
# This time you give method = "anova" since you are predicting a continuous variable.
library(rpart)
predicted_age <- rpart(Age ~ Pclass + Sex + SibSp + Parch + Fare + Embarked + Title + family_size,
                       data = all_data[!is.na(all_data$Age),], method = "anova")
all_data$Age[is.na(all_data$Age)] <- predict(predicted_age, all_data[is.na(all_data$Age),])
```
>选取非空数据集来拟合，预测空数据集。

## 类型转化
```r
as.factor(Survived)
```

## 离散化
二分离散
```r
train$Child[train$Age] <- 0
train$Child[train$Age<18] <- 1
```

分段离散
```r
<10
[10,20)
[20,30)
[30,+)
```

## 增加列和删除列
```r
test$Survived <- rep(0,418)
test$Child <- NULL
```

## 构造数据框
```r
my_solution <- data.frame(PassengerId = test$PassengerId, Survived = my_prediction)
```

## 划分 Training & Test

训练集和测试集的构造比较好的是随机混洗，且采用多级机制。

混洗：
```r
n <- nrow(titanic) #总的行数
shuffled <- titanic[sample(n),] #Shuffle the dataset, call the result shuffled
```

安装7:3的比例划分：

```r
train <- shuffled[1:round(0.7 * n),]
test <- shuffled[(round(0.7 * n) + 1):n,]
```

# 3. 建模

机器学习的特点：
>It actually involves making predictions about observations based on previous information.

分类Classfication，聚类Clustering，回归Regression

## 选择算法

## 交叉验证

Cross Validation.

```r
set.seed(1)
accs <- rep(0,6) #初始化精度向量
for (i in 1:6) {
  # 索引，按照六份的比例
  indices <- (((i-1) * round((1/6)*nrow(shuffled))) + 1):
  ((i*round((1/6) * nrow(shuffled))))
  
  train <- shuffled[-indices,]
  test <- shuffled[indices,]
  
  # 建模
  tree <- rpart(Survived ~ ., train, method = "class")
  
  # 预测
  pred <- predict(tree,test,type="class")
  
  # Assign the confusion matrix to conf
  conf <- table(test$Survived,pred)
  
  # 计算精度
  accs[i] <- sum(diag(conf))/sum(conf)
}

# 取精度的平均值
mean(accs)
```

# 4. 特征提取

- Feature engineering really boils down to(歸結到) the human element in machine learning.
- How much you understand the data, with your human intuition and creativity, can make the difference.
- Enter feature engineering: creatively engineering your own features by combining the different existing variables.
- In fact, feature engineering has been described as easily the most important factor in determining the success or failure of your predictive model.
