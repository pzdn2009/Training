# glm

## gml

**Generalized Linear Model**.

在统计学里，对特定变量之间的关系进行建模、分析最常用的手段之一就是回归分析。回归分析的输出变量通常记做YY，也称为因变量\(dependent\)、响应变量\(response\)、被解释变量\(explained\)、被预测变量\(predicted\)、从属变量\(regressand\)；输入变量通常记做x1x1,…,xpxp，也称为自变量\(independent\)、控制变量\(control&controlled\)、解释变量\(explanatory\)、预测变量\(predictor\)、回归量\(regressor\)。

特點：

1. 响应变量的分布推广至指数分散族\(exponential dispersion family\)：比如正态分布、泊松分布、二项分布、负二项分布、伽玛分布、逆高斯分布。
2. 预测量xi和未知参数βi的非随机性：仍然假设预测量xi具有非随机性、可测且不存在测量误差；未知参数βi认为是未知且不具有随机性的常数。
3. 研究对象：广义线性模型的主要研究对象仍然是响应变量的均值E\[Y\]。
4. 联接方式：广义线性模型里采用的联连函数\(link function\)理论上可以是任意的，而不再局限于f\(x\)=x。当然了联接函数的选取必然地必须适应于具体的研究案例。标准联接函数\(canonical link or standard link\)，如正态分布对应于恒等式，泊松分布对应于自然对数函数等。

## 1. example code

```r
# To run this example use
# ./bin/spark-submit examples/src/main/r/ml/glm.R

# Load SparkR library into your R session
library(SparkR)

# Initialize SparkSession
sparkR.session(appName = "SparkR-ML-glm-example")

# $example on$
irisDF <- suppressWarnings(createDataFrame(iris))
# Fit a generalized linear model of family "gaussian" with spark.glm
gaussianDF <- irisDF
gaussianTestDF <- irisDF
gaussianGLM <- spark.glm(gaussianDF, Sepal_Length ~ Sepal_Width + Species, family = "gaussian")

# Model summary
summary(gaussianGLM)

# Prediction
gaussianPredictions <- predict(gaussianGLM, gaussianTestDF)
showDF(gaussianPredictions)

# Fit a generalized linear model with glm (R-compliant)
gaussianGLM2 <- glm(Sepal_Length ~ Sepal_Width + Species, gaussianDF, family = "gaussian")
summary(gaussianGLM2)

# Fit a generalized linear model of family "binomial" with spark.glm
# Note: Filter out "setosa" from label column (two labels left) to match "binomial" family.
binomialDF <- filter(irisDF, irisDF$Species != "setosa")
binomialTestDF <- binomialDF
binomialGLM <- spark.glm(binomialDF, Species ~ Sepal_Length + Sepal_Width, family = "binomial")

# Model summary
summary(binomialGLM)

# Prediction
binomialPredictions <- predict(binomialGLM, binomialTestDF)
showDF(binomialPredictions)
# $example off$
```

解釋： 1. 創建irisDF 2. 訓練集，測試集 3. glm擬合， 4. 顯示擬合結果 5. 預測 6. 顯示預測結果 7. R風格的擬合 8. 描述 9. 排除“setosa”做二項分佈擬合

### 1.1 擬合結果

```r
Deviance Residuals: 
(Note: These are approximate quantiles with relative error <= 0.01)
     Min        1Q    Median        3Q       Max  
-1.30711  -0.26011  -0.06189   0.19111   1.41253  

Coefficients:
                    Estimate  Std. Error  t value  Pr(>|t|)  
(Intercept)         2.2514    0.36975     6.0889   9.5681e-09
Sepal_Width         0.80356   0.10634     7.5566   4.1873e-12
Species_versicolor  1.4587    0.11211     13.012   0         
Species_virginica   1.9468    0.10001     19.465   0         

(Dispersion parameter for gaussian family taken to be 0.1918059)

    Null deviance: 102.168  on 149  degrees of freedom
Residual deviance:  28.004  on 146  degrees of freedom
AIC: 183.9

Number of Fisher Scoring iterations: 1
```

### 1.2 預測結果

```r
> showDF(gaussianPredictions)
+------------+-----------+------------+-----------+-------+-----+------------------+
|Sepal_Length|Sepal_Width|Petal_Length|Petal_Width|Species|label|        prediction|
+------------+-----------+------------+-----------+-------+-----+------------------+
|         5.1|        3.5|         1.4|        0.2| setosa|  5.1| 5.063856384860279|
|         4.9|        3.0|         1.4|        0.2| setosa|  4.9| 4.662075934441676|
|         4.7|        3.2|         1.3|        0.2| setosa|  4.7| 4.822788114609117|
|         4.6|        3.1|         1.5|        0.2| setosa|  4.6| 4.742432024525396|
|         5.0|        3.6|         1.4|        0.2| setosa|  5.0|    5.144212474944|
|         5.4|        3.9|         1.7|        0.4| setosa|  5.4| 5.385280745195162|
|         4.6|        3.4|         1.4|        0.3| setosa|  4.6| 4.983500294776558|
|         5.0|        3.4|         1.5|        0.2| setosa|  5.0| 4.983500294776558|
|         4.4|        2.9|         1.4|        0.2| setosa|  4.4| 4.581719844357954|
|         4.9|        3.1|         1.5|        0.1| setosa|  4.9| 4.742432024525396|
|         5.4|        3.7|         1.5|        0.2| setosa|  5.4| 5.224568565027721|
|         4.8|        3.4|         1.6|        0.2| setosa|  4.8| 4.983500294776558|
|         4.8|        3.0|         1.4|        0.1| setosa|  4.8| 4.662075934441676|
|         4.3|        3.0|         1.1|        0.1| setosa|  4.3| 4.662075934441676|
|         5.8|        4.0|         1.2|        0.2| setosa|  5.8| 5.465636835278883|
|         5.7|        4.4|         1.5|        0.4| setosa|  5.7|5.7870611956137665|
|         5.4|        3.9|         1.3|        0.4| setosa|  5.4| 5.385280745195162|
|         5.1|        3.5|         1.4|        0.3| setosa|  5.1| 5.063856384860279|
|         5.7|        3.8|         1.7|        0.3| setosa|  5.7| 5.304924655111442|
|         5.1|        3.8|         1.5|        0.3| setosa|  5.1| 5.304924655111442|
+------------+-----------+------------+-----------+-------+-----+------------------+
only showing top 20 rows
```

## 2. 可用family

每一种响应分布（指数分布族）允许各种关联函数将均值和线性预测器关联起来。

| Family | Response Type | Supported Links |
| :--- | :--- | :--- |
| Gaussian | Continuous | Identity\*, Log, Inverse |
| Binomial | Binary | Logit\*, Probit, CLogLog |
| Poisson | Count | Log\*, Identity, Sqrt |
| Gamma | Continuous | Inverse\*, Idenity, Log |

## 3. 問題

**glm.fit:算法没有聚合** 

結果時Pr值大，不顯著，需要增大迭代次數。

