# 單元測試

## 1. 依賴

Ref：https://spring.io/guides/gs/testing-web/ 

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>

<dependency>
    <groupId>org.assertj</groupId>
    <artifactId>assertj-core</artifactId>

    <!-- use 2.8.0 for Java 7 projects -->
    <version>3.8.0</version>
    <scope>test</scope>
</dependency>
```

## 2. 標記

最新版的用法：
* @RunWith(SpringRunner.class) 
* @SpringBootTest  --尋找SpringBootApplication 的實例。

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class MgpApplicationTests {
}
```

SpringBootTest.WebEnvironment：
* MOCK。MOCK —提供一个Mock的Servlet环境，内置的Servlet容器并没有真实的启动，主要搭配使用@AutoConfigureMockMvc
* RANDOM_PORT。提供一个真实的Servlet环境，也就是说会启动内置容器，然后使用的是随机端口 
* DEFINED_PORT。这个配置也是提供一个真实的Servlet环境，使用的默认的端口，如果没有配置就是8080 

## 3. 庫

AssertJ

```java
import static org.assertj.core.api.Assertions.assertThat;
import static org.hamcrest.Matchers.containsString;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
```

## 4. 測試點

### 4.1 控制器

控制器不為空



### 4.2 WEB 請求

* TestRestTemplate
* @LocalServerPort
* MockMvc 
* WebApplicationContext 
