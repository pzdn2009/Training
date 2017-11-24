# Date & LocalDateTime

Ref：
https://www.cnblogs.com/exmyth/p/6425878.html
http://blog.csdn.net/bangrenzhuce/article/details/52270232 ,（*****）

* Instant：瞬时实例。
* LocalDate：本地日期，不包含具体时间 
* LocalTime：本地时间，不包含日期。
* LocalDateTime：组合了日期和时间，但不包含时差和时区信息。
* ZonedDateTime：最完整的日期时间，包含时区和相对UTC或格林威治的时差。

Java 8中 java.util.Date 类新增了两个方法，分别是from(Instant instant)和toInstant()方法。

相互轉化：
```java
public class DateLocalDateTimeUtil {

    public static LocalDateTime UDateToLocalDate(Date date) {
        Instant instant = date.toInstant();
        ZoneId zone = ZoneId.systemDefault();
        return LocalDateTime.ofInstant(instant, zone);
    }

    public static Date LocalDateTimeToUdate(LocalDateTime localDateTime) {
        ZoneId zone = ZoneId.systemDefault();
        Instant instant = localDateTime.atZone(zone).toInstant();
        java.util.Date date = Date.from(instant);
        return date;
    }
}
```

```java
LocalTime localTime = localDateTime.toLocalTime();
LocalDate localDate = localDateTime.toLocalDate();
```


