# als

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

> 
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
