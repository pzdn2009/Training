# read.df

從數據源讀取生成一個SparkDataFrame。

read.df(path = NULL, source = NULL, schema = NULL,na.strings = "NA",...)
參數
* path: 文件路徑
* source: 外部數據源名稱
* schema: 定義在structType的數據模式。
* na.strings: Default string value for NA when source is "csv"
     ...: additional external data source specific named properties.

Eg:
```r
sparkR.session()
df1 <- read.df("path/to/file.json", source = "json")

schema <- structType(structField("name", "string"),structField("info", "map<string,double>"))
df2 <- read.df(mapTypeJsonPath, "json", schema)

df3 <- loadDF("data/test_table", "parquet", mergeSchema = "true")
```
# structed

Create a structField object that contains the metadata for a  single field in a schema.

structField(x, type, nullable = TRUE, ...)


參數：
- x: 字段名稱.
- type: 數據類型
- nullable: 是否可為空

```r
field1 <- structField("a", "integer")
field2 <- structField("c", "string")
field3 <- structField("avg", "double")
schema <-  structType(field1, field2, field3)
df1 <- gapply(df, list("a", "c"), function(key, x) { y <- data.frame(key, mean(x$b), stringsAsFactors = FALSE) }, schema)
```