# Server

## server配置

```
server.address
指定server绑定的地址

server.compression.enabled
是否开启压缩，默认为false.

server.compression.excluded-user-agents
指定不压缩的user-agent，多个以逗号分隔，默认值
为:text/html,text/xml,text/plain,text/css

server.compression.mime-types
指定要压缩的MIME type，多个以逗号分隔.

server.compression.min-response-size
执行压缩的阈值，默认为2048

server.context-parameters.[param name]
设置servlet context 参数

server.context-path
设定应用的context-path.

server.display-name
设定应用的展示名称，默认: application

server.jsp-servlet.class-name
设定编译JSP用的servlet，默认: org.apache.jasper
.servlet.JspServlet)

server.jsp-servlet.init-parameters.[param name]
设置JSP servlet 初始化参数.

server.jsp-servlet.registered
设定JSP servlet是否注册到内嵌的servlet容器，默认true

server.port
设定http监听端口

server.servlet-path
设定dispatcher servlet的监听路径，默认为: /
```