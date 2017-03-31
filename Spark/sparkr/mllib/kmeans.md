# kmeans

**Input**

Param name|Type(s)|Default|Description
--|--|--|--
featuresCol|Vector|"features"|Feature vector

**Output**

Param name|Type(s)|Default|Description
--|--|--|--
predictionCol|Int|"prediction"|Predicted cluster center

```r
# Fit a k-means model with spark.kmeans
irisDF <- suppressWarnings(createDataFrame(iris))
kmeansDF <- irisDF
kmeansTestDF <- irisDF
kmeansModel <- spark.kmeans(kmeansDF, ~ Sepal_Length + Sepal_Width + Petal_Length + Petal_Width,
                            k = 3)

# Model summary
summary(kmeansModel)

# Get fitted result from the k-means model
showDF(fitted(kmeansModel))

# Prediction
kmeansPredictions <- predict(kmeansModel, kmeansTestDF)
showDF(kmeansPredictions)
```

# summary結果
```r
> summary(kmeansModel)
$k
[1] 3

$coefficients
  Sepal_Length Sepal_Width Petal_Length Petal_Width
1 5.883607     2.740984    4.388525     1.434426   
2 5.006        3.428       1.462        0.246      
3 6.853846     3.076923    5.715385     2.053846   

$size
$size[[1]]
[1] 61

$size[[2]]
[1] 50

$size[[3]]
[1] 39


$cluster
SparkDataFrame[prediction:int]

$is.loaded
[1] FALSE
```