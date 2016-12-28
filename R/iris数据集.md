#iris数据集

这是著名（Fisher的或安德森）的鸢尾花数据集，给出了萼片长度和宽度、花瓣长度和宽度四个变量的测量结果，以厘米为单位。分别从各3种鸢尾花（山鸢尾，云芝和锦葵）各选50朵。

数据集类别
```
> class(iris)
[1] "data.frame"

> nrow(iris)
[1] 150
> ncol(iris)
[1] 5

> dim(iris)
[1] 150   5
```

查看数据
```
> head(iris)
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa
```
Sepal：萼片
Petal：花瓣
setosa：山鸢尾
