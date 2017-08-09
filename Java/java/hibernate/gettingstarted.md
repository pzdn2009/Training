# GettingStarted

基於配置的流程

## 1. hibernate.cfg.xml 配置：DB & HB

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD//EN"
    "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
  <session-factory>
    <property name="connection.url">jdbc:sqlserver://172.18.28.119:1433;database=pzdnDB</property>
    <property name="connection.driver_class">com.microsoft.sqlserver.jdbc.SQLServerDriver</property>
    <mapping resource="com/l3/programming/ResourceApplicationEntity.hbm.xml"/>
    <mapping resource="com/l3/programming/ResourceCategoryEntity.hbm.xml"/>
    <mapping resource="com/l3/programming/ResourceItemEntity.hbm.xml"/>
    <mapping resource="ResApp.xml"/>
    <!-- <property name="connection.username"/> -->
    <!-- <property name="connection.password"/> -->

    <!-- DB schema will be updated if needed -->
    <!-- <property name="hbm2ddl.auto">update</property> -->
  </session-factory>
</hibernate-configuration>
```

## 2. Entity配置： Class & Table | Field & Column

ResourceCategoryEntity.hbm.xml
```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-mapping PUBLIC
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>

    <class name="com.l3.programming.ResourceCategoryEntity" table="ResourceCategory" schema="dbo" catalog="pzdnDB">
        <id name="categoryId" column="CategoryID"/>
        <property name="categoryName" column="CategoryName"/>
        <property name="categoryType" column="CategoryType"/>
        <many-to-one name="resourceApplicationByApplicationId" class="com.l3.programming.ResourceApplicationEntity">
            <column name="ApplicationID" not-null="true"/>
        </many-to-one>
        <set name="resourceItemsByCategoryId" inverse="true">
            <key>
                <column name="CategoryID" not-null="true"/>
            </key>
            <one-to-many not-found="ignore" class="com.l3.programming.ResourceItemEntity"/>
        </set>
        <set name="resourceMediaByCategoryId" inverse="true">
            <key>
                <column name="CategoryID" not-null="true"/>
            </key>
            <one-to-many not-found="ignore" class="com.l3.programming.ResourceMediaEntity"/>
        </set>
    </class>
</hibernate-mapping>
```

## 3. 代碼

ResourceCategoryEntity
```java
@Data
public class ResourceCategoryEntity {
    private int categoryId;
    private String categoryName;
    private int categoryType;
    private ResourceApplicationEntity resourceApplicationByApplicationId;
    private Collection<ResourceItemEntity> resourceItemsByCategoryId;
    private Collection<ResourceMediaEntity> resourceMediaByCategoryId;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        ResourceCategoryEntity that = (ResourceCategoryEntity) o;

        if (categoryId != that.categoryId) return false;
        if (categoryType != that.categoryType) return false;
        if (categoryName != null ? !categoryName.equals(that.categoryName) : that.categoryName != null) return false;

        return true;
    }

    @Override
    public int hashCode() {
        int result = categoryId;
        result = 31 * result + (categoryName != null ? categoryName.hashCode() : 0);
        result = 31 * result + categoryType;
        return result;
    }
}
```

## 4. 運行
```java
public class SigmaApplication {
    private SessionFactory sessionFactory;

    public static void main(String[] args) throws Exception {

        new SigmaApplication().testBasicUsage();
    }

    void setUp() throws Exception {
        // A SessionFactory is set up once for an application!
        final StandardServiceRegistry registry = new StandardServiceRegistryBuilder()
                .configure() // configures settings from hibernate.cfg.xml
                .build();
        try {
            sessionFactory = new MetadataSources(registry).buildMetadata().buildSessionFactory();
        } catch (Exception e) {
            // The registry would be destroyed by the SessionFactory, but we had trouble building the SessionFactory
            // so destroy it manually.
            StandardServiceRegistryBuilder.destroy(registry);
        }
    }

    @SuppressWarnings("unchecked")
    public void testBasicUsage() throws Exception {

        setUp();
        // create a couple of events...
        Session session = sessionFactory.openSession();

        // now lets pull events from the database and list them
        session = sessionFactory.openSession();
        session.beginTransaction();
        List result = session.createQuery("from ResourceCategoryEntity ").list();
        for (ResourceCategoryEntity event : (List<ResourceCategoryEntity>) result) {
            System.out.println("ResourceCategoryEntity (" + event.getCategoryName() + ") : " + event.getCategoryType());
        }
        session.getTransaction().commit();
        session.close();

        tearDown();
    }

    void tearDown() throws Exception {
        if (sessionFactory != null) {
            sessionFactory.close();
        }
    }
}
```

## 5. 總結

### 5.1 microsoft.sqlserver 驅動
```xml
<dependency>
			<groupId>com.microsoft.sqlserver</groupId>
			<artifactId>sqljdbc4</artifactId>
			<version>4.0</version>
</dependency>
```

### 5.2 GUID 要配置為String的

### 5.3 xml文件要放到運行目錄下

### 5.4 is開頭的字段要處理下

### 5.5 可以使用反向工程工具

View->Tool Windows->Persistence -> Generate Persistence Mapping ...
