# AOP

aop全称Aspect Oriented Programming。

本来spring就自带一套aop实现，我们直接使用此实现即可，本来使用aop还需要定义一些xml文件，但由于我们使用的是spring-boot框架，这一步就省略掉了。也就是说，在spring-boot中，我们可以直接使用aop而不需要任何的配置。

## 0. 理论

**切面（Aspect）**：一个关注点的模块化，这个关注点可能会横切多个对象。事务管理是J2EE应用中一个关于横切关注点的很好的例子。在Spring AOP中，切面可以使用基于模式或者基于@Aspect注解的方式来实现。

**连接点（Joinpoint）**：在程序执行过程中某个特定的点，比如某方法调用的时候或者处理异常的时候。在Spring AOP中，一个连接点总是表示一个方法的执行。

**通知（Advice）**：在切面的某个特定的连接点上执行的动作。其中包括了“around”、“before”和“after”等不同类型的通知（通知的类型将在后面部分进行讨论）。许多AOP框架（包括Spring）都是以拦截器做通知模型，并维护一个以连接点为中心的拦截器链。

**切入点（Pointcut）**：匹配连接点的断言。通知和一个切入点表达式关联，并在满足这个切入点的连接点上运行（例如，当执行某个特定名称的方法时）。切入点表达式如何和连接点匹配是AOP的核心：Spring缺省使用AspectJ切入点语法。

**引入（Introduction）**：用来给一个类型声明额外的方法或属性（也被称为连接类型声明（inter-type declaration））。Spring允许引入新的接口（以及一个对应的实现）到任何被代理的对象。例如，你可以使用引入来使一个bean实现IsModified接口，以便简化缓存机制。

**目标对象（Target Object）**：被一个或者多个切面所通知的对象。也被称做被通知（advised）对象。既然Spring AOP是通过运行时代理实现的，这个对象永远是一个被代理（proxied）对象。

**AOP代理（AOP Proxy）**：AOP框架创建的对象，用来实现切面契约（例如通知方法执行等等）。在Spring中，AOP代理可以是JDK动态代理或者CGLIB代理。

**织入（Weaving）**：把切面连接到其它的应用程序类型或者对象上，并创建一个被通知的对象。这些可以在编译时（例如使用AspectJ编译器），类加载时和运行时完成。Spring和其他纯Java AOP框架一样，在运行时完成织入。

## 1. 定义切入点

```java
    /*
     * 定义一个切入点
     */
    // @Pointcut("execution (* findById*(..))")
    @Pointcut("execution(* com.test.service.CacheDemoService.find*(..))")
    public void excudeService(){}
    
@Pointcut("execution(public * com.didispace.web..*.*(..))")
    public void webLog(){}
```

## 2. 定义Before
```java
    @Before("webLog()")
    public void doBefore(JoinPoint joinPoint) throws Throwable {
        startTime.set(System.currentTimeMillis());
        // 省略日志记录内容
    }
```
## 3. 定义AfterReturning
```java
    @AfterReturning(returning = "ret", pointcut = "webLog()")
    public void doAfterReturning(Object ret) throws Throwable {
        // 处理完请求，返回内容
        logger.info("RESPONSE : " + ret);
        logger.info("SPEND TIME : " + (System.currentTimeMillis() - startTime.get()));
    }
```

* 使用@Aspect注解将一个java类定义为切面类
* 使用@Pointcut定义一个切入点，可以是一个规则表达式，比如下例中某个package下的所有函数，也可以是一个注解等。
* 根据需要在切入点不同位置的切入内容
* 使用@Before在切入点开始处切入内容
* 使用@After在切入点结尾处切入内容
* 使用@AfterReturning在切入点return内容之后切入内容（可以用来对处理返回值做一些加工处理）
* 使用@Around在切入点前后切入内容，并自己控制何时执行切入点自身的内容
* 使用@AfterThrowing用来处理当切入内容部分抛出异常之后的处理逻辑

## 4. Springboot中使用

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

```java
package com.l3.sigma.web;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class ServiceMonitor {
    @AfterReturning("execution(* com.l3.sigma..*.*(..))")
    public void print(JoinPoint joinPoint){
        System.out.println("Completed: " + joinPoint);
    }
}
```
