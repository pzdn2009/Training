# Springboot生成War包

配置war和依赖tomcat
```xml
<packaging>war</packaging>
  
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency> 
```

继承SpringBootServletInitializer ：
```java
@SpringBootApplication  
public class SpringBootServlet extends SpringBootServletInitializer {  
  
    // jar启动  
    public static void main(String[] args) {  
        SpringApplication.run(SpringBootServlet.class, args);  
    }  
  
    // tomcat war启动  
    @Override  
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {  
        return application.sources(SpringBootServlet.class);  
    }  
  
}  
```
