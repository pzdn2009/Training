# gbt

## gbt

Gradient Boosted Regression Trees

Gradient Boosting

## Sample Code

```r
# Load training data
df <- read.df("data/mllib/sample_libsvm_data.txt", source = "libsvm")
training <- df
test <- df

# Fit a GBT classification model with spark.gbt
model <- spark.gbt(training, label ~ features, "classification", maxIter = 10)

# Model summary
summary(model)

# Prediction
predictions <- predict(model, test)
showDF(predictions)
```

## 結果

```r
+-----+--------------------+----------+
|label|            features|prediction|
+-----+--------------------+----------+
|  0.0|(692,[127,128,129...|       0.0|
|  1.0|(692,[158,159,160...|       1.0|
|  1.0|(692,[124,125,126...|       1.0|
|  1.0|(692,[152,153,154...|       1.0|
|  1.0|(692,[151,152,153...|       1.0|
|  0.0|(692,[129,130,131...|       0.0|
|  1.0|(692,[158,159,160...|       1.0|
|  1.0|(692,[99,100,101,...|       1.0|
|  0.0|(692,[154,155,156...|       0.0|
|  0.0|(692,[127,128,129...|       0.0|
|  1.0|(692,[154,155,156...|       1.0|
|  0.0|(692,[153,154,155...|       0.0|
|  0.0|(692,[151,152,153...|       0.0|
|  1.0|(692,[129,130,131...|       1.0|
|  0.0|(692,[154,155,156...|       0.0|
|  1.0|(692,[150,151,152...|       1.0|
|  0.0|(692,[124,125,126...|       0.0|
|  0.0|(692,[152,153,154...|       0.0|
|  1.0|(692,[97,98,99,12...|       1.0|
|  1.0|(692,[124,125,126...|       1.0|
+-----+--------------------+----------+
only showing top 20 rows
```

