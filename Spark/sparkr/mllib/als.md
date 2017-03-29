# als

# 原型

spark.als(data, ratingCol = "rating",userCol = "user", itemCol = "item", rank = 10, regParam = 0.1,
maxIter = 10, nonnegative = FALSE, implicitPrefs = FALSE, alpha = 1,numUserBlocks = 10, numItemBlocks = 10, checkpointInterval = 10,seed = 0)

參數：
- data: DF
- ratingCol:得分的列名
- userCol:用戶id的列名。id必須是整型。
- itemCol: 物品的列名。Id必須是整型。
- rank: 矩陣因子等級。
- regParam:回歸參數
- maxIter: 最大迭代次數

# Sample Code：

```r
# Load training data
data <- list(list(0, 0, 4.0), list(0, 1, 2.0), list(1, 1, 3.0),
             list(1, 2, 4.0), list(2, 1, 1.0), list(2, 2, 5.0))
df <- createDataFrame(data, c("userId", "movieId", "rating"))
training <- df
test <- df

# Fit a recommendation model using ALS with spark.als
model <- spark.als(training, maxIter = 5, regParam = 0.01, userCol = "userId",
                   itemCol = "movieId", ratingCol = "rating")

# Model summary
summary(model)

# Prediction
predictions <- predict(model, test)
showDF(predictions)

```

# 結果

```r
+------+-------+------+----------+                                              
|userId|movieId|rating|prediction|
+------+-------+------+----------+
|   1.0|    1.0|   3.0|  2.997274|
|   2.0|    1.0|   1.0| 1.0015064|
|   0.0|    1.0|   2.0| 1.9999025|
|   1.0|    2.0|   4.0| 3.9981441|
|   2.0|    2.0|   5.0| 4.9958687|
|   0.0|    0.0|   4.0| 3.9945998|
+------+-------+------+----------+
```