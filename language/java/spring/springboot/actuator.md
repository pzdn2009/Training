# actuator

生产环境的监控。

## 依赖

```markup
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

## 终结点

提供应用程序监控的路由。

配置application.yml，才能看到后面的各种指标

```text
management:
  security:
    enabled: false
```

### /beans

敏感。输出应用程序中的所有Bean。

```javascript
            {
                "bean": "swaggerApiListingReader",
                "aliases": [],
                "scope": "singleton",
                "type": "springfox.documentation.swagger.web.SwaggerApiListingReader",
                "resource": "URL [jar:file:/D:/apache-maven-3.5.0/localRepo/io/springfox/springfox-swagger-common/2.6.1/springfox-swagger-common-2.6.1.jar!/springfox/documentation/swagger/web/SwaggerApiListingReader.class]",
                "dependencies": []
            },
            {
                "bean": "myEndPoint",
                "aliases": [],
                "scope": "singleton",
                "type": "com.wingontravel.cms.common.MemInfoEndPoint",
                "resource": "class path resource [com/wingontravel/cms/common/EndPointAutoConfig.class]",
                "dependencies": []
            },
```

### /loggers

敏感。输出应用程序中的所有logger

```javascript
{
    "levels": [
        "OFF",
        "ERROR",
        "WARN",
        "INFO",
        "DEBUG",
        "TRACE"
    ],
    "loggers": {
        "ROOT": {
            "configuredLevel": "WARN",
            "effectiveLevel": "WARN"
        },
        "com": {
            "configuredLevel": null,
            "effectiveLevel": "WARN"
        },
        "com.***": {
            "configuredLevel": null,
            "effectiveLevel": "WARN"
        },
        "com.***.cms": {
            "configuredLevel": "INFO",
            "effectiveLevel": "INFO"
        },
        "com.***.cms.CmsWebapiApplication": {
            "configuredLevel": null,
            "effectiveLevel": "INFO"
        },
        "com.***.cms.common": {
            "configuredLevel": null,
            "effectiveLevel": "INFO"
        },
        "com.***.cms.common.GlobalDefaultExceptionHandler": {
            "configuredLevel": null,
            "effectiveLevel": "INFO"
        },
        "com.***.cms.common.WebLogAspect": {
            "configuredLevel": null,
            "effectiveLevel": "INFO"
        },
        "com.***.cms.controller": {
            "configuredLevel": null,
            "effectiveLevel": "INFO"
        },
        "io": {
            "configuredLevel": null,
            "effectiveLevel": "WARN"
        },
        "io.swagger": {
            "configuredLevel": null,
            "effectiveLevel": "WARN"
        },
    }
}
```

## Endpoint

包：org.springframework.boot.actuate.endpoint

```java
public interface Endpoint<T> {  
    String getId();  
    boolean isEnabled();  
    boolean isSensitive();  
    T invoke();  
}
```

Endpoint的加载还是要依靠spring.factories实现的。Spring-boot-actutor包下的META-IN/spring.factories配置了EndpointAutoConfiguration。

