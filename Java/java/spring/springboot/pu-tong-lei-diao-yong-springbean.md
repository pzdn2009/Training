# 普通类调用Spring Bean

将SpringUtil 放在可以扫描的包下：

```java
@Component //自动扫描会注入
public class SpringUtil implements ApplicationContextAware {

    private static ApplicationContext applicationContext = null;

    // 非@import显式注入，@Component是必须的，且该类必须与main同包或子包
    // 若非同包或子包，则需手动import 注入，有没有@Component都一样
    // 可复制到Test同包测试

    //获取applicationContext
    public static ApplicationContext getApplicationContext() {
        return applicationContext;
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        if (SpringUtil.applicationContext == null) {
            SpringUtil.applicationContext = applicationContext;
        }
        System.out.println("---------------com.ilex.jiutou.util.Test.Main.SubPackage.SpringUtil---------------");
    }

    //通过name获取 Bean.
    public static Object getBean(String name) {
        return getApplicationContext().getBean(name);

    }

    //通过class获取Bean.
    public static <T> T getBean(Class<T> clazz) {
        return getApplicationContext().getBean(clazz);
    }

    //通过name,以及Clazz返回指定的Bean
    public static <T> T getBean(String name, Class<T> clazz) {
        return getApplicationContext().getBean(name, clazz);
    }
}
```

Usage：
```java
var logConfig = SpringUtil.getApplicationContext().getBean(LogConfig.class);
```

其中LogConfig：
```java
@Data
@Configuration
@ConfigurationProperties(prefix = "log.to.es")
public class LogConfig {
    private Integer queueSize;
    private Integer maxMilliseconds;
    private String debugUrl;
    private String errorUrl;
    private Boolean isRunning;

    public String getUrlByLevel(Level level) {
        switch (level.levelStr) {
            case "DEBUG":
            case "INFO":
                return debugUrl;
            case "WARN":
            case "ERROR":
                return errorUrl;
            default:
                return "";
        }
    }
}
```