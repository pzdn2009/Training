# Lombok

## Lombok

Lombok是一个可以通过简单的注解形式来帮助我们简化消除一些必须有但显得很臃肿的Java代码的工具，通过使用对应的注解，可以在编译源码的时候生成对应的方法。官方地址：[https://projectlombok.org/，github地址：https://github.com/rzwitserloot/lombok。](https://projectlombok.org/，github地址：https://github.com/rzwitserloot/lombok。)

## 注解说明

* val 。Finally! Hassle-free final local variables.
* **@Getter / @Setter** 。可以作用在类上和属性上，放在类上，会对所有的非静态\(non-static\)属性生成Getter/Setter方法，放在属性上，会对该属性生成Getter/Setter方法。并可以指定Getter/Setter方法的访问级别。
* **EqualsAndHashCode**。默认情况下，会使用所有非瞬态\(non-transient\)和非静态\(non-static\)字段来生成equals和hascode方法，也可以指定具体使用哪些属性。
* **@ToString**。生成toString方法，默认情况下，会输出类名、所有属性，属性会按照顺序输出，以逗号分割。
* **@NoArgsConstructor, @RequiredArgsConstructor and @AllArgsConstructor**无参构造器、部分参数构造器、全参构造器，当我们需要重载多个构造器的时候，Lombok就无能为力了。
* **@Data**。@ToString, @EqualsAndHashCode, 所有属性的@Getter, 所有non-final属性的@Setter和@RequiredArgsConstructor的组合，通常情况下，我们使用这个注解就足够了。
* **@Value** : 用于注解final类。
* **@Builder** : 产生复杂的构建器api类
* **@Synchronized** : 同步方法安全的转化
* **@Getter\(lazy=true\)** :
* **@Log** : 支持各种logger对象，使用时用对应的注解，如：@Log4j
* **@NonNull**

  or: How I learned to stop worrying and love the NullPointerException.

```java
 import lombok.NonNull;

public class NonNullExample extends Something {
  private String name;

  public NonNullExample(@NonNull Person person) {
    super("Hello");
    this.name = person.getName();
  }
}

import lombok.NonNull;

public class NonNullExample extends Something {
  private String name;

  public NonNullExample(@NonNull Person person) {
    super("Hello");
    if (person == null) {
      throw new NullPointerException("person");
    }
    this.name = person.getName();
  }
}
```

### log

There are several choices available: **@CommonsLog** Creates private static final org.apache.commons.logging.Log log = org.apache.commons.logging.LogFactory.getLog\(LogExample.class\);

**@JBossLog** Creates private static final org.jboss.logging.Logger log = org.jboss.logging.Logger.getLogger\(LogExample.class\);

**@Log** Creates private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger\(LogExample.class.getName\(\)\);

**@Log4j** Creates private static final org.apache.log4j.Logger log = org.apache.log4j.Logger.getLogger\(LogExample.class\);

**@Log4j2** Creates private static final org.apache.logging.log4j.Logger log = org.apache.logging.log4j.LogManager.getLogger\(LogExample.class\);

**@Slf4j** Creates private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger\(LogExample.class\);

**@XSlf4j** Creates private static final org.slf4j.ext.XLogger log = org.slf4j.ext.XLoggerFactory.getXLogger\(LogExample.class\);

With Lombok

```java
import lombok.extern.java.Log;
import lombok.extern.slf4j.Slf4j;

@Log
public class LogExample {

  public static void main(String... args) {
    log.error("Something's wrong here");
  }
}

@Slf4j
public class LogExampleOther {

  public static void main(String... args) {
    log.error("Something else is wrong here");
  }
}

@CommonsLog(topic="CounterLog")
public class LogExampleCategory {

  public static void main(String... args) {
    log.error("Calling the 'CounterLog' with a message");
  }
}
```

原始java：

```java
public class LogExample {
  private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger(LogExample.class.getName());

  public static void main(String... args) {
    log.error("Something's wrong here");
  }
}

public class LogExampleOther {
  private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExampleOther.class);

  public static void main(String... args) {
    log.error("Something else is wrong here");
  }
}

public class LogExampleCategory {
  private static final org.apache.commons.logging.Log log = org.apache.commons.logging.LogFactory.getLog("CounterLog");

  public static void main(String... args) {
    log.error("Calling the 'CounterLog' with a message");
  }
}
```

