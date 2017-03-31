# sql

Executes a SQL query using Spark, returning the result as a SparkDataFrame.

sql(sqlQuery)

```r
sparkR.session()
path <- "path/to/file.json"
df <- read.json(path)

createOrReplaceTempView(df, "table")

new_df <- sql("SELECT * FROM table")

```