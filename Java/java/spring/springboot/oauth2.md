# OAuth2 

## dependency

```xml
<dependency>
            <groupId>org.springframework.security.oauth</groupId>
            <artifactId>spring-security-oauth2</artifactId>
</dependency>
```

## 術語

* Authorization Server，授權服務器
* Resource Server，資源服務器

## 代碼
provide 

```java
@Configuration
public class SecurityConfig {

    @Configuration
    @EnableResourceServer
    protected static class ResourceServer extends ResourceServerConfigurerAdapter {

        /**
         * Identifies this resource server.
         * Usefull if the AuthorisationServer authorises multiple Resource servers
         */
        private static final String RESOURCE_ID = "*****";

        @Autowired
        DataSource dataSource;

        @Override
        public void configure(HttpSecurity http) throws Exception {
            // @formatter:off
            http.authorizeRequests().anyRequest().authenticated();
            // @formatter:on
        }

        @Override
        public void configure(ResourceServerSecurityConfigurer resources) throws Exception {
            resources.resourceId(RESOURCE_ID);
            resources.tokenStore(tokenStore());
        }

        @Bean
        public TokenStore tokenStore() {
            return new JdbcTokenStore(dataSource);
        }
    }

    @Configuration
    @EnableAuthorizationServer
    protected static class AuthorizationServerConfiguration extends AuthorizationServerConfigurerAdapter {

        @Autowired
        DataSource dataSource;

        @Bean
        public TokenStore tokenStore() {
            return new JdbcTokenStore(dataSource);
        }

        @Override
        public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
            endpoints.tokenStore(tokenStore());
        }

        @Override
        public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
            clients.jdbc(dataSource);
        }
    }
}
```

```
curl -H "Accept: application/json" user:password@localhost:8888/oauth/token -d grant_type=client_credentials -d scope=trust,read,write

curl -H "Authorization: Bearer 3b237dbe-27c4-420d-8b6d-9cd075d4250d" localhost:8888/
```


