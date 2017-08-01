# Spring boot 詳解

spring-boot-start-web包含了spring webmvc和tomcat等web开发的特性。

spring-boot-maven-plugin插件的作用，在main方法運行。

@**SpringBootApplication**，等价于以默认属性使用@Configuration，@EnableAutoConfiguration和@ComponentScan。

@RestController返回json字符串的数据，直接可以编写RESTFul的接口；

運行：mvn spring-boot:run

Spring Boot也是引用了JSON解析包Jackson，可以在对象上使用Jackson提供的json属性的注解，对**时间进行格式化**，对一些**字段进行忽略**等等。

server.port=9090，可以修改默認的端口號。

Spring boot默认是/ ，这样直接通过http://ip:port/就可以访问到index页面，如果要修改为http://ip:port/path/ 访问的话，那么需要在application.properties文件中加入server.context-path = /你的path,比如：spring-boot,那么访问地址就是http://ip:port/cms 路径。

```properties
server.context-path=/cms
```

修改JDK版本：
```xml
<plugin>
   <artifactId>maven-compiler-plugin</artifactId>
   <configuration>
      <source>1.8</source>
      <target>1.8</target>
   </configuration>
</plugin>
```