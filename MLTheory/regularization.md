# Regularization

http://www.cnblogs.com/jianxinzhou/p/4083921.html

正则化是针对**过拟合**而提出的，以为在求解模型最优的是一般优化最小的经验风险，现在在该经验风险上加入**模型复杂度**这一项（正则化项是模型参数向量的范数），并使用一个rate比率来**权衡模型复杂度与以往经验风险的权重**，如果模型复杂度越高，结构化的经验风险会越大，现在的目标就变为了结构经验风险的最优化，可以防止模型训练过度复杂，有效的降低过拟合的风险。

