# restapi

* 常用注解
* 错误处理
* 跨域
* 多语言

## 1. 常用注解

* @**RestController** = @Controller + @ResponseBody ，作用是能夠返回Json數據。
* @**RequestMapping**\("/greeting"\)。配置路由
* @**RequestParam**\(value="name", defaultValue="World"\)。指定请求参数，queryString
* @**GetMapping**\("/"\)。GET方法映射的路由
* @**PostMapping**。Post方法映射的路由
* @**Valid**。用来校验请求体
* @**RequestBody**。请求体，form。
* **RequestMethod**.POST 。请求方法
* @**ResponseEntity**。响应体。有StatusCode以及Body。
* @**PathVariable**：路径变量{id}

* @**Controller** 控制器（注入服务）
* @**Service** 服务（注入dao）
* @**Repository** dao（实现dao访问）
* @**Component** （**把普通pojo实例化到spring容器中，相当于配置文件中的**）

---
* @Autowired：自动导入依赖的bean
* @**PostConstruct**：用于在依赖关系注入完成之后需要执行的方法上，以执行任何初始化。
* @SpringBootApplication：
 * @ComponentScan：组件扫描
 * @Configuration：等同于spring的XML配置文件；使用Java代码可以检查类型安全。
 * @EnableAutoConfiguration：自动配置
* @JsonBackReference解决嵌套外链问题。
* @RepositoryRestResourcepublic配合spring-boot-starter-data-rest使用。
* @Import：用来导入其他配置类。
* @ImportResource：用来加载xml配置文件。
* @Value：注入Spring boot application.properties配置的属性的值。
* @Inject：等价于默认的@Autowired，只是没有required属性；
* @Bean:相当于XML中的,放在方法的上面，而不是类，意思是产生一个bean,并交给spring管理。
* @Qualifier：当有多个同一类型的Bean时，可以用@Qualifier(“name”)来指定。与@Autowired配合使用。
---
* @ControllerAdvice：包含@Component。可以被扫描到。统一处理异常。
* @ExceptionHandler（Exception.class）：用在方法上面表示遇到这个异常就执行以下方法。
* @RestControllerAdvice：包含ControllerAdvice.

## 2. 错误处理

* GlobalDefaultExceptionHandler

## 3. 跨域

* @CrossOrigin(origins = {"*","null"},allowCredentials="true")
* WebMvcConfigure
* Filter
* Spring security：http.cors() + WebMvcConfigurer。

```java
@Configuration
class CORSConfiguration implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*") 
                .allowedMethods("POST", "GET", "PUT", "OPTIONS", "DELETE")
                .maxAge(3600)
                .allowCredentials(true);
    }
}


@Component
public class CORSFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        HttpServletResponse res = (HttpServletResponse) response;
        res.addHeader("Access-Control-Allow-Credentials", "true");
        res.addHeader("Access-Control-Allow-Origin", "*");
        res.addHeader("Access-Control-Allow-Methods", "GET, POST, DELETE, PUT");
        res.addHeader("Access-Control-Allow-Headers", "Content-Type,X-CAF-Authorization-Token,sessionToken,X-TOKEN");
        if (((HttpServletRequest) request).getMethod().equals("OPTIONS")) {
            response.getWriter().println("ok");
            return;
        }
        chain.doFilter(request, response);
    }
    @Override
    public void destroy() {
    }
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }
}

```
## 4. 多语言

* BaseException
* 多语言资源文件
* 全局异常处理：BaseException
* 自定义异常与多语言文件绑定
* 数据库：`extends AbstractMessageSource implements ResourceLoaderAware`
