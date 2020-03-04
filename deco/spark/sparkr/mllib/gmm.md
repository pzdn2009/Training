# GMM

Gaussian Mixture Model

| Param name | Type\(s\) | Default | Description |
| :--- | :--- | :--- | :--- |
| featuresCol | Vector | "features" | Feature vector |

| Param name | Type\(s\) | Default | Description |
| :--- | :--- | :--- | :--- |
| predictionCol | Int | "prediction" | Predicted cluster center |
| probabilityCol | Vector | "probability" | Probability of each cluster |

```r
# Load training data
df <- read.df("data/mllib/sample_kmeans_data.txt", source = "libsvm")
training <- df
test <- df

# Fit a gaussian mixture clustering model with spark.gaussianMixture
model <- spark.gaussianMixture(training, ~ features, k = 2)

# Model summary
summary(model)

# Prediction
predictions <- predict(model, test)
showDF(predictions)
```

