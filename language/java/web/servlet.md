# Servlet

## Servlet

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的**中间层**。

当使用Spring-Boot时，嵌入式Servlet容器通过扫描注解的方式注册Servlet、Filter和Servlet规范的所有监听器（如HttpSessionListener监听器）。

Spring boot 的主Servlet 为 **DispatcherServlet**，其默认的url-pattern为“/”。

### Servlet接口

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

* Servlet 通过调用 init \(\) 方法进行初始化。 全局唯一一次
* Servlet 调用 service\(\) 方法来处理客户端的请求。
* Servlet 通过调用 destroy\(\) 方法终止（结束）。
* 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

### Web.xml

```markup
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

### @WebServlet

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

* name：相当于
* value：等价于urlPatterns属性。两个属性不能同时使用。
* urlPatterns：相当于
* loadOnStartup：加载顺序。
* initParams：初始化参数。@**WebInitParam**，
* asyncSupported：是否支持异步操作模式。
* description：描述
* displayName：显示名。

## HttpServletRequest

```java
public void printProperties(HttpServletRequest request) {

    //servletRequest中的方法

    //1. 获取请求体的编码方式
    String characterEncoding = request.getCharacterEncoding();
    println("getCharacterEncoding = " + characterEncoding);

    //2. get body length
    int contentLength = request.getContentLength();
    println("getContentLength = " + contentLength);

    //3. MIME type
    String mimeType = request.getContentType();
    println("getContentType = " + mimeType);

    //4. 接收请求的接口的 Internet Protocol (IP) 地址
    String ip = request.getLocalAddr();
    println("getLocalAddr = " + ip);

    //5. 基于 Accept-Language 头，返回客户端将用来接受内容的首选 Locale 客户端语言环境
    Locale locale = request.getLocale();
    println("getLocale = " + locale);

    //6. 所有的语言环境
    Enumeration<Locale> locales = request.getLocales();
    while(locales.hasMoreElements()){
        Locale temp = locales.nextElement();
        println("\n Locales = " + temp);
    }

    //7. 接收请求的 Internet Protocol (IP) 接口的主机名
    String localName = request.getLocalName();
    println("localName = " + localName);

    //8. 接收请求的接口的 Internet Protocol (IP) 端口号
    int localPort = request.getLocalPort();
    println("localPort = " + localPort);

    //9. 返回请求使用的协议的名称和版本
    String protocol = request.getProtocol();
    println("protocol = " + protocol);

    //10. 读取请求正文信息
    BufferedReader reader = request.getReader();
    println("getReader = " + reader.toString());

    //11. 发送请求的客户端
    String remoteAddr = request.getRemoteAddr();
    println("RemoteAddr = " + remoteAddr);

    //12. 发送请求的客户主机
    String remoteHost = request.getRemoteHost();
    println("RemoteHost = " + remoteHost);

    //13. 发送请求的客户主机端口
    int remotePort = request.getRemotePort();
    println("RemotePort = " + remotePort);

    //14. 返回用于发出此请求的方案名称，例如：http 、 https 、 ftp
    String scheme = request.getScheme();
    println("Scheme = " + scheme);

    //15. 返回请求被发送到的服务器的主机名。它是Host头值":"(如果有)之前的那部分的值。 或者解析服务器名称或服务器的IP地址
    String serverName = request.getServerName();
    println("ServerName = " + serverName);

    //16. 返回请求被发送到的端口。他是"Host"头值":" （如果有）之后的那部分的值，或者接受客户端连接的服务器端口。
    int serverPort = request.getServerPort();
    println("ServerPort = " + serverPort);

    //17. 返回一个boolean值，指示此请求是否是使用安全通道(比如HTTPS) 发出的。
    boolean secure = request.isSecure();
    println("isSecure = " + secure);

    //以上方法为 ServletRequest 接口提供的


    //以下方法为 HttpServletRequest 接口提供的

    /*
     * 18. 返回用于保护servlet的验证方法名称。 所有的servlet容器都支持
     * basic、 form和client certificate验证, 并且可能还支持digest验证
     */
    String authType = request.getAuthType();
    println("authType = " + authType);

    //19. getDateHeader ??
    request.getDateHeader("");

    //20. 返回请求头包含的所有头名称的枚举。
    Enumeration<String> headerNames = request.getHeaderNames();
    println("<hr/>");
    while(headerNames.hasMoreElements()){
        String name = headerNames.nextElement();
        println(" headerNmea = " + name + ";　　　getHeader = " + request.getHeader(name));
    }

    println("<hr/>");
    //21. 以int的形式返回指定请求头的值。 ???
    request.getIntHeader("123");

    //22. 返回与客户端发出此请求时发送的URL相关联的额外路径信息。
    String pathInfo = request.getPathInfo();
    println("PathInfo = " + pathInfo);

    //23. 返回包含在请求RUL中路径后面的查询字符串。如果没有查询字符串返回null
    String remoteUser = request.getRemoteUser();
    println("RemoteUser = " + remoteUser);

    //24. 返回客户端制定的回话ID
    String requestedSessionId = request.getRequestedSessionId();
    println("requestSessionId = " + requestedSessionId);

    //25. 返回请求调用servlet的URL部分
    String servletPath = request.getServletPath();
    println("servletPath = " + servletPath);

    //26. 返回与此请求关联的当前HttpSession，如果没有当前会话并且参数为true,则返回一个新会话。
    HttpSession session = request.getSession(true);
    println("getSession(true) = " + session);

    //27. 返回包含当前已经过验证的用户的名称的java.security.Principal对象。如果用户没有经过验证，则该方法返回null
    Principal userPrincipal = request.getUserPrincipal();
    println("userPrincipal = " + userPrincipal);

    //28. 检查会话的id是否作为Cook进入的
    boolean sessionIdFromCookie = request.isRequestedSessionIdFromCookie();
    println("sessionIdFromCookie = " + sessionIdFromCookie);

    //29. 检查请求的会话ID是否作为请求的URL的一部分进入的
    boolean sessionIdFromURL = request.isRequestedSessionIdFromURL();
    println("sessionIdFormURL = " + sessionIdFromURL);
}

