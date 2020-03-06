# 自定義配置

Spring Boot中使用自定义的properties。

步驟：

1. 在application.properties中添加配置项，

   privilege.properties:

   ```text
   # user privilege
   privilege.assistor=assitor
   privilege.admin=assistor_create,star_operate,requirement_dispatch,fee_return,fee_charge,expiration_set
   privilege.superman=admin_create
   # 嵌套配置
   path.rightsmanagement.login=http://offline.rm.com/Login
   path.rightsmanagement.webapi=http://offlinerm.com/rmwebapi/
   path.rightsmanagement.noPermission=http://offline.rm/NoPermission.aspx
   path.cms.webapi=http://localhost:8091/api/cms/
   ```

2. 定義配置類

   ```java
   //簡單配置
   @ConfigurationProperties(prefix = "privilege", locations = "classpath:application.properties")
   public class PrivilegeSettings {
   }
   //
   //嵌套配置示例
   @Configuration
   @ConfigurationProperties(prefix = "path")
   @Data
   public class PathConfig {

    private RightsManagement rightsmanagement;
    private Cms cms;

    @Getter
    @Setter
    public static class RightsManagement {
        private String login;
        private String webapi;
        private String noPermission;
    }

    @Getter
    @Setter
    public static class Cms {
        private String webapi;
    }
   }
   ```

3. 配置入口

   ```text
   @EnableConfigurationProperties
   ```

4. 使用

   ```java
   @Autowired
   private PrivilegeSettings privilegeSettings;
   //使用
   @Autowired
   private PathConfig pathConfig;
   ```

