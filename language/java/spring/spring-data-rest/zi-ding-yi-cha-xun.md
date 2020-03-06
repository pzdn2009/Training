# 自定義查詢

在Repository中定義：

```java
/**
     * 根据用户名称查找用户
     */
    @RestResource(path="name",rel="name")
    public User findByName(@Param("name") String name);

    //模糊查詢
    @RestResource(path="nameStartsWith",rel="nameStartsWith")
    public List<User> findByNameStartsWith(@Param("name") String name);

    //忽略大小寫
    @RestResource(path="nameStartsWith",rel="nameStartsWith")
    public List<User> findByNameStartsWithIgnoringCase(@Param("name") String name);

    //多條件查詢
    @RestResource(path="nameAndAge",rel="nameAndAge")
    public List<User> findByNameAndAge(@Param("name")String name ,@Param("age")int age);
```

[http://127.0.0.1:8080/user/search/name?name=pzdn](http://127.0.0.1:8080/user/search/name?name=pzdn)

Spring Data允许在方法名中使用四种动词：**get、read、find和count**。其中，动词get、read和find是同义的，这三个动词对应的Repository方法都会查询数据并返回对象。而动词count则会返回匹配对象的数量，而不是对象本身。

