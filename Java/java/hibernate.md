# Hibernate

下載地址：http://hibernate.org/orm/downloads/
Docs：http://hibernate.org/orm/documentation/

Maven依賴：

```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>5.2.10.Final</version>
</dependency>

<!-- for JPA, use hibernate-entitymanager instead of hibernate-core -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-entitymanager</artifactId>
    <version>5.2.10.Final</version>
</dependency>

<!-- optional -->

<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-osgi</artifactId>
    <version>5.2.10.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-envers</artifactId>
    <version>5.2.10.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-c3p0</artifactId>
    <version>5.2.10.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-proxool</artifactId>
    <version>5.2.10.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-infinispan</artifactId>
    <version>5.2.10.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-ehcache</artifactId>
    <version>5.2.10.Final</version>
</dependency>
```