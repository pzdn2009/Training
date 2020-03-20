* StructField——字段
* StructType——schema
* Row——行
* `createDataFrame(rowRDD, schema)`


```scala
private def runProgrammaticSchemaExample(spark: SparkSession): Unit = {                                                                     
    import spark.implicits._                                                                                                                  
    // $example on:programmatic_schema$                                                                                                       
    // Create an RDD                                                                                                                          
    val peopleRDD = spark.sparkContext.textFile("examples/src/main/resources/people.txt")                                                     
                                                                                                                                              
    // The schema is encoded in a string                                                                                                      
    val schemaString = "name age"                                                                                                             
                                                                                                                                              
    // Generate the schema based on the string of schema                                                                                      
    val fields = schemaString.split(" ")                                                                                                      
      .map(fieldName => StructField(fieldName, StringType, nullable = true))                                                                  
    val schema = StructType(fields)                                                                                                           
                                                                                                                                              
    // Convert records of the RDD (people) to Rows                                                                                            
    val rowRDD = peopleRDD                                                                                                                    
      .map(_.split(","))                                                                                                                      
      .map(attributes => Row(attributes(0), attributes(1).trim))                                                                              
                                                                                                                                              
    // Apply the schema to the RDD                                                                                                            
    val peopleDF = spark.createDataFrame(rowRDD, schema)                                                                                      
                                                                                                                                              
    // Creates a temporary view using the DataFrame                                                                                           
    peopleDF.createOrReplaceTempView("people")                                                                                                
                                                                                                                                              
    // SQL can be run over a temporary view created using DataFrames                                                                          
    val results = spark.sql("SELECT name FROM people")                                                                                        
                                                                                                                                              
    // The results of SQL queries are DataFrames and support all the normal RDD operations                                                    
    // The columns of a row in the result can be accessed by field index or by field name                                                     
    results.map(attributes => "Name: " + attributes(0)).show()                                                                                
    // +-------------+                                                                                                                        
    // |        value|                                                                                                                        
    // +-------------+                                                                                                                        
    // |Name: Michael|                                                                                                                        
    // |   Name: Andy|                                                                                                                        
    // | Name: Justin|                                                                                                                        
    // +-------------+                                                                                                                        
    // $example off:programmatic_schema$                                                                                                      
  }                    
```