# 自定義配置

Spring Boot中使用自定义的properties。

步驟：

1. 在application.properties中添加配置项，
privilege.properties:
```properties
# user privilege
privilege.assistor=assitor
privilege.admin=assistor_create,star_operate,requirement_dispatch,fee_return,fee_charge,expiration_set
privilege.superman=admin_create
```
2. 定義配置類
```java
@ConfigurationProperties(prefix = "privilege", locations = "classpath:application.properties")
public class PrivilegeSettings {
}
```

3. 配置入口
```
@EnableConfigurationProperties
```

4. 使用
```java
@Autowired
private PrivilegeSettings privilegeSettings;
```