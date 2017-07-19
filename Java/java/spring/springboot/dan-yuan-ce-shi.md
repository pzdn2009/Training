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

* @RunWith(SpringRunner.class) 
* @SpringBootTest  --尋找SpringBootApplication 的實例。

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