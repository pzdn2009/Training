# svm函数

所在包：e1071

svm(formula, data = NULL, ..., subset, na.action =na.omit, scale = TRUE)

svm(x, y = NULL, scale = TRUE, type = NULL, kernel ="radial", degree = 3, gamma = if (is.vector(x)) 1 else 1 / ncol(x),
coef0 = 0, cost = 1, nu = 0.5,
class.weights = NULL, cachesize = 40, tolerance = 0.001, epsilon = 0.1,shrinking = TRUE, cross = 0, probability = FALSE, fitted = TRUE,..., subset, na.action = na.omit)

- formula：R公式。
- data：数据集
- x：解释变量
- y：响应变量
- type:向量机模型，分类、回归、异常模型检测。C，nu,one,eps,nu！！！
- scale：
- kernel：核函数，解决线性不可分的问题。线性，多项式，径向基核，神经网络
- degree：多项式内积函数中的参数
- gamma：
- coef0
- nu
