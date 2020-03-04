# 神经网络

## 神经网络

nnet包

ANN：Artificial Neutral Network。

BP（Back Propagation）神经网络模型拓扑结构包括输入层（input）、隐层\(hide layer\)和输出层\(output layer\)。

* 输入层神经元的个数由样本属性的维度决定；
* 输出层神经元的个数由样本分类个数决定；
* 隐藏层的层数和每层的神经元个数由用户指定。

神经元

**激励值** N个输入及其对应的权重乘积之和。

隐藏层

前馈网络feedforward network

梯度下降

黑塞矩阵

BP算法

## 示例

class.nd函数：将向量变成一个visited矩阵。

```text
> library(nnet)
> v1 <- c("a","b","c","d")
> v2 <- c(1,2,1,3)
> class.ind(v1)
     a b c d
[1,] 1 0 0 0
[2,] 0 1 0 0
[3,] 0 0 1 0
[4,] 0 0 0 1
> class.ind(v2)
     1 2 3
[1,] 1 0 0
[2,] 0 1 0
[3,] 1 0 0
[4,] 0 0 1
```

nnet函数：建立单隐藏层或无隐藏层的前馈人工网络神经模型

