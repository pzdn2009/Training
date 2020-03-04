# 关联分析

## 关联分析

## 1. 理论

### 项集Itemset

包含0个或多个项的集合，如果包含k个项，则称为k-项集。

### 关联规则Association Rule

如X→Y的蕴含表达式，其中X和Y是不相交的项集，X∩Y=∅。

### 支持度Support

S（X→Y） = P\(X,Y\)。同时含有X和Y的概率。

S（Z） &gt;= minsup的项集Z，称为频繁项集（Frequent Itemset）。

### 置信度Confidence

表示在关联规则的先决条件下X发生的条件下，关联结果Y发生的概率，即含有X的项集中，同时含有Y的可能性：

C（X→Y） = P（Y\|X） = P（X，Y）/P（X）

阈值mincon

### 提升度Lift

表示在含有X的条件下同时含有Y的可能性 与 没有这个条件下项集中含有Y的可能性之比，即在P（Y）基础上，P（Y\|X）：

L（X→Y） = P（Y\|X） / P \(Y\) = C\(X→Y\) /P\(Y\)

## 2. 示例

```text
> install.packages("arules") 
> library(arules)
> data(Groceries)

> r0 <- apriori(Groceries,parameter=list(support = 0.001,confidence=0.5)) # 设置支持度和信度
Apriori

Parameter specification:
 confidence minval smax arem  aval originalSupport maxtime support minlen maxlen target   ext
        0.5    0.1    1 none FALSE            TRUE       5   0.001      1     10  rules FALSE

Algorithmic control:
 filter tree heap memopt load sort verbose
    0.1 TRUE TRUE  FALSE TRUE    2    TRUE

Absolute minimum support count: 9 

set item appearances ...[0 item(s)] done [0.00s].
set transactions ...[169 item(s), 9835 transaction(s)] done [0.00s].
sorting and recoding items ... [157 item(s)] done [0.00s].
creating transaction tree ... done [0.02s].
checking subsets of size 1 2 3 4 5 6 done [0.00s].
writing ... [5668 rule(s)] done [0.00s].
creating S4 object  ... done [0.00s].

> r0 # 最终规则结果
set of 5668 rules

> inspect(r0[1:10]) #查看前10条规则
     lhs                    rhs                support     confidence lift    
[1]  {honey}             => {whole milk}       0.001118454 0.7333333  2.870009
[2]  {tidbits}           => {rolls/buns}       0.001220132 0.5217391  2.836542
[3]  {cocoa drinks}      => {whole milk}       0.001321810 0.5909091  2.312611
[4]  {pudding powder}    => {whole milk}       0.001321810 0.5652174  2.212062
[5]  {cooking chocolate} => {whole milk}       0.001321810 0.5200000  2.035097
[6]  {cereals}           => {whole milk}       0.003660397 0.6428571  2.515917
[7]  {jam}               => {whole milk}       0.002948653 0.5471698  2.141431
[8]  {specialty cheese}  => {other vegetables} 0.004270463 0.5000000  2.584078
[9]  {rice}              => {other vegetables} 0.003965430 0.5200000  2.687441
[10] {rice}              => {whole milk}       0.004677173 0.6133333  2.400371
```

