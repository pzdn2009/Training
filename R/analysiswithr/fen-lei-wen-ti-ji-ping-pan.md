#  分类问题及评判

典型的分类问题：

- 识别垃圾邮件
- 根据商品目录对商品分类
- 识别即将违约的贷款
- 将顾客指派到某个顾客类

常用分类方法：

* 朴素贝叶斯
* 决策树
* 逻辑斯蒂回归
* 支持向量机

评判：

**Accuracy** and **Error**。
其中，Error = 1 – Accuracy .

# 混淆矩阵

Confusion matrix。

Truth/prediction|positive|negative
--|--|--
true|TP|FN
false|FP|TN

- TP：真阳性
- FP：假阳性
- FN：真阴性
- FN：假阴性

含义词：
- 精度accuracy：（TP + TN） / (TP+FP+FN+TN)，对角
- 准确率precision：TP/(TP+FP)，左列
- 召回率recall：TP/(TP+FN)，上行
- 灵敏度sensitivity：TP/(TP+FN)，上行
- 特异度specificity：TN/(TN+FP)，下行
- F1:2*precision*recall/(precision+recall)

eg：
```r
>table(titanic$Survived,pred)
  pred
      1   0
  1 212  78
  0  53 371
```