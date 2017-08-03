# Redis

spring-boot-starter-redis自动导入依赖。

Spring boot会自动配置RedisConnectionFactory, StringRedisTemplate 或者RedisTemplate。默认，这些实例会连接localhost:6379。 

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-data-redis</artifactId>  
</dependency>    
```

## 1. 两步快速入門

### 1.1 引入依賴

```properties
#redis 集群方式
spring.redis.cluster.nodes=172.18.21.1:6390,172.18.21.2:6391,172.18.21.3:6392
```

### 1.2 注入RedisTemplate

```java
@Autowired  
RedisTemplate<String, String> redisTemplate;  

// 也可以注入StringRedisTemplate
//    private StringRedisTemplate redisTemplate;
@Test
public void redisTest() {  
    String key = "somekey";  
    String value = "pzdn2009";  
      
    ValueOperations<String, String> opsForValue = redisTemplate.opsForValue();  
      
    //数据插入测试：  
    opsForValue.set(key, value);  
    String valueFromRedis = opsForValue.get(key);  
    logger.info("redis value after set: {}", valueFromRedis);  
    assertThat(valueFromRedis, is(value));  
      
    //数据删除测试：  
    redisTemplate.delete(key);  
    valueFromRedis = opsForValue.get(key);  
    logger.info("redis value after delete: {}", valueFromRedis);  
    assertThat(valueFromRedis, equalTo(null));  
}  
```
说明：
1. 可以注入RedisTemplate<?, ?>，也可以注入StringRedisTemplate；
2. opsForValue()，返回对value操作的接口；
3. opsForList()，返回对List操作的接口ListOperations<K, V>；
4. 其他的类似。