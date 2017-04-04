# dist

dist(x, method = "euclidean", diag = FALSE, upper = FALSE, p = 2)

as.dist(m, diag = FALSE, upper = FALSE)

**参数：**
- x：数据框或者dist对象
- method：距离函数。"euclidean", "maximum", "manhattan", "canberra", "binary" or "minkowski". 其中之一

**x与y之间的距离测量：**
- **euclidean:欧式距离**

  sqrt(sum((x_i - y_i)^2)).
  $$ dist(X,Y) = \sqrt { \sum_{i=1}^n{(x_i-y_i)}^2 } $$
- maximum:切比雪夫距离，即上确界范数，supremum norm
  
  Chebyshev .
- **manhattan: 曼哈顿距离**，绝对值距离，或者称为城市距离
  
  $$ dist(X,Y) =  \sum_{i=1}^n{ |x_i-y_i |} $$
- **minkowski:闵可夫斯基距离**
  
  The p norm, the pth root of the sum of the pth powers of the differences of the components.明氏距离是欧氏距离的推广，是对多个距离度量公式的概括性的表述。
  $$ dist(X,Y) = ( \sum_{i=1}^n{|x_i-y_i|}^p )^{\frac{1}{p}} $$
