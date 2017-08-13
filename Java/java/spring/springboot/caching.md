# Caching

## 1. 简介

### 1.1 Summary

Spring Framework提供透明的Cache实现。Spring Boot通过@EnableCaching自动配置缓存设施。

默认使用ConcurrentHashMap实现。

### 1. 2 CacheManager:
```java
package org.springframework.cache;

import java.util.Collection;

public interface CacheManager {
    Cache getCache(String var1);

    Collection<String> getCacheNames();
}
```
* 根据名称获取一个Cache；
* 获取所有Cache的名称。

继承层次：
![](/assets/java_spbt_CacheManager.PNG)

### 1.3 Cache

Cache：
```java
package org.springframework.cache;

import java.util.concurrent.Callable;

public interface Cache {
    String getName();

    Object getNativeCache();

    Cache.ValueWrapper get(Object var1);

    <T> T get(Object var1, Class<T> var2);

    <T> T get(Object var1, Callable<T> var2);

    void put(Object var1, Object var2);

    Cache.ValueWrapper putIfAbsent(Object var1, Object var2);

    void evict(Object var1);

    void clear();

    public static class ValueRetrievalException extends RuntimeException {
        private final Object key;

        public ValueRetrievalException(Object key, Callable<?> loader, Throwable ex) {
            super(String.format("Value for key '%s' could not be loaded using '%s'", key, loader), ex);
            this.key = key;
        }

        public Object getKey() {
            return this.key;
        }
    }

    public interface ValueWrapper {
        Object get();
    }
}
```
* 获取Cache名称；
* 读写操作。

继承层次：

![](/assets/java_spbt_Cache.PNG)

## 2. 示例

默认已经引入了spring-boot-starter-cache依赖。

```java
//1. Application应用
@EnableCaching

//2. 需要缓存的地方应用
@Cacheable("cachename")

//3. 应用其他注解和配置，见下文
```

## 3. 注解

```java
@CacheConfig(cacheNames = "users")
public interface UserRepository extends JpaRepository<User, Long> {

    @Cacheable
    User findByName(String name);

}
```

### 3.1 @CacheConfig

主要用于配置该类中会用到的一些共用的缓存配置。

EG：@CacheConfig(cacheNames = "users")：配置了该数据访问对象中返回的内容将存储于名为users的缓存对象中，我们也可以不使用该注解，直接通过@Cacheable自己配置缓存集的名字来定义。

### 3.2 @Cacheable

配置了findByName函数的返回值将被加入缓存。同时在查询时，会先从缓存中获取，若不存在才再发起对数据库的访问。

该注解主要有下面几个参数：
* value、cacheNames：两个等同的参数（cacheNames为Spring 4新增，作为value的别名），用于指定缓存存储的集合名。由于Spring 4中新增了@CacheConfig，因此在Spring 3中原本必须有的value属性，也成为非必需项了
* key：缓存对象存储在Map集合中的key值，非必需，**缺省按照函数的所有参数组合作为key值**，若自己配置需使用SpEL表达式，

 比如：@Cacheable(key = "#p0")：使用函数第一个参数作为缓存的key值，更多关于SpEL表达式的详细内容可参考官方文档
* condition：缓存对象的条件，非必需，也需使用SpEL表达式，只有满足表达式条件的内容才会被缓存，
 
 比如：@Cacheable(key = "#p0", condition = "#p0.length() < 3")，表示只有当第一个参数的长度小于3的时候才会被缓存，若做此配置上面的AAA用户就不会被缓存，读者可自行实验尝试。
* unless：另外一个缓存条件参数，非必需，需使用SpEL表达式。它不同于condition参数的地方在于它的判断时机，该条件是在函数被调用之后才做判断的，所以它可以通过对result进行判断。
* keyGenerator：用于指定key生成器，非必需。若需要指定一个自定义的key生成器，我们需要去实现org.springframework.cache.interceptor.KeyGenerator接口，并使用该参数来指定。需要注意的是：**该参数与key是互斥的**
* cacheManager：用于指定使用哪个缓存管理器，非必需。只有当有多个时才需要使用
* cacheResolver：用于指定使用那个缓存解析器，非必需。需通过org.springframework.cache.interceptor.CacheResolver接口来实现自己的缓存解析器，并用该参数指定。
除了这里用到的两个注解之外，还有下面几个核心注解：

### 3.3 @CachePut

配置于函数上，能够根据参数定义条件来进行缓存，

它与@Cacheable不同的是，它每次都会真是调用函数，所以主要用于**数据新增和修改操作**上。

它的参数与@Cacheable类似，具体功能可参考上面对@Cacheable参数的解析

### 3.4 @CacheEvict

配置于函数上，通常用在删除方法上，用来从缓存中移除相应数据。除了同@Cacheable一样的参数之外，它还有下面两个参数：
* allEntries：非必需，默认为false。当为true时，会移除所有数据
* beforeInvocation：非必需，默认为false，会在调用方法之后移除数据。当为true时，会在调用方法之前移除数据。