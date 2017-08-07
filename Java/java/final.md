# Final

## 獲取機器信息

```java
    public static void main(String[] args) throws Exception {
         InetAddress addr = InetAddress.getLocalHost();  
         String ip=addr.getHostAddress().toString(); //获取本机ip  
         String hostName=addr.getHostName().toString(); //获取本机计算机名称  
         System.out.println(ip);
         System.out.println(hostName);
    }
```

## 當前線程

```java
Thread.currentThread();
```

## Json首字母大寫

```java
@JsonProperty("CreateTime")
private LocalDateTime createTime;
```

## spring boot常用模板引擎

thymeleaf,freemarker,使用步骤:

1) 在pom.xml文件中添加thymeleaf或freemarker依赖
2) 在application.properties文件中添加thymeleaf或freemarker配置
3) 在src/main/resources下新建templates文件夹，存放thymeleaf(xx.html)或freemarker(xx.ftl)模板
4) 编写controller类（注意，使用@Controller注解，而不是@RestController注解）
5) 编写启动类(App.Java)
6) 访问服务进行测试

## 不需要重启加载模板

```xml
spring.thymeleaf.cache=false
```