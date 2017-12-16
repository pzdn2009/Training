# 安全

hasRole：

```java
@PreAuthorize("hasRole('ROLE_USER')") 
public interface PreAuthorizedOrderRepository extends CrudRepository<Order, UUID> {

	@PreAuthorize("hasRole('ROLE_ADMIN')") 
	@Override
	void deleteById(UUID aLong);

	@PreAuthorize("hasRole('ROLE_ADMIN')")
	@Override
	void delete(Order order);

	@PreAuthorize("hasRole('ROLE_ADMIN')")
	@Override
	void deleteAll(Iterable<? extends Order> orders);

	@PreAuthorize("hasRole('ROLE_ADMIN')")
	@Override
	void deleteAll();
}
```

Secured:
```java
@Secured("ROLE_USER") 
@RepositoryRestResource(collectionResourceRel = "people", path = "people")
public interface SecuredPersonRepository extends CrudRepository<Person, UUID> {

	@Secured("ROLE_ADMIN") 
	@Override
	void deleteById(UUID aLong);

	@Secured("ROLE_ADMIN")
	@Override
	void delete(Person person);

	@Secured("ROLE_ADMIN")
	@Override
	void deleteAll(Iterable<? extends Person> persons);

	@Secured("ROLE_ADMIN")
	@Override
	void deleteAll();
}
```

方法級別的：
```java
@Configuration 
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled = true, prePostEnabled = true) 
public class SecurityConfiguration extends WebSecurityConfigurerAdapter { 
	...
}
```