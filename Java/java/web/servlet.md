# Servlet

当使用Spring-Boot时，嵌入式Servlet容器通过扫描注解的方式注册Servlet、Filter和Servlet规范的所有监听器（如HttpSessionListener监听器）。 

Spring boot 的主Servlet 为 DispatcherServlet，其默认的url-pattern为“/”。

@WebServlet
