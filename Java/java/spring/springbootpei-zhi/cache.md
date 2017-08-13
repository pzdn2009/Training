# cache

```properties
# SPRING CACHE (CacheProperties)
spring.cache.cache-names= # Comma-separated list of cache names to create \
if supported by the underlying cache manager.

spring.cache.caffeine.spec= # The spec to use to create caches. \
Check CaffeineSpec for more details on the spec format.

spring.cache.couchbase.expiration=0 # Entry expiration in milliseconds.\
 By default the entries never expire.

spring.cache.ehcache.config= # The location of the configuration file \
to use to initialize EhCache.

spring.cache.guava.spec= # The spec to use to create caches.\
Check CacheBuilderSpec for more details on the spec format.

spring.cache.infinispan.config= # The location of the configuration file \
to use to initialize Infinispan.

spring.cache.jcache.config= # The location of the configuration file \
to use to initialize the cache manager.
spring.cache.jcache.provider= # Fully qualified name of the CachingProvider \
implementation to use to retrieve the JSR-107 compliant cache manager. \
Only needed if more than one JSR-107 implementation is available on the classpath.

spring.cache.type= # Cache type, auto-detected according \
to the environment by default.
```
