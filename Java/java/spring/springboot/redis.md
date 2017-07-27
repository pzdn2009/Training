# Redis

spring-boot-starter-redis自动导入依赖。

Spring boot会自动配置RedisConnectionFactory, StringRedisTemplate 或者RedisTemplate。默认，这些实例会连接localhost:6379。 

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-data-redis</artifactId>  
</dependency>    
```

## 1. 2步快速入門

### 1.1 引入依賴

```properties
#redis 集群方式
spring.redis.cluster.nodes=172.18.21.1:6390,172.18.21.2:6391,172.18.21.3:6392
```

### 1.2 注入RedisTemplate

```java
@Autowired  
RedisTemplate<String, String> redisTemplate;  

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