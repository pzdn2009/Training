# 創建SparkDataFrames

## 創建SparkDataFrames

從本地數據集創建，傳遞data.frame作為參數

* as.DataFrame
* createDataFrame 
* read.df

## 1. From local data frames

```r
> head(faithful)
> class(faithful)
> dim(faithful)

> df <- as.DataFrame(faithful)

> head(df)
  eruptions waiting
1     3.600      79
2     1.800      54
3     3.333      74
4     2.283      62
5     4.533      85
6     2.883      55

> printSchema(df)
root
 |-- eruptions: double (nullable = true)
 |-- waiting: double (nullable = true)
```

## 2. From Data Sources

```r
> people <- read.df("./examples/src/main/resources/people.json", "json")

> head(people)
  age    name
1  NA Michael
2  30    Andy
3  19  Justin
```

## 3. From Hive Tables

也可以創建DF連到HIVE表。這需要創建一個能夠支持訪問Hive元數據庫的SparkSession。在SparkR中，創建SparkSession時使用參數\(enableHiveSupport = TRUE\)。

```r
> sql("CREATE TABLE IF NOT EXISTS src (key INT, value STRING)")
SparkDataFrame[]

> sql("LOAD DATA LOCAL INPATH 
'/home/pzdn/app/spark2.1.0/examples/src/main/resources/kv1.txt' INTO TABLE src")
SparkDataFrame[]

> results <- sql("FROM src SELECT key, value")
> head(results)
  key   value
1 238 val_238
2  86  val_86
3 311 val_311
4  27  val_27
5 165 val_165
6 409 val_409
```

