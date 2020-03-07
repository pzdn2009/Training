# MR-map

Ref：https://blog.csdn.net/lzw2016/article/details/84779254

本质上是执行map函数。

# MapRunner

```java
public class MapRunner<K1, V1, K2, V2>
    implements MapRunnable<K1, V1, K2, V2> {
  
  private Mapper<K1, V1, K2, V2> mapper;
  private boolean incrProcCount;

  @SuppressWarnings("unchecked")
  public void configure(JobConf job) {
    //conf指定的实现类
    this.mapper = ReflectionUtils.newInstance(job.getMapperClass(), job);
    //increment processed counter only if skipping feature is enabled
    this.incrProcCount = SkipBadRecords.getMapperMaxSkipRecords(job)>0 && 
      SkipBadRecords.getAutoIncrMapperProcCount(job);
  }

  public void run(RecordReader<K1, V1> input, OutputCollector<K2, V2> output,
                  Reporter reporter)
    throws IOException {
    try {
      // allocate key & value instances that are re-used for all entries
      //把key和value读出来
      K1 key = input.createKey();
      V1 value = input.createValue();
      
      //迭代：真正的进行读取
      while (input.next(key, value)) {
        // map pair to output
        mapper.map(key, value, output, reporter);
        if(incrProcCount) {
          reporter.incrCounter(SkipBadRecords.COUNTER_GROUP, 
              SkipBadRecords.COUNTER_MAP_PROCESSED_RECORDS, 1);
        }
        //计算结果放回output
      }
    } finally {
      mapper.close();
    }
  }

  protected Mapper<K1, V1, K2, V2> getMapper() {
    return mapper;
  }
}
```
* input阶段的RecordReader；
* key，value
* 循环执行map函数

# Mapper接口

Maps a single input key/value pair into an intermediate（中间） key/value pair

```java
void map(K1 key, V1 value, OutputCollector<K2, V2> output, Reporter reporter)
  throws IOException;
```