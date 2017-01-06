# Apriori

所在包：
arules包


eclat(data, parameter = NULL, control = NULL)

apriori(data, parameter = NULL, appearance = NULL, control = NULL)

- data:数据集。
- parameter: APparameter或者命名列表。默认支持度为0.1，最小信度为0.8，最大的项为10，最大时间为5s。
- appearance: APappearance或者命名列表。约束：包含这些参数lhs, rhs, both, items, none。lhs、rhs、both这些是对规则的，items是对项集的。none表示规则和项集都不能出现
- control:sort。tree，heap。

重点
APparameter：
- support
- minlen
- maxlen
- target
 - "frequent itemsets"
 - "maximally frequent itemsets"
 - "closed frequent itemsets"
 - "rules" (only available for Apriori)
 - "hyperedgesets" (only available for Apriori; see references for the definition of association hyperedgesets)
- confidence
