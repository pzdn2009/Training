# Servlet

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的**中间层**。

当使用Spring-Boot时，嵌入式Servlet容器通过扫描注解的方式注册Servlet、Filter和Servlet规范的所有监听器（如HttpSessionListener监听器）。 

Spring boot 的主Servlet 为 **DispatcherServlet**，其默认的url-pattern为“/”。

## Servlet接口

```java
package javax.servlet;
import java.io.IOException;

public interface Servlet {
    void init(ServletConfig var1) throws ServletException;

    ServletConfig getServletConfig();

    void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;

    String getServletInfo();

    void destroy();
}
```

* Servlet 通过调用 init () 方法进行初始化。 全局唯一一次
* Servlet 调用 service() 方法来处理客户端的请求。
* Servlet 通过调用 destroy() 方法终止（结束）。
* 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

## Web.xml

```xml
<web-app>      
    <servlet>
        <servlet-name>HelloWorld</servlet-name>
        <servlet-class>HelloWorld</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>HelloWorld</servlet-name>
        <url-pattern>/HelloWorld</url-pattern>
    </servlet-mapping>
</web-app>  
```
* Servlet需要有一个名称；
* 名称对应一个java类，用于反射创建示例；
* 名称对应着一个请求的基础路径

## @WebServlet

Servlet3.0新特性@WebServlet。

@WebServlet 用于将一个类声明为 Servlet，该注解将会在部署时被容器处理，容器将根据具体的属性配置将相应的类部署为 Servlet。
```java
public @interface WebServlet {
    String name() default "";

    String[] value() default {};

    String[] urlPatterns() default {};

    int loadOnStartup() default -1;

    WebInitParam[] initParams() default {};

    boolean asyncSupported() default false;

    String smallIcon() default "";

    String largeIcon() default "";

    String description() default "";

    String displayName() default "";
}
```

* name：相当于<servlet-name>
* value：等价于urlPatterns属性。两个属性不能同时使用。
* urlPatterns：相当于<url-pattern>
* loadOnStartup：加载顺序。<load-on-startup>
* initParams：初始化参数。@**WebInitParam**，<init-param>
* asyncSupported：是否支持异步操作模式。<async-supported>
* description：描述<description>
* displayName：显示名。<display-name>

