# 创建Bean

## 1. 注解方式

Ref：[http://blog.csdn.net/javaloveiphone/article/details/52182899](http://blog.csdn.net/javaloveiphone/article/details/52182899)

@Configuration与@Bean合用：

* @Configuration标注在类上，相当于把该类作为spring的xml配置文件中的，作用为：配置spring容器\(应用上下文\)。
* @Bean标注在方法上\(返回某个实例的方法\)，等价于spring的xml配置文件中的，作用为：注册bean对象

eg:

```java
@Configuration
public class TestConfiguration {
        public TestConfiguration(){
            System.out.println("spring容器启动初始化。。。");
        }

    //@Bean注解注册bean,同时可以指定初始化和销毁方法
    //@Bean(name="testNean",initMethod="start",destroyMethod="cleanUp")
    @Bean
    @Scope("prototype")
    public TestBean testBean() {
        return new TestBean();
    }
}

public class TestMain {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(TestConfiguration.class);
        //获取bean
        TestBean tb = context.getBean("testBean");
        tb.sayHello();
    }
}
```

Points： 1. @Bean注解在返回实例的方法上，如果未通过@Bean指定bean的名称，则默认与标注的方法名相同，如testBean； 2. @Bean注解默认作用域为单例singleton作用域，可通过**@Scope\("prototype"\)**设置为原型作用域； 3. 既然@Bean的作用是注册bean对象，那么完全可以使用@Component、@Controller、@Service、@Ripository等注解注册bean，当然需要配置@ComponentScan注解进行自动扫描。

## 2. XML方式

```markup
<beans>
  <bean name="product" class="springio.bean.Product" />
  <bean id="localDate" class="java.time.LocalDate" factory-method="now" />
  <bean id="executorService" class="java.util.concurrent.Executors" 
  factory-method="newCachedThreadPool" destroy-method="shutdown" />
  <bean name="p" class="springio.bean.Product">
    <constructor-arg name="name" value="sdfsdfsdddd" />
    <constructor-arg name="desc" value="sdfsdfsdddd" />
    <constructor-arg name="price" value="9.95" />
  </bean>
</beans>
```

```java
ApplicationContext context = new ClassPathXmlApplicationContext(new string[]{ "config1.xml" });

Product p = context.getBean("product",Product.class)
```

* name 
* id 
* factory-method
* ref
* constructor-arg

