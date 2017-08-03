# RestTemplate

## RestTemplate实现调用路由

```java
public static String restProxy(String url, String body, HttpMethod httpMethod) {
    String result = Consts.EMPTY;
    try {
        HttpHeaders headers = new HttpHeaders();
        MediaType type = MediaType.parseMediaType("application/json;charset=UTF-8");
        headers.setContentType(type);
        RestTemplate restTemplate = new RestTemplate();
        HttpEntity entity = new HttpEntity<String>(body, headers);
        ResponseEntity<String> responseEntity = restTemplate.exchange(url, httpMethod, entity, String.class);
        result = responseEntity.getBody();
    } catch (Exception ex) {
        ex.printStackTrace();
    }
    return result;
}
```

## 调用postForObject

```java
private void Send(List<LogEntity> logEntities) {

        if (logEntities != null && logEntities.size() > 0) {

            System.out.println("logEntities" + logEntities.size());

            RestTemplate restTemplate = new RestTemplate();

            HttpHeaders headers = new HttpHeaders();
            MediaType type = MediaType.parseMediaType("application/json; charset=UTF-8");
            headers.setContentType(type);

            var formEntity = new HttpEntity<String>(JsonUtils.serialize(logEntities), headers);

            String result = restTemplate.postForObject(logConfig.getDebugUrl(), formEntity, String.class);
        }
    }
```

## Spring 注入

```java
package hello;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class Application {

	private static final Logger log = LoggerFactory.getLogger(Application.class);

	public static void main(String args[]) {
		SpringApplication.run(Application.class);
	}

	@Bean
	public RestTemplate restTemplate(RestTemplateBuilder builder) {
		return builder.build();
	}

	@Bean
	public CommandLineRunner run(RestTemplate restTemplate) throws Exception {
		return args -> {
			Quote quote = restTemplate.getForObject(
					"http://gturnquist-quoters.cfapps.io/api/random", Quote.class);
			log.info(quote.toString());
		};
	}
}
```

* **RestTemplateBuilder ** 是通过Spring注入。