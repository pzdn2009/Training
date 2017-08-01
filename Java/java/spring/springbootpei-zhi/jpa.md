# JPA 

```
spring.jpa.database
指定目标数据库.

spring.jpa.database-platform
指定目标数据库的类型.

spring.jpa.generate-ddl
是否在启动时初始化schema，默认为false

spring.jpa.hibernate.ddl-auto
指定DDL mode (none, validate, update, create, create-drop). 当使用内嵌数据库时，默认是create-drop，否则为none.

spring.jpa.hibernate.naming-strategy
指定命名策略.

spring.jpa.open-in-view
是否注册OpenEntityManagerInViewInterceptor，绑定JPA EntityManager到请求线程中，默认为: true

spring.jpa.properties
添加额外的属性到JPA provider.

spring.jpa.show-sql
是否开启sql的log，默认为: false
```