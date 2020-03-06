# 概率模型及评判

## 概率模型及评判

概率模型既可以判断某个事项是否在给定的类别中，也可以范湖该事项在这个类中的概率估计值。

* 决策树
* 逻辑斯蒂回归

## 评判

* ROC

  發源于信号检测理论。[https://zh.wikipedia.org/wiki/ROC%E6%9B%B2%E7%BA%BF\#.E6.9B.B2.E7.B7.9A.E4.B8.8B.E9.9D.A2.E7.A9.8D.EF.BC.88AUC.EF.BC.89](https://zh.wikipedia.org/wiki/ROC%E6%9B%B2%E7%BA%BF#.E6.9B.B2.E7.B7.9A.E4.B8.8B.E9.9D.A2.E7.A9.8D.EF.BC.88AUC.EF.BC.89) Receiver Operator Characteristic Curve\(ROC Curve\)。

  TPR：y轴。TPR = TP / \(TP + FN\)。在所有实际为**阳性**的样本中，被**正确地**判断为阳性之比率。FP：诊断为有，实际却没有。

  FPR：x轴。FPR = FP / \(FP + TN\)。在所有实际为阴性的样本中，被**错误地**判断为阳性之比率。FN：诊断为没有，实际却有

  画曲线：

  曲线解释：越接近Y轴越好。理想目标：TPR=1，FPR=0,即图中\(0,1\)点，故ROC曲线越靠拢\(0,1\)点，越偏离45度对角线越好，Sensitivity、Specificity越大效果越好。

```r
# Code of previous exercise
set.seed(1)
tree <- rpart(income ~ ., train, method = "class")
probs <- predict(tree, test, type = "prob")[,2]

# Load the ROCR library
library(ROCR)

# Make a prediction object: pred
pred <- prediction(probs,labels=test$income)

# Make a performance object: perf
perf <-  performance(pred,"tpr","fpr")

# Plot this curve
plot(perf)
```

![](../../../.gitbook/assets/roc%20%281%29.png)

* AUC

  Area Under Curve。

  ROC线下的面积。

```r
> perf@y.values[[1]]
[1] 0.00000000 0.06634657 0.10257529 0.52204278 0.89436927 1.00000000
```

* 对数似然估计
* 偏离值
* AIC
* 熵

