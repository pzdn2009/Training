# Config

通过配置服务（Config Server）来为所有的环境和应用提供外部配置的集中管理。

GIT：git clone [https://github.com/spring-guides/gs-centralized-configuration.git](https://github.com/spring-guides/gs-centralized-configuration.git) GIT測試地址：[https://github.com/spring-cloud-samples/config-repo](https://github.com/spring-cloud-samples/config-repo)

Guard教程：[https://spring.io/guides/gs/centralized-configuration/\#initial](https://spring.io/guides/gs/centralized-configuration/#initial)

官方教程：[http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html)

## 1. 依賴SpringCloud

Config Server與Config Client都需要。

```markup
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Camden.SR5</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## 2. Config Server

```markup
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

```java
@EnableConfigServer
@SpringBootApplication
public class ConfigServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(ConfigServiceApplication.class, args);
    }
}
```

* **@EnableConfigServer**表明是配置服務器
* application.properties中，**spring.cloud.config.server.git.uri** = [https://github.com/spring-cloud-samples/config-repo，表示配置地址。](https://github.com/spring-cloud-samples/config-repo，表示配置地址。)
* 搜索的格式：

  ```text
  /{application}/{profile}[/{label}]
  /{application}-{profile}.yml
  /{label}/{application}-{profile}.yml
  /{application}-{profile}.properties
  /{label}/{application}-{profile}.properties
  ```

## 3. Config Client

```markup
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

* application.properties中，**spring.cloud.config.uri**: [http://myconfigserver.com，配置ConfigServer的地址。](http://myconfigserver.com，配置ConfigServer的地址。)

```java
@RefreshScope
@RestController
class MessageRestController {

    @Value("${foo:Hello default}")
    private String message;

    @RequestMapping("/message")
    String getMessage() {
        return this.message;
    }
}
```

* 使用**@RefreshScope**來從Server拉。一个Spring的@Bean在添加了@RefreshScope注解，可以解决Bean初始化的时候只能获得初始配置的问题。