public void println(Object obj){
    System.out.println(obj);
}
```

輸入結果 ：

```text
1. getCharacterEncoding = utf-8
2. getContentLength = -1
3. getContentType = null
4. getLocalAddr = 127.0.0.1
5. getLocale = zh_CN
6. Locales = zh_CN
   Locales = zh
   Locales = en_US
   Locales = en
7. localName = lm.licenses.adobe.com
8. localPort = 8080
9. protocol = HTTP/1.1
10. getReader = org.apache.catalina.connector.CoyoteReader@17b8d3d
11. RemoteAddr = 127.0.0.1
12. RemoteHost = 127.0.0.1
13. RemotePort = 57814
14. Scheme = http
15. ServerName = localhost
16. ServerPort = 8080
17. isSecure = false
18. authType = null
19. 
20. headerNmea = host;　　　getHeader = localhost:8080
    headerNmea = user-agent;　　　getHeader = Mozilla/5.0 (Windows NT 6.1; rv:32.0) Gecko/20100101 Firefox/32.0
    headerNmea = accept;　　　getHeader =     text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
headerNmea = accept-language;　　　getHeader = zh-cn,zh;q=0.8,en-us;q=0.5,en;q=0.3
headerNmea = accept-encoding;　　　getHeader = gzip, deflate
headerNmea = cookie;　　　getHeader = JSESSIONID=30256CEB48E2BF6050BF6E122635EAC4
headerNmea = connection;　　　getHeader = keep-alive
21. 
22. PathInfo = null
23. RemoteUser = null
24. requestSessionId = 30256CEB48E2BF6050BF6E122635EAC4
25. servletPath = /req
26. getSession(true) = org.apache.catalina.session.StandardSessionFacade@1fcf1ba
27. userPrincipal = null
28. sessionIdFromCookie = true
29. sessionIdFormURL = false
```

Ref:[http://blog.csdn.net/ranmudaofa/article/details/39785785](http://blog.csdn.net/ranmudaofa/article/details/39785785)

