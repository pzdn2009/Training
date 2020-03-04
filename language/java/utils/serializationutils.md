# SerializationUtils

以流的方式，序列化工具类，主要用于序列化操作，同时提供对象深拷贝克隆接口：

所在包

```java
package org.apache.commons.lang;

public static Object clone(Serializable object) {
    return deserialize(serialize(object));
}
```

