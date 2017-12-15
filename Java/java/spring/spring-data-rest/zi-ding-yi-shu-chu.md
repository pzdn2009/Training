# 自定義輸出

## 1. 隱藏某個字段

使用默認的@JsonIgnore來注釋

## 2. @Projection
定義一個投影：
```java
@Projection(name = "motoPaymentInfoProjection", types = {MotoPaymentInfoEntity.class})
public interface MotoPaymentInfoProjection {
    String getInvoiceNo();

    String MerchantId();
    
    @Value("#{target.invoiceNo} #{target.merchantId}")
    String getFullInfo();
}
```

使用：
```java
@RepositoryRestResource(collectionResourceRel = "motoPaymentInfo", path = "/motoPaymentInfo",
        excerptProjection=MotoPaymentInfoProjection.class)
```

## 3. 屏蔽方法

```java
@RepositoryRestResource(path="user",excerptProjection=ListUser.class)
public interface UserRepository extends JpaRepository<User, Long>{

    @RestResource(exported = false)
    @Override
    public void delete(Long id);
}
```

## 4. RepositoryRestMvcConfiguration

暴露ID字段：

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