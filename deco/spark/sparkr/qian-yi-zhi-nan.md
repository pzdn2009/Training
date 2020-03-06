# 遷移指南

## 遷移指南

## 2.1.0

* Join默認不再產生笛卡爾積，使用crossJoin替代。

## 1.6.x to 2.0

* 刪除了table函數，使用tableToDF替代。
* 類名DataFrame被重命名為SparkDataFrame，避免命名衝突。
* Spark的SQLContext，HiveContext被SparkSession所取代。使用 sparkR.session\(\)來初始化SparkSession，而不是用sparkR.init\(\)。一旦執行了sparkR.session\(\)，再當前SparkSession中就可以執行SparkDataFrame操作了。
* sparkR.session不支持sparkExecutorEnv參數。為了設置執行器的環境，需要用前綴“spark.executorEnv.VAR\_NAME”設置Spark配置屬性，例如，“spark.executorEnv.PATH”
* 以下函數不再需要sqlcontext作為參數：createDataFrame, as.DataFrame, read.json, jsonFile, read.parquet, parquetFile, read.text, sql, tables, tableNames, cacheTable, uncacheTable, clearCache, dropTempTable, read.df, loadDF, createExternalTable.
* registerTempTable 方法被createOrReplaceTempView所取代。
* dropTempTable 方法被dropTempView所取代。
* 以下函數不再需要sc參數：setJobGroup, clearJobGroup, cancelJobGroup

