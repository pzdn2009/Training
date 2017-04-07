# 打分问题及评判

问题：

- 预测关键词广告的价值
- 估计贷款会拖欠的概率
- 预测营销活动将增加多少交易量或销售量

方法：

打分即回归Regression，效果是预测或者预报数值

- 线性回归
- 逻辑斯蒂回归（值在0~1）

# 评判

- 残差：预测值与实际结果之间的偏差（y方向）。
  
  $$ res_i = y_i - \hat{y_i} $$
- RMSE均方根误差： Root-Mean-Square-Error.

  sqrt(mean(d$prediction-d$y)^2))
  
  $$ \sqrt{ \frac{1}{N} \sum_{i=1}^{N}{res_i}^2 } $$
- R-平方：
  
  1-sum((d$prediction-d$y)^2/sum((mean(d$y)-d$y)^2)))
  
  $$ R^2 = 1 -{\frac{SS_{res}}{SS_{tot}}} $$
  
  $$ SS_{res} = \sum{ ( y_i - \hat{y_i} )^2 } $$
  
  $$ SS_{tot} = \sum{(y_i - \overline{y_i})^2} $$
  
- 相关性：Pearson，Spearman,kendall
- 绝对误差：
