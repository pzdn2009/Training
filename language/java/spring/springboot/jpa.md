# JPA


## 1. 引用

官方文檔Ref：[http://docs.spring.io/spring-data/data-jpa/docs/current/reference/html/](http://docs.spring.io/spring-data/data-jpa/docs/current/reference/html/)

中文版指南：[https://ityouknow.gitbooks.io/spring-data-jpa-reference-documentation/content/](https://ityouknow.gitbooks.io/spring-data-jpa-reference-documentation/content/)

```xml
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

配置：
```properties
# 配置数据库
spring.jpa.database = sql_server
# 查询时是否显示日志
spring.jpa.show-sql = true
# Hibernate ddl auto (create, create-drop, update)
spring.jpa.hibernate.ddl-auto = none
# Naming strategy
spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
# stripped before adding them to the entity manager)
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.SQLServer2008Dialect
```

## 2. 注解

* @Entity。對應關係型DB表
* @Document。支持Mongo表
* @Id。對應主鍵
* @GeneratedValue(strategy=GenerationType.AUTO)。主键的产生策略。
* Identity：表自动增长字段，Oracle不支持这种方式；
 * AUTO：JPA自动选择合适的策略，是默认选项；
 * Sequence：通过序列产生主键，通过@SequenceGenerator注解指定序列名，Mysql不支持这种方式。
 * TABLE：通过表产生主键，框架借由表模拟产生主键，使用该策略可以使用更易于数据库的移植。
* @Lob。NVARCHAR(max)
* @Column。對應列名。
 * name
 * unique
 * nullable
 * inserttable：表示在ORM框架执行插入操作时,该字段是否应出现INSETRT语句中,默认为true
 * updateable：表示在ORM框架执行更新操作时,该字段是否应该出现在UPDATE语句中,默认为true.
 * columnDefinition
 * secondaryTable
* @Temporal。时间类型。
 * TemporalType.DATE
 * TemporalType.TIME
 * TemporalType.TIMESTAMP
* @Transient
* @Enumerated
* @Version
* @OneToOne
* @OneToMany
* @ManyToOne
* @ManyToMany
* @Formula 一个SQL表达式，这种属性是只读的,不在数据库生成属性(可以使用sum、average、max等)
* @OrderBy(name = "group_name ASC, name DESC")
* @JoinColumn(name = "ONE_ID", referencedColumnName = "ONE_ID")//设置对应数据表的列名和引用的数据表的列名 
* @MappedSuperclass
* @Embedded
* @Embeddable
* InheritanceType
 * SINGLE_TABLE：全部合併在一個表
 * TABLE_PER_CLASS：父子表獨立，屬性冗餘
 * JOINED：父子表獨立，通過外鍵


Ref:https://blog.csdn.net/dragonpeng2008/article/details/52297426

Ref：http://www.oracle.com/technetwork/cn/middleware/ias/toplink-jpa-annotations-100895-zhs.html

---

* @Query\(value = "select \* from t\_userinfo limit ?1", nativeQuery =true\)
* @Transactional

## 3. Repository

Spring Data JPA creates an implementation on the fly when you run the application.

* Repository
* CrudRepository
* PagingAndSortingRepository

### 3.1 CrudRepository

```java
package hello;
import java.util.List;
import org.springframework.data.repository.CrudRepository;

public interface CustomerRepository extends CrudRepository<Customer, Long> {
    List<Customer> findByLastName(String lastName);
}
```

默認方法：

```java
userRepository.findAll();
userRepository.findOne(1l);
userRepository.save(user);
userRepository.delete(user);
userRepository.count();
userRepository.exists(1l);
```

自定義簡單查詢：

findXXBy,readAXXBy,queryXXBy,countXXBy, getXXBy。

複雜查詢：

```java
Page<User> findByUserName(String userName,Pageable pageable);

@Test
public void testPageQuery() throws Exception {
    int page=1,size=10;
    Sort sort = new Sort(Direction.DESC, "id");
    Pageable pageable = new PageRequest(page, size, sort);
    userRepository.findALL(pageable);
    userRepository.findByUserName("testName", pageable);
}

@Transactional
@Modifying
@Query("delete from User where id = ?1")
void deleteByUserId(Long id);
```

### 3.2 PagingAndSortingRepository

PagingAndSortingRepository 接口继承于 CrudRepository 接口，拥有CrudRepository 接口的所有方法， 并新增两个方法：分页和排序。 但是**这两个方法不能包含筛选条件**。

```java
@NoRepositoryBean
public interface PagingAndSortingRepository<T, ID extends Serializable> extends CrudRepository<T, ID> {

    /**
     * Returns all entities sorted by the given options.
     * 
     * @param sort
     * @return all entities sorted by the given options
     */
    Iterable<T> findAll(Sort sort);

    /**
     * Returns a {@link Page} of entities meeting the paging restriction provided in the {@code Pageable} object.
     * 
     * @param pageable
     * @return a page of entities
     */
    Page<T> findAll(Pageable pageable);
}
```

Sample：

```java
@Test  
    public void test_findAll_page(){  
        int currentPage =0; //当前页从0 开始  
        int pageSize = 5;  

        //排序  
        Order idOrder = new Order(Direction.DESC, "id");  
        Order nameOrder = new Order(Direction.ASC,"name");  
        Sort sort = new Sort(idOrder,nameOrder);  
        PageRequest pageRequest  = new PageRequest(currentPage, pageSize, sort);  

        Page<StudentPO> page = this.studentdPageSortRepository.findAll(pageRequest);  
        System.out.println("总记录数:" + page.getTotalElements());  
        System.out.println("总页数:" + page.getTotalPages());  
        System.out.println("当前页（request):" + page.getNumber());  
        System.out.println("当前页总记录数（request):" + page.getSize());  
        System.out.println("当前页记录总数：" + page.getNumberOfElements());  
        List<StudentPO> students = page.getContent();  
        for (StudentPO studentPO : students) {  
            System.out.println(studentPO);  
        }  
    }
```

* 排序語法構造：Sort sort = new Sort\(Direction.DESC, "sort"\).and\(new Sort\(Direction.DESC, "id"\)\);

# 4. 高级

* 命名查询
* @Query
* 命名参数
* 集成层次
* @QueryHint
* @NamedEntityGraph
* Projection
* Specifications
* QueryByExample
* Auditing
