# jackson

json2javabean：http://www.atool.org/json2javabean.php
jsonformat:http://www.bejson.com/

## dependency

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.8.10</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.8.10</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.8.10</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.8.10</version>
</dependency>
```

## 序列化格式化輸出

使用writerWithDefaultPrettyPrinter

```java
private static ObjectMapper objectMapper = new ObjectMapper();

public static String serialize(Object object, boolean withPretty) {
        Writer write = new StringWriter();
        try {
            if (withPretty) {
                objectMapper.writerWithDefaultPrettyPrinter().writeValue(write, object);
            } else {
                objectMapper.writeValue(write, object);
            }
        } catch (Exception e) {
            log.error(e.getMessage(), SigmaConstant.WINGON.concat(e.getStackTrace().toString()));
        }
        return write.toString();
    }
```

## 反序列化

```java
public static <T> T deserialize(String json, TypeReference<T> typeRef) {
        Object object = null;
        try {
            return objectMapper.readValue(json, typeRef);
        } catch (Exception e) {
            log.error(e.getMessage(), SigmaConstant.WINGON.concat(e.getStackTrace().toString()));
        }
        return null;
    }
```

## 忽略未知屬性

```
objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
```

## 設置日誌格式

```java
@JsonFormat(pattern = "yyyy-MM-dd'T'HH:mm:ss.SSSZ", locale = "zh", timezone = "GMT+8")
private Date transactionTime;
```

* pattern 指定转化的格式，SSSZ(S指的是微秒，Z指时区)。此处的pattern和java.text.SimpleDateFormat中的Time Patterns一致
pattern示例：
 * "yyyy.MM.dd G 'at' HH:mm:ss z"    2001.07.04 AD at 12:08:56 PDT
 * "EEE, MMM d, ''yy"    Wed, Jul 4, '01
 * "h:mm a"    12:08 PM
 * "hh 'o''clock' a, zzzz"    12 o'clock PM, Pacific Daylight Time
 * "K:mm a, z"    0:08 PM, PDT
 * "yyyyy.MMMMM.dd GGG hh:mm aaa"    02001.July.04 AD 12:08 PM
 * "EEE, d MMM yyyy HH:mm:ss Z"    Wed, 4 Jul 2001 12:08:56 -0700
 * "yyMMddHHmmssZ"    010704120856-0700
 * "yyyy-MM-dd'T'HH:mm:ss.SSSZ"    2001-07-04T12:08:56.235-0700
 * "yyyy-MM-dd'T'HH:mm:ss.SSSXXX"    2001-07-04T12:08:56.235-07:00
 * "YYYY-'W'ww-u"    2001-W27-3
* locale，表示地區
* timezone，表示時區

Ref:
http://www.sojson.com/blog/245.html
http://www.jianshu.com/p/0ab28624c126