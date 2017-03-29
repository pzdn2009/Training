# gml

# 1. example code
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

解釋：
1. 創建irisDF
2. 訓練集，測試集
3. glm擬合，
4. 顯示擬合結果
5. 預測
6. 顯示預測結果
7. R風格的擬合
8. 描述
9. 排除“setosa”做二項分佈擬合


## 1.1 擬合結果

```
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

## 1.2 預測結果

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

# 2. 可用family

Family|Response Type|Supported Links
--|--|--
Gaussian|Continuous|Identity*, Log, Inverse
Binomial|Binary|Logit*, Probit, CLogLog
Poisson|Count|Log*, Identity, Sqrt
Gamma|Continuous|Inverse*, Idenity, Log
