# Jersey

Ref :[http://www.jianshu.com/p/15c32cb52da1](http://www.jianshu.com/p/15c32cb52da1) jersey 是基于Java的一个轻量级RESTful风格的Web Services框架。

## GET & POST

```java
@GET
@Path("/thing")
public String get() {
    return "thing";
}

@POST
@Path("/add")
public Boolean add(@FormParam("name") String name) {
    // TODO save
    return true;
}
```

## Param

jersey中有几种常用的接收参数的注解：

**@PathParam** 接收链接中参数，如"/xxx/{name}/",@PathParm\("name"\) **@QueryParam** 接收链接中的普通参数，如"/xxx?name=ttt",@QueryParam\("name"\) **@FormParm** 接收post提交中的表单参数 **@FormDataParm** 上传文件接收文件参数

