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