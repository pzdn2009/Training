# Spring Data Rest

## Spring Data Rest

[https://docs.spring.io/spring-data/rest/docs/current/reference/html/](https://docs.spring.io/spring-data/rest/docs/current/reference/html/)

Spring Data REST是基于Spring Data的repository之上，可以把 repository 自动输出为REST资源. 可以返回JSON格式或者HAL格式。

## 1. 依賴

Spring boot中引用：

```markup
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-rest-hal-browser</artifactId>
</dependency>
```

Spring mvc中引用：

```markup
<dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-rest-webmvc</artifactId>
            <version>2.5.6.RELEASE</version>
</dependency>
```

## 2. 定義Repository，暴露

```java
@RepositoryRestResource(path="user")
public interface UserRepository extends JpaRepository<User, Long>{  
}
```

## 3. 訪問

[http://localhost:8080/user](http://localhost:8080/user)

```javascript
{
  "_embedded": {
    "users": [
      {
        "name": "白鱼",
        "password": "123123",
        "age": 15,
        "sex": false,
        "_links": {
          "self": {
            "href": "http://127.0.0.1:8080/user/4"
          },
          "user": {
            "href": "http://127.0.0.1:8080/user/4"
          }
        }
      },
      {
        "name": "小白鱼",
        "password": "123123",
        "age": 25,
        "sex": false,
        "_links": {
          "self": {
            "href": "http://127.0.0.1:8080/user/5"
          },
          "user": {
            "href": "http://127.0.0.1:8080/user/5"
          }
        }
      },
      {
        "name": "小白",
        "password": "123123",
        "age": 25,
        "sex": false,
        "_links": {
          "self": {
            "href": "http://127.0.0.1:8080/user/6"
          },
          "user": {
            "href": "http://127.0.0.1:8080/user/6"
          }
        }
      },
      {
        "name": "小 鱼 ",
        "password": "123123",
        "age": 25,
        "sex": false,
        "_links": {
          "self": {
            "href": "http://127.0.0.1:8080/user/7"
          },
          "user": {
            "href": "http://127.0.0.1:8080/user/7"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://127.0.0.1:8080/user"
    },
    "profile": {
      "href": "http://127.0.0.1:8080/profile/user"
    }
  },
  "page": {
    "size": 20,
    "totalElements": 4,
    "totalPages": 1,
    "number": 0
  }
}
```

其他地址： GET\|PUT\|DELETE [http://127.0.0.1:8080/user/2](http://127.0.0.1:8080/user/2) GET [http://127.0.0.1:8080/user?page=1&size=3](http://127.0.0.1:8080/user?page=1&size=3) GET [http://127.0.0.1:8080/user?page=1&size=3&sort=age,desc](http://127.0.0.1:8080/user?page=1&size=3&sort=age,desc) POST [http://127.0.0.1:8080/user](http://127.0.0.1:8080/user) body:{jsonData}

