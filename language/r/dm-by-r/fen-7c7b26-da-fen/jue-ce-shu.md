# 决策树

## 决策树

特點：

1. 貪心算法；當前節點的決策保證最優，但是增長節點時就不再改變了；
2. 會導致模型傾斜；
3. 會導致過度擬合，可以通過cp參數控制。
4. Rpart自動計算複雜度來終止樹的深度。
5. **cp** determines when the splitting up of the decision tree stops.
6. **minsplit** determines the minimum amount of observations in a leaf of the tree.

## 1. 理论

**熵entropy**

**纯度purity**

**信息增益information gain**

**基尼系数gini index**

**增益比率gain ratio**

**ID3算法**

伪代码：

```cpp
Generate_decision_tree(samples, feature_list) {
TreeNode N = new TreeeNode();  //创建新节点
 if (IsSameClass(samples)) {   //样本都是同类别
    N->class = samples[0].class;
    return N
}
if (feature_list.empty()) {   //遍历完所有类别
   return N;
}

Feature best = SelectBestInfoGainFeature(feature_list);  //获取最优的信息增益的那个特征
SetVisitedFeature(best);   //标记为已遍历

foreach ( level in best.GetAllLevels() )  {  //遍历所有水平
    TreeNode childN = Generate_decision_tree( GetCurrentSample(level), feature_list );
    N -> children.push(childN);
}
}
```

**C45算法**

特点：

* 用信息增益率来选择属性。ID3选择属性用的是子树的信息增益，这里可以用很多方法来定义信息，ID3使用的是熵（entropy， 熵是一种不纯度度量准则），也就是熵的变化值，而C4.5用的是信息增益率。
* 在决策树构造过程中进行剪枝，因为某些具有很少元素的结点可能会使构造的决策树过拟合（Overfitting），如果不考虑这些结点可能会更好。
* 对非离散数据也能处理。
* 能够对不完整数据进行处理。

IGR\(Ex,a\) = Info / H。 H = -sum\(Pvi\*log\(Pvi\)\)

## 2.示例

安装包

```r
> install.packages('rattle')
> install.packages('rpart.plot')
> install.packages('RColorBrewer')
> library(rattle)
> library(rpart.plot)
> library(RColorBrewer)
```

官方代码：

```r
fit <- rpart(Kyphosis ~ Age + Number + Start, data = kyphosis)
fit2 <- rpart(Kyphosis ~ Age + Number + Start, data = kyphosis,
              parms = list(prior = c(0.65, 0.35), split = "information"))
fit3 <- rpart(Kyphosis ~ Age + Number + Start, data=kyphosis,
              control = rpart.control(cp = 0.05))
par(mfrow = c(1,2), xpd = TRUE)
plot(fit)
text(fit, use.n = TRUE)
plot(fit2)
text(fit2, use.n = TRUE)
```

titanic示例代码：

```r
# Your train and test set are still loaded in
str(train)
str(test)

# Build the decision tree
my_tree_two <- rpart(Survived ~ Pclass + Sex + Age + SibSp + Parch + Fare + Embarked, data = train, method = "class")

# Visualize the decision tree using plot() and text()
plot(my_tree_two)
text(my_tree_two)

# Load in the packages to build a fancy plot
library(rattle)
library(rpart.plot)
library(RColorBrewer)
# Time to plot your fancy tree
fancyRpartPlot(my_tree_two)


# my_tree_two and test are available in the workspace

# Make predictions on the test set
my_prediction <- predict(my_tree_two, newdata = test, type = "class")

# Finish the data.frame() call
my_solution <- data.frame(PassengerId = test$PassengerId, Survived = my_prediction)

# Use nrow() on my_solution
nrow(my_solution)

# Finish the write.csv() call
write.csv(my_solution, file = "my_solution.csv", row.names = FALSE)
```

## 3. iris supervised example

```r
# Set random seed. Don't remove this line.
set.seed(1)

# Take a look at the iris dataset
str(iris)
summary(iris)

# A decision tree model has been built for you
tree <- rpart(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width,
              data = iris, method = "class")

# A dataframe containing unseen observations
unseen <- data.frame(Sepal.Length = c(5.3, 7.2),
                     Sepal.Width = c(2.9, 3.9),
                     Petal.Length = c(1.7, 5.4),
                     Petal.Width = c(0.8, 2.3))

# Predict the label of the unseen observations. Print out the result.
predict(tree,unseen,type="class")
```

結果：

```r
> predict(tree,unseen,type="class")
        1         2 
   setosa virginica 
Levels: setosa versicolor virginica
>
```

