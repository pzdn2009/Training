# Naive Bayes

基于独立假设的，即假设样本的每一个特征与其他特征都不相关。

**贝叶斯定理**
$$ P(B|A) = \frac{P(A|B)P(B)}{P(A)} $$

**联合概率**

$$ P(AB) = P(A) \cdot P(B) $$

**条件概率**

$$ P(A|B) = \frac{P(AB)}{P(B)} $$

**先验概率**
Prior Probability.

是指根据以往经验和分析得到的概率，如全概率公式，它往往作为"由因求果"问题中的"因"出现的概率·

先验概率的计算比较简单，没有使用贝叶斯公式；而后验概率的计算，要使用贝叶斯公式.

**后验概率**
Posterior probability.

后验概率是指在得到“结果”的信息后重新修正的概率，如贝叶斯公式中的。是“执果寻因”问题中的"果"。

# 原理

## Training
1. 设样本集为 $$ x = \{X_1,X_2,...,X_n \} $$，其中 $$ X_i = \{a_1,a_2,...,a_k\}$$，所有类别集为 $$ Y = \{ C_1,C2,...,C_m\} $$。
   
   分别为观测，特征，分类。
2. 计算先验概率 $$ P(C_i) $$。
   
   每个类别在训练集中出现的概率。
3. 计算条件概率 $$ P(a_1|C_1) $$，$$ P(a_2|C_1) $$，...，$$ P(a_k|C1) $$；$$ P(a_1|C_2) $$，$$ P(a_2|C_2) $$，...，$$ P(a_k|C2) $$；$$ P(a_1|C_m) $$，$$ P(a_2|C_m) $$，...，$$ P(a_k|Cm) $$
   
   每个特征在每个类别中的条件概率。

## Test
1. 设 $$ X= \{ a_1,a_2,...,a_k \} $$为待分类项，而每个ai为X的一个特征属性
2. 计算 $$ P(X|C_i) P(C_i) = P(a_1|C_i) P(a_2|C_i)...P(a_k|C_i) \cdot P(C_i) = P(C_i) \prod_{j=1}^m{ P(a_j|C_i)} $$
3. 根据贝叶斯定理求后验概率P(Ci|X)，得到X属于Ci类别的后验概率；根据最大后验概率判断所属类别。$$ P(C_\alpha | X) = max \{P(C_i|X)\},C_\alpha \in Y $$，则测试样本属于类别 $$ C_\alpha $$
