# 使用EhCache设施

http://www.ehcache.org/

## 1. EhCache 简介

EhCache 是一个纯Java的进程内缓存框架，具有快速、精干等特点，是Hibernate中默认的CacheProvider。

Ehcache是一种广泛使用的开源Java分布式缓存。主要面向通用缓存,Java EE和轻量级容器。它具有内存和磁盘存储，缓存加载器,缓存扩展,缓存异常处理程序,一个gzip缓存servlet过滤器,支持REST和SOAP api等特点。

主要的特性有：
1. 快速
2. 简单
3. 多种缓存策略
4. 缓存数据有两级：**内存和磁盘**，因此无需担心容量问题
5. 缓存数据会在虚拟机重启的过程中写入磁盘
6. 可以通过RMI、可插入API等方式进行分布式缓存
7. 具有缓存和缓存管理器的侦听接口
8. 支持多缓存管理器实例，以及一个实例的多个缓存区域
9. 提供Hibernate的缓存实现

## 2. 示例

引入依赖：
```xml
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

使用配置文件：
```xml
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="ehcache.xsd">

    <cache name="users"
           maxEntriesLocalHeap="200"
           timeToLiveSeconds="600">
    </cache>

</ehcache>
```

默认位置为resource下，也可以手动指定：
```properties
spring.cache.ehcache.config=classpath:config/another-config.xml
```

## 3. ehcache.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd">

  <!-- 磁盘缓存位置 -->
  <diskStore path="java.io.tmpdir/ehcache"/>

  <!-- 默认缓存 -->
  <defaultCache
          maxEntriesLocalHeap="10000"
          eternal="false"
          timeToIdleSeconds="120"
          timeToLiveSeconds="120"
          maxEntriesLocalDisk="10000000"
          diskExpiryThreadIntervalSeconds="120"
          memoryStoreEvictionPolicy="LRU">
    <persistence strategy="localTempSwap"/>
  </defaultCache>

  <cache name="userCache"
         maxElementsInMemory="1000"
         eternal="false"
         timeToIdleSeconds="3"
         timeToLiveSeconds="3"
         maxEntriesLocalDisk="10000000"
         overflowToDisk="false"
         memoryStoreEvictionPolicy="LRU"/>
</ehcache>
```
ehcache.xml配置参数说明：

* name：缓存名称。
* maxElementsInMemory：缓存最大个数。
* eternal：缓存中对象是否为永久的，如果是，超时设置将被忽略，对象从不过期。
* timeToIdleSeconds：置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
* timeToLiveSeconds：缓存数据的生存时间（TTL），也就是一个元素从构建到消亡的最大时间间隔值，这只能在元素不是永久驻留时有效，如果该值是0就意味着元素可以停顿无穷长的时间。
* maxEntriesLocalDisk：当内存中对象数量达到maxElementsInMemory时，Ehcache将会对象写到磁盘中。
* overflowToDisk：内存不足时，是否启用磁盘缓存。
diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
* maxElementsOnDisk：硬盘最大缓存个数。
* diskPersistent：是否在VM重启时存储硬盘的缓存数据。默认值是false。
* diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
* memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
* clearOnFlush：内存数量最大时是否清除。
