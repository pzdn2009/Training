# 模型持久化

以下代碼示例，通過SparkR加載保存 MLLib。

```r
irisDF <-suppressWarnings(createDataFrame(iris)) # Fit a generalized linear model of family "gaussian" with spark.glm 
gaussianDF <- irisDF 
gaussianTestDF <- irisDF gaussianGLM <- spark.glm(gaussianDF, Sepal_Length ~ Sepal_Width + Species, family = "gaussian")
# Save and then load a fitted MLlib model 
modelPath <- tempfile(pattern = "ml", fileext = ".tmp") 
write.ml(gaussianGLM, modelPath) 
gaussianGLM2 <- read.ml(modelPath)
# Check model summary 
summary(gaussianGLM2)
# Check model prediction 
gaussianPredictions <- predict(gaussianGLM2, gaussianTestDF)
showDF(gaussianPredictions)
unlink(modelPath)
```

> Find full example code at “examples/src/main/r/ml/ml.R”

關鍵步驟：

1. 生成臨時文件路徑，template；
2. 寫ml模型，write.ml
3. 讀取ml模型，read.ml
4. 刪除臨時文件，unlink

