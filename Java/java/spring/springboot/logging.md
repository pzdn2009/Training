# logging

Slf4j（Simple Logging Façade For Java），是一个简单日志门面抽象框架，它本身只提供了日志Facade API和一个简单的日志类实现，一般常配合**Log4j**，**LogBack**，**JdkLog**使用。

LogBack和Log4j都是开源日记工具库，LogBack是Log4j的改良版本，比Log4j拥有更多的特性，同时也带来很大性能提升。

spring-boot默认支持logback，所以无需引用任何以来只需要，配置application.properties即可。

