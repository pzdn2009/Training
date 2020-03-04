# 定制化

```java
@Configuration
public class SpringDataRestConfig {
    @Bean
    public RepositoryRestConfigurer repositoryRestConfigurer() {
        return new RepositoryRestConfigurerAdapter() {
            @Override
            public void configureRepositoryRestConfiguration(RepositoryRestConfiguration config) {
                config.exposeIdsFor(MotoPaymentInfoEntity.class, MotoPaymentRequestEntity.class);
                //config.setDefaultMediaType(org.springframework.http.MediaType.APPLICATION_JSON);
                config.setDefaultPageSize(100);
            }
        };
    }
}
```

