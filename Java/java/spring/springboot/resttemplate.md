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