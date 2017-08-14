# WebMvcConfigurer

Ref：http://blog.csdn.net/L_Sail/article/details/71436392。

# 1. 簡介

提供對WEB的一些全局配置。
實現了接口WebMvcConfigurer 。如下，提供自定義配置：

```java
@EnableWebMvc
@Configuration
public class WebConfig extends WebMvcConfigurerAdapter {
    //...
}
```

# 2. 詳解

## 1. @EnableWebMvc

开启MVC配置，相当于

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <mvc:annotation-driven/>

</beans>
```

* <mvc:annotation-driven>会自动注册RequestMappingHandlerMapping与RequestMappingHandlerAdapter两个Bean。

## 2. Conversion and Formatting

```java
@Override
    public void addFormatters(FormatterRegistry registry) {
        super.addFormatters(registry);
        registry.addFormatter(new StringDateFormatter());
        registry.addConverter(new StringToListConvert());
    }
```

* By default formatters for Number and Date types are installed。@NumberFormat和@DateTimeFormat。


## 3. 配置跨域支持

```java
@Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowCredentials(true)
                .allowedMethods("GET", "POST", "DELETE", "PUT")
                .maxAge(3600);
    }
```

詳細的配置：CorsRegistry字典。

## 4. 資源處理

```java
@Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/templates/**").addResourceLocations(ResourceUtils.CLASSPATH_URL_PREFIX + "/templates/");
        registry.addResourceHandler("/static/**").addResourceLocations(ResourceUtils.CLASSPATH_URL_PREFIX + "/static/");
        super.addResourceHandlers(registry);
    }
    
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/resources/**")
                .addResourceLocations("/public-resources/")
                .setCacheControl(CacheControl.maxAge(1, TimeUnit.HOURS).cachePublic());
    }
```

* 對指定的資源配置指定的路徑。
* 設置過期時間。

## 5. 添加攔截器

```java
@Override
    public void addInterceptors(InterceptorRegistry registry) {

        registry.addInterceptor(loginInterceptor()).addPathPatterns("/cms/resource/**");
        registry.addInterceptor(permissionInterceptor()).addPathPatterns("/cms/resource/**");
        registry.addInterceptor(headerResourceInterceptor()).addPathPatterns("/cms/resource/**");
    }
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LocaleInterceptor());
        registry.addInterceptor(new ThemeInterceptor()).addPathPatterns("/**").excludePathPatterns("/admin/**");
        registry.addInterceptor(new SecurityInterceptor()).addPathPatterns("/secure/*");
    }
```
* 依次為登錄，授權，資源頭部處理的攔截器。
* 排除路徑

## 6. view控制
```java
 @Override  
    public void addViewControllers(ViewControllerRegistry registry){  
        registry.addViewController("/login").setViewName("login");  
        registry.addViewController("/").setViewName("home");
    }   
```

## 7. 驗證器

```java
@Override
    public Validator getValidator(); {
        // return "global" validator
    }
```

## 8. 內容協商

```java
@Override
    public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
        configurer.mediaType("json", MediaType.APPLICATION_JSON);
    }
```
## 9. 視圖解析

```java
@Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        registry.enableContentNegotiation(new MappingJackson2JsonView());
        registry.jsp();
    }
    
    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        registry.enableContentNegotiation(new MappingJackson2JsonView());
        registry.freeMarker().cache(false);
    }

    @Bean
    public FreeMarkerConfigurer freeMarkerConfigurer() {
        FreeMarkerConfigurer configurer = new FreeMarkerConfigurer();
        configurer.setTemplateLoaderPath("/WEB-INF/");
        return configurer;
    }

```

