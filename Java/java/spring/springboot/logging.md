# logging

Slf4j（Simple Logging Façade For Java），是一个简单日志门面抽象框架，它本身只提供了日志Facade API和一个简单的日志类实现，一般常配合**Log4j**，**LogBack**，**JdkLog**使用。

LogBack和Log4j都是开源日记工具库，LogBack是Log4j的改良版本，比Log4j拥有更多的特性，同时也带来很大性能提升。

spring-boot默认支持logback，所以无需引用任何以来只需要，配置application.properties即可。

## 自定義Appender的問題

Ref：http://www.itkeyword.com/doc/9602162384580714x181/spring-boot-logback-autowire

自定義的AppenderBase<ILoggingEvent>不能和Spring框架的Bean很好地結合起來。

要重新寫。

在单利里面不能使用SpringUtil，总之就是获取不到SpringApplicationContext

最后的解决方法：addAppender

```java
   public static void addAppender() {
        LoggerContext lc = (LoggerContext) LoggerFactory.getILoggerFactory();
        JoranConfigurator jc = new JoranConfigurator();
        jc.setContext(lc);

        Appender appender = new AppenderBase<ILoggingEvent>() {
            private String systemcode = "***";
            private String source = "XXXXX";

            @Override
            protected void append(ILoggingEvent event) {
                try {
                    var args = event.getArgumentArray();
                    if (args != null && args.length == 1) {

                        if (args[0] != null && args[0].toString().startsWith("detail:")) {

                            LogEntity logEntity = new LogEntity();
                            logEntity.setDetail(args[0].toString());
                            logEntity.setSource(source);
                            logEntity.setMessage(event.getMessage());
                            logEntity.setSystemCode(systemcode);

                            logEntity.setCreateTime(LocalDateTime.now().toString() + "+08:00");

                            InetAddress addr = InetAddress.getLocalHost();
                            logEntity.setMachineName(addr.getHostName());
                            logEntity.setIpAddress(addr.getHostAddress());

                            logEntity.setProcessID(Long.parseLong(ManagementFactory.getRuntimeMXBean().getName().split("@")[0]));
                            logEntity.setProcessName(ManagementFactory.getRuntimeMXBean().getName());

                            logEntity.setThreadID(Thread.currentThread().getId());

                            logEntity.setLevel(event.getLevel());

                            LogManager.getInstance().Enqueue(logEntity);
                        }
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        };
        ch.qos.logback.classic.Logger rootLogger = lc.getLogger(
                Logger.ROOT_LOGGER_NAME);

        rootLogger.addAppender(appender);

        appender.start();
    }
```

**原理**：日志框架中有一个工厂类：LoggerFactory，在这个类中可以获取到当前日志的上下文对象LoggerContext，通过LoggerContext可以获取到指定包的Logger ,通过Logger对象那就无所不能了。
```java
LoggerContext lc = (LoggerContext) LoggerFactory.getILoggerFactory();

ch.qos.logback.classic.Logger rootLogger = lc.getLogger(
                Logger.ROOT_LOGGER_NAME);

```

**场景**：动态修改日志对象以及内部。