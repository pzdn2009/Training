# knn

## knn

k-Nearest-Neighbors。

Instance-Based Learning。

* 将训练集保存在内存中
* 不像决策树那样有实际的模型
* 与训练集比较未知的实例
* 数值型变量：归一化处理
* 类别型变量：类别因子组成的0,1矩阵，当因子出现则为1，不出现则为0。

\frac{\mathbf{x} - \min\(\mathbf{x}\)}{\max\(\mathbf{x}\) - \min\(\mathbf{x}\)

思想：如果一个样本在特征空间中的k个最相似\(即特征空间中最邻近\)的样本中的大多数属于某一个类别，则该样本也属于这个类别。

**做法：** 1. 首先，计算新样本与训练样本之间的距离，找到距离最近的K个邻居； 2. 然后，根据这些邻居所属的类别来判定新样本的类别，如果它们都属于同一个类别，那么新样本也属于这个类；否则，对每个后选类别进行评分，按照某种规则确定新样本的类别。

**伪代码：**

```text
(1)计算已知类别数据集中点与当前点之间的距离（采用欧式距离公式:）
(2)按照距离递增次序排列
(3)选取与当前点距离最小的k个点
(4)确定前k个点所在类别出现的频率
(5)返回前k个点出现频率最高的类别作为当前点的预测分类。
```

**K选择：**

k太小，分类结果易受噪声点影响；k太大，近邻中又可能包含太多的其它类别的点。（对距离加权，可以降低k值设定的影响）

k值通常是采用交叉检验来确定（以k=1为基准）

经验规则：k一般低于训练样本数的平方根

**Voronoi Diagram**

## 1. Normalize

```r
set.seed(1)
inputData <- read.csv("F:\\Projects\\R\\Titanic\\train.csv",header =T)
inputData <- inputData[,c("Survived","Pclass","Sex","Age")]
inputData <- na.omit(inputData)

n <- nrow(inputData)
shuffleTrain <- inputData[sample(n),]

train <- shuffleTrain[1:round(0.7 * n),]
test <- shuffleTrain[round(0.7 * n+1):n,]

# Store the Survived column of train and test in train_labels and test_labels
train_labels <- train$Survived
test_labels <- test$Survived

# Copy train and test to knn_train and knn_test
knn_train <- data.frame("Age"=train$Age,"Survived"=train$Survived,"Pclass"=as.integer( train$Pclass),"Sex"=as.numeric(train$Sex))
knn_test <- data.frame("Age"=test$Age,"Survived"=test$Survived,"Pclass"=as.integer( test$Pclass),"Sex"=as.numeric(test$Sex))

# Drop Survived column for knn_train and knn_test
knn_train$Survived <- NULL
knn_test$Survived <- NULL

# Normalize Pclass
min_class <- min(knn_train$Pclass)
max_class <- max(knn_train$Pclass)
knn_train$Pclass <- (knn_train$Pclass - min_class) / (max_class - min_class)
knn_test$Pclass <- (knn_test$Pclass - min_class) / (max_class - min_class)

# Normalize Age
min_age <- min(knn_train$Age)
max_age <- max(knn_train$Age)
knn_train$Age <- (knn_train$Age - min_age) / (max_age - min_age)
knn_test$Age <- (knn_test$Age - min_age) / (max_age - min_age)

# prediction
pred <- knn(train = knn_train, test = knn_test, cl = as.factor(train_labels) , k = 5,prob = FALSE, use.all = TRUE)
conf <- table(test_labels,pred)
conf
```

## 2. find K

```r
library(class)
range <- 1:round(0.2 * nrow(knn_train))
accs <- rep(0, length(range))

for (k in range) {

  # Fill in the ___, make predictions using knn: pred
  pred <- knn(knn_train, knn_test, cl=train_labels, k = k)

  # Fill in the ___, construct the confusion matrix: conf
  conf <- table(test_labels, pred)

  # Fill in the ___, calculate the accuracy and store it in accs[k]
  accs[k] <- sum(diag(conf))/sum(conf)
}

# Plot the accuracies. Title of x-axis is "k".
plot(range, accs, xlab = "k")

# Calculate the best k
which.max(accs)
```

