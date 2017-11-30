# Spring

## 簡介



![Spring Framework](http://docs.spring.io/spring/docs/5.0.0.BUILD-SNAPSHOT/spring-framework-reference/images/spring-overview.png.pagespeed.ce.XVe1noRCMt.png)

1. The **spring-core** and **spring-beans** modules provide the fundamental parts of the framework, including the IoC and Dependency Injection features. 

Spring特性：
IOC。控制反轉與依賴注入。
AOP。切面編程。
Bean。所有對象都受容器管理，都是一個bean。

GroupId|ArtifactId|Description
--|--|
org.springframework|spring-aop|	Proxy-based AOP support
org.springframework|spring-aspects|AspectJ based aspects
org.springframework|spring-beans|Beans support, including Groovy
org.springframework|spring-context|Application context runtime, including scheduling and remoting abstractions
org.springframework|spring-context-support|Support classes for integrating common third-party libraries into a Spring application context
org.springframework|spring-core|Core utilities, used by many other Spring modules
org.springframework|spring-expression|Spring Expression Language (SpEL)
org.springframework|spring-instrument|Instrumentation agent for JVM bootstrapping
org.springframework|spring-instrument-tomcat|Instrumentation agent for Tomcat
org.springframework|spring-jdbc|JDBC support package, including DataSource setup and JDBC access support
org.springframework|spring-jms	|JMS support package, including helper classes to send/receive JMS messages
org.springframework|spring-messaging|Support for messaging architectures and protocols
org.springframework|spring-orm|	Object/Relational Mapping, including JPA and Hibernate support
org.springframework|spring-oxm|	Object/XML Mapping
org.springframework|spring-test|Support for unit testing and integration testing Spring components
org.springframework|spring-tx|Transaction infrastructure, including DAO support and JCA integration
org.springframework|spring-web|Foundational web support, including web client and web-based remoting
org.springframework|spring-webmvc|HTTP-based Model-View-Controller and REST endpoints for Servlet stacks
org.springframework|spring-websocket|WebSocket and SockJS infrastructure, including STOMP messaging support


## 目錄
- Springboot
- Springcloud
- Springboot配置
- Spring MVC

## Spring 4.0 特性

* 泛型依賴注入
```java
private BaseRepository<M> repository;  
@Autowired  
private Repository<User> userRepository;
//key是bean名字；value就是所有实现了BaseService的Bean
@Autowired  
private Map<String, BaseService> map;  
```
* List注入
这样会注入所有实现了BaseService的Bean；但是顺序是不确定的，如果我们想要按照某个顺序获取；在Spring4中可以使用@Order或实现Ordered接口来实现
```java
@Autowired  
private List<BaseService> list;  
```
* @Lazy可以延迟依赖注入