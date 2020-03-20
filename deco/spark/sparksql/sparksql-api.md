* SparkSession
* DataFrame

# 示例代码分析

所有功能入口点。`SparkSession.builder`来创建。

examples/src/main/scala/org/apache/spark/examples/sql/SparkSQLExample.scala:

```scala                                                                                                                                          
package org.apache.spark.examples.sql                                                                                                         
                                                                                                                                              
import org.apache.spark.sql.Row                                                                                                               
// $example on:init_session$      
# 导入SparkSession包                                                                                                            
import org.apache.spark.sql.SparkSession                                                                                                      
// $example off:init_session$                                                                                                                 
// $example on:programmatic_schema$                                                                                                           
// $example on:data_types$                                                                                                                    
import org.apache.spark.sql.types._                                                                                                           
// $example off:data_types$                                                                                                                   
// $example off:programmatic_schema$                                                                                                          
                                                                                                                                              
// static
object SparkSQLExample {                                                                                                                      
                                                                                                                                              
  // $example on:create_ds$                                                                                                                   
  case class Person(name: String, age: Long)                                                                                                  
  // $example off:create_ds$                                                                                                                  
                                                                                                                                              
  def main(args: Array[String]) {                                                                                                             
    // $example on:init_session$     
    // builder模式构建入口                                                                                                         
    val spark = SparkSession                                                                                                                  
      .builder()                                                                                                                              
      .appName("Spark SQL basic example")                                                                                                     
      .config("spark.some.config.option", "some-value")                                                                                       
      .getOrCreate()                                                                                                                          
                                                                                                                                              
    // 支持隐式转换
    // For implicit conversions like converting RDDs to DataFrames                                                                            
    import spark.implicits._                                                                                                                  
    // $example off:init_session$                                                                                                             
    
    runBasicDataFrameExample(spark)                                                                                                           
    runDatasetCreationExample(spark)                                                                                                          
    runInferSchemaExample(spark)                                                                                                              
    runProgrammaticSchemaExample(spark)                                                                                                       
                                                                                                                                              
    spark.stop()                                                                                                                              
  }                                                                                                                                           
```

                                                                                                                                      


