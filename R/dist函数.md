# dist

dist(x, method = "euclidean", diag = FALSE, upper = FALSE, p = 2)

as.dist(m, diag = FALSE, upper = FALSE)

**参数：**
- x：数据框或者dist对象
- method：距离函数。"euclidean", "maximum", "manhattan", "canberra", "binary" or "minkowski". 其中之一

**x与y之间的距离测量：**
- euclidean:欧式距离

  sqrt(sum((x_i - y_i)^2)).
- maximum:切比雪夫距离，即上确界范数，supremum norm
- manhattan: 曼哈顿距离，绝对值距离，或者称为城市距离
- minkowski:闵可夫斯基距离

  The p norm, the pth root of the sum of the pth powers of the differences of the components.


