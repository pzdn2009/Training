# 創建SparkDataFrames

從本地數據集創建，傳遞data.frame作為參數

- as.DataFrame
- createDataFrame 
- read.df

# 1. From local data frames

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

# 2. From Data Sources

```r
> people <- read.df("./examples/src/main/resources/people.json", "json")

> head(people)
  age    name
1  NA Michael
2  30    Andy
3  19  Justin
```