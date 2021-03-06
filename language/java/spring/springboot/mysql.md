# MySQL

## 1. 添加依賴

```markup
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
</dependency>
```

## 2. dataSource配置

```text
spring.datasource.url=jdbc:mysql://localhost:3306/db_example
spring.datasource.username=springuser
spring.datasource.password=ThePassword
spring.datasource.driverClassName=com.mysql.jdbc.Driver

#指定连接池中最大的活跃连接数.
spring.datasource.max-active=20
#指定连接池最大的空闲连接数量.
spring.datasource.max-idle=8
#指定连接池最小的空闲连接数量
spring.datasource.min-idle=8
# 指定启动连接池时，初始建立的连接数量
spring.datasource.initial-size=10
```

## 3. JPA配置

```text
#生成建議改為none。測試可以用create
spring.jpa.hibernate.ddl-auto=create
```

> spring.jpa.hibernate.ddl-auto 可選 **none, update, create, create-drop**, refer to the Hibernate documentation for details. • **none** This is the default for MySQL, no change to the database structure. • **update** Hibernate changes the database according to the given Entity structures. • **create** Creates the database every time, but don’t drop it when close. • **create-drop** Creates the database then drops it when the SessionFactory closes.

The default for H2 and other embedded databases is create-drop, but for others like MySQLis none

對於類似H2的嵌入式數據庫，默認的選項是create-drop，而MYSQL應為none。

## 4. 其他步驟

* 創建Entity
* 創建Repository
* 注入Repository
* 通過Repository來訪問DB。

