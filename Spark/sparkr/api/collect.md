# collect

Collects all the elements of a SparkDataFrame and coerces them into an R **data.frame**。

輸出目標：data.frame

```r
sparkR.session()
path <- "path/to/file.json"
df <- read.json(path)
collected <- collect(df)
firstName <- collected[[1]]$name
```