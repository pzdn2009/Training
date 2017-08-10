# with Jndi

JNDI（Java Naming and Directory Interface, Java命名和目录接口）.

使用代码配置：
```java
@Bean
public DataSource dataSource() 
{
  JndiDataSourceLookup dataSourceLookup = new JndiDataSourceLookup();
  DataSource dataSource = dataSourceLookup.getDataSource("java:comp/env/jdbc/YourDS");
  return dataSource;
}
```

使用properties配置：
```properties
spring.datasource.jndi-name=java:jboss/datasources/customers
```