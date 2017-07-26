# Circuit Breaker

教程Ref:https://spring.io/guides/gs/circuit-breaker/#initial

熔斷器模式：http://martinfowler.com/bliki/CircuitBreaker.html。

GIT源碼：git clone https://github.com/spring-guides/gs-circuit-breaker.git

## 1. 依賴
```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-hystrix</artifactId>
</dependency>

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

Netflix’s Hystrix庫提供了熔斷器的實現。

Hystrix watches for failing calls to that method, and if failures build up to a **threshold**, Hystrix opens the circuit so that subsequent calls automatically fail。

## 2. 示例

```java
package hello;

import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

import java.net.URI;

@Service
public class BookService {

  private final RestTemplate restTemplate;

  public BookService(RestTemplate rest) {
    this.restTemplate = rest;
  }

  @HystrixCommand(fallbackMethod = "reliable")
  public String readingList() {
    URI uri = URI.create("http://localhost:8090/recommended");

    return this.restTemplate.getForObject(uri, String.class);
  }

  public String reliable() {
    return "Cloud Native Java (O'Reilly)";
  }

}
```

* **@HystrixCommand**：以可以被Hystrix監控到的代理的方式連接到熔斷器。
* 只能在**@Component or @Service**中工作。
* 提供reliable做熔斷之後的補償處理。

```java
@EnableCircuitBreaker
@RestController
@SpringBootApplication
public class ReadingApplication {
   //....
}
```

使用**@EnableCircuitBreaker**來啟用熔斷。