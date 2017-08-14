# Freemarker

## 1. 依賴
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>demo</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.6.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-cache</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-freemarker</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>


</project>
```

## 2. freemarker模板

```properties
spring.freemarker.template-loader-path=classpath:/templates
spring.freemarker.cache=false
spring.freemarker.charset=UTF-8
spring.freemarker.enabled=true
spring.freemarker.suffix=.ftl

spring.thymeleaf.enabled=false
spring.groovy.template.enabled=false

server.port=9991
```

## 3. 控制器
```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@SpringBootApplication
@Controller
public class DemoApplication {

	@RequestMapping("/hello")
	public String hello(Map<String,Object> map){
		map.put("name", "pzdn2009");
		map.put("gender",1); //gender:性别，1：男；0：女；

		List<Map<String,Object>> friends =new ArrayList<Map<String,Object>>();

		Map<String,Object> friend = new HashMap<String,Object>();
		friend.put("name", "张三");
		friend.put("age", 20);
		friends.add(friend);

		friend = new HashMap<String,Object>();
		friend.put("name", "李四");
		friend.put("age", 22);
		friends.add(friend);

		map.put("friends", friends);

		return "hello";
	}

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```

## 4. ftl

resources/templates/下：

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title>Hello World!</title>
</head>
<body>
<p>
    welcome ${name} to freemarker!
</p>


<p>性别：
<#if gender==0>
    女
<#elseif gender==1>
    男
<#else>
    保密
</#if>
</p>


<h4>我的好友：</h4>
<#list friends as item>
姓名：${item.name} , 年龄${item.age}
<br>
</#list>

</body>
</html>
```