
* 加载和保存数据
* JSON
* Hive
* JDBC

# 1. 加载和保存数据

* `.read.load`
* `.write.save`
* `.read.format("json").load`
* `.write.format("parquet").save`
* csv
* direct_sql：`FROM parquet.`examples/src/`
* `.saveAsTable`——写入Hive
* `.write.bucketBy`
* `.write.partitionBy`
* sortBy
* `df.write.option("path", "/some/path").saveAsTable("t")`

```scala
private def runBasicDataSourceExample(spark: SparkSession): Unit = {                                                                        
    // $example on:generic_load_save_functions$                                                                                               
    val usersDF = spark.read.load("examples/src/main/resources/users.parquet")                                                                
    usersDF.select("name", "favorite_color").write.save("namesAndFavColors.parquet")                                                          
    // $example off:generic_load_save_functions$                                                                                              
    
    // $example on:manual_load_options$                                                                                                       
    val peopleDF = spark.read.format("json").load("examples/src/main/resources/people.json")                                                  
    peopleDF.select("name", "age").write.format("parquet").save("namesAndAges.parquet")                                                       
    // $example off:manual_load_options$                                                                                                      
    
    // $example on:manual_load_options_csv$                                                                                                   
    val peopleDFCsv = spark.read.format("csv")                                                                                                
      .option("sep", ";")                                                                                                                     
      .option("inferSchema", "true")                                                                                                          
      .option("header", "true")                                                                                                               
      .load("examples/src/main/resources/people.csv")                                                                                         
    // $example off:manual_load_options_csv$                                                                                                  
                                                                                                                                              
    // $example on:direct_sql$                                                                                                                
    val sqlDF = spark.sql("SELECT * FROM parquet.`examples/src/main/resources/users.parquet`")                                                
    // $example off:direct_sql$                                                                                                               
    
    // $example on:write_sorting_and_bucketing$                                                                                               
    peopleDF.write.bucketBy(42, "name").sortBy("age").saveAsTable("people_bucketed")                                                          
    // $example off:write_sorting_and_bucketing$                                                                                              
    
    // $example on:write_partitioning$                                                                                                        
    usersDF.write.partitionBy("favorite_color").format("parquet").save("namesPartByColor.parquet")                                            
    // $example off:write_partitioning$                                                                                                       

    // $example on:write_partition_and_bucket$                                                                                                
    usersDF                                                                                                                                   
      .write                                                                                                                                  
      .partitionBy("favorite_color")                                                                                                          
      .bucketBy(42, "name")                                                                                                                   
      .saveAsTable("users_partitioned_bucketed")                                                                                              
    // $example off:write_partition_and_bucket$                                                                                               
                                                                                                                                              
    spark.sql("DROP TABLE IF EXISTS people_bucketed")                                                                                         
    spark.sql("DROP TABLE IF EXISTS users_partitioned_bucketed")                                                                              
  }                                          
```

# 2. JSON

* `spark.read.json(path)`
* `spark.read.json(otherPeopleDataset)`

```scala
private def runJsonDatasetExample(spark: SparkSession): Unit = {                                                                            
    // $example on:json_dataset$                                                                                                              
    // Primitive types (Int, String, etc) and Product types (case classes) encoders are                                                       
    // supported by importing this when creating a Dataset.                                                                                   
    import spark.implicits._                                                                                                                  
                                                                                                                                              
    // A JSON dataset is pointed to by path.                                                                                                  
    // The path can be either a single text file or a directory storing text files                                                            
    val path = "examples/src/main/resources/people.json"                                                                                      
    val peopleDF = spark.read.json(path)                                                                                                      
                                                                                                                                              
    // The inferred schema can be visualized using the printSchema() method                                                                   
    peopleDF.printSchema()                                                                                                                    
    // root                                                                                                                                   
    //  |-- age: long (nullable = true)                                                                                                       
    //  |-- name: string (nullable = true)                                                                                                    
                                                                                                                                              
    // Creates a temporary view using the DataFrame                                                                                           
    peopleDF.createOrReplaceTempView("people")                                                                                                
                                                                                                                                              
    // SQL statements can be run by using the sql methods provided by spark                                                                   
    val teenagerNamesDF = spark.sql("SELECT name FROM people WHERE age BETWEEN 13 AND 19")                                                    
    teenagerNamesDF.show()                                                                                                                    
    // +------+                                                                                                                               
    // |  name|                                                                                                                               
    // +------+                                                                                                                               
    // |Justin|                                                                                                                               
    // +------+                                                                                                                               
                                                                                                                                              
    // Alternatively, a DataFrame can be created for a JSON dataset represented by                                                            
    // a Dataset[String] storing one JSON object per string                                                                                   
    val otherPeopleDataset = spark.createDataset(                                                                                             
      """{"name":"Yin","address":{"city":"Columbus","state":"Ohio"}}""" :: Nil)                                                               
    val otherPeople = spark.read.json(otherPeopleDataset)                                                                                     
    otherPeople.show()                                                                                                                        
    // +---------------+----+                                                                                                                 
    // |        address|name|                                                                                                                 
    // +---------------+----+                                                                                                                 
    // |[Columbus,Ohio]| Yin|                                                                                                                 
    // +---------------+----+                                                                                                                 
    // $example off:json_dataset$                                                                                                             
  }                                     
```

# 3. JDBC

```scala
// Note: JDBC loading and saving can be achieved via either the load/save or jdbc methods
// Loading data from a JDBC source
val jdbcDF = spark.read
  .format("jdbc")
  .option("url", "jdbc:postgresql:dbserver")
  .option("dbtable", "schema.tablename")
  .option("user", "username")
  .option("password", "password")
  .load()

val connectionProperties = new Properties()
connectionProperties.put("user", "username")
connectionProperties.put("password", "password")
val jdbcDF2 = spark.read
  .jdbc("jdbc:postgresql:dbserver", "schema.tablename", connectionProperties)
// Specifying the custom data types of the read schema
connectionProperties.put("customSchema", "id DECIMAL(38, 0), name STRING")
val jdbcDF3 = spark.read
  .jdbc("jdbc:postgresql:dbserver", "schema.tablename", connectionProperties)

// Saving data to a JDBC source
jdbcDF.write
  .format("jdbc")
  .option("url", "jdbc:postgresql:dbserver")
  .option("dbtable", "schema.tablename")
  .option("user", "username")
  .option("password", "password")
  .save()

jdbcDF2.write
  .jdbc("jdbc:postgresql:dbserver", "schema.tablename", connectionProperties)

// Specifying create table column data types on write
jdbcDF.write
  .option("createTableColumnTypes", "name CHAR(64), comments VARCHAR(1024)")
  .jdbc("jdbc:postgresql:dbserver", "schema.tablename", connectionProperties)
```