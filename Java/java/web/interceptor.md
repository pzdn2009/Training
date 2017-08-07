# HandlerInterceptor 

## HandlerInterceptor 接口

```java
public interface HandlerInterceptor {
    boolean preHandle(HttpServletRequest var1, HttpServletResponse var2, Object var3) throws Exception;

    void postHandle(HttpServletRequest var1, HttpServletResponse var2, Object var3, ModelAndView var4) throws Exception;

    void afterCompletion(HttpServletRequest var1, HttpServletResponse var2, Object var3, Exception var4) throws Exception;
}
```

* preHandle：返回false就中断执行了

## XML 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context-3.0.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <mvc:annotation-driven/>
    <!--配置拦截器, 多个拦截器,顺序执行 -->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/cms/resource/**"/>
            <bean class="com.interceptors.LoginInterceptor"/>
        </mvc:interceptor>
        <!-- 当设置多个拦截器时，先按顺序调用preHandle方法，然后逆序调用每个拦截器的postHandle和afterCompletion方法 -->
        <mvc:interceptor>
            <mvc:mapping path="/cms/resource/**"/>
            <bean class="com.interceptors.PermissionInterceptor"/>
        </mvc:interceptor>
        <!-- 匹配的是url路径， 如果不配置或/**,将拦截所有的Controller -->
        <mvc:interceptor>
            <mvc:mapping path="/cms/resource/**"/>
            <bean class="com.interceptors.HeaderResourceInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
    <mvc:annotation-driven/>
    <mvc:resources mapping="/statics/**" location="/statics/"/>
</beans>
```

## WebMvcConfigurerAdapter方式
```java
@Configuration
public class MyWebAppConfiguer extends WebMvcConfigurerAdapter {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyInterceptor1()).addPathPatterns("/**");
        registry.addInterceptor(new MyInterceptor2()).addPathPatterns("/**");
        super.addInterceptors(registry);
    }
}
```