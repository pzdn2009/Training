# Druid Usage

## 1. Dependency

```markup
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.5</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.tomcat</groupId>
                    <artifactId>tomcat-jdbc</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

## 2. 數據源配置

關鍵點：com.alibaba.druid.pool.DruidDataSource

```text
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.MySQL.jdbc.Driver
spring.datasource.url=jdbc:MySQL://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=123456

# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.filters=stat,wall,log4j
```

數據源的其他配置：

```text
# 下面为连接池的补充设置，应用到上面所有数据源中

# 初始化大小，最小，最大

spring.datasource.initialSize=5
spring.datasource.minIdle=5
spring.datasource.maxActive=20

# 配置获取连接等待超时的时间
spring.datasource.maxWait=60000

# 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
spring.datasource.timeBetweenEvictionRunsMillis=60000

# 配置一个连接在池中最小生存的时间，单位是毫秒
spring.datasource.minEvictableIdleTimeMillis=300000
spring.datasource.validationQuery=SELECT 1 FROM DUAL
spring.datasource.testWhileIdle=true
spring.datasource.testOnBorrow=false
spring.datasource.testOnReturn=false

# 打开PSCache，并且指定每个连接上PSCache的大小
spring.datasource.poolPreparedStatements=true
spring.datasource.maxPoolPreparedStatementPerConnectionSize=20

# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.filters=stat,wall,log4j

# 通过connectProperties属性来打开mergeSql功能；慢SQL记录
spring.datasource.connectionProperties=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000

# 合并多个DruidDataSource的监控数据
#spring.datasource.useGlobalDataSourceStat=true
```

## 3. 配置統計功能

```java
@Configuration

public class DruidConfiguration {
    /**
     * 注册一个StatViewServlet
     *
     * @return
     */

    @Bean
    public ServletRegistrationBean DruidStatViewServle2() {

        //org.springframework.boot.context.embedded.ServletRegistrationBean提供类的进行注册.
        ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        //添加初始化参数：initParams
        //白名单：
        servletRegistrationBean.addInitParameter("allow", "");
        //IP黑名单 (存在共同时，deny优先于allow) : 如果满足deny的话提示:Sorry, you are not permitted to view this page.
        servletRegistrationBean.addInitParameter("deny", "");
        //登录查看信息的账号密码.
        servletRegistrationBean.addInitParameter("loginUsername", "pzdn2009");
        servletRegistrationBean.addInitParameter("loginPassword", "daniu");
        //是否能够重置数据.
        servletRegistrationBean.addInitParameter("resetEnable", "false");
        return servletRegistrationBean;
    }

    /**
     * 注册一个：filterRegistrationBean
     *
     * @return
     */
    @Bean
    public FilterRegistrationBean druidStatFilter2() {

        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(new WebStatFilter());
        //添加过滤规则.
        filterRegistrationBean.addUrlPatterns("/*");
        //添加不需要忽略的格式信息.
        filterRegistrationBean.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
        return filterRegistrationBean;

    }
}
```

## 4. Visit

/druid/index.html [http://localhost:8888/druid/login.html](http://localhost:8888/druid/login.html)

## 5. Document

官方资料直达地址：

Druid 首页 [https://github.com/alibaba/druid/wiki/%E9%A6%96%E9%A1%B5](https://github.com/alibaba/druid/wiki/%E9%A6%96%E9%A1%B5)

Druid 常见问题 [https://github.com/alibaba/druid/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98](https://github.com/alibaba/druid/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

Druid 发布版 [https://github.com/alibaba/druid/releases](https://github.com/alibaba/druid/releases)

Druid 源码 [https://github.com/alibaba/druid](https://github.com/alibaba/druid)

