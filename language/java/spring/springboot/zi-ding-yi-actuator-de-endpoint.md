# 自定义actuator的EndPoint

步骤： 1. 引入依赖 2. 定义EndPoint模型 3. 实现EndPoint 4. 加入自动配置 5. 访问测试

## 1. 引入依赖

```markup
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <artifactId>meminfo-endpoint</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>meminfo-endpoint</name>
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
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

* 重点是actuator。

## 2.3. 定义EndPoint模型 & 实现

定义内存信息：

```java
@Data
public class MemInfo {
    @JsonFormat(pattern="yyyy-MM-dd HH:mm:ss")
    private Date recordTime;//记录时间
    private long maxMemory;//能构从操作系统那里挖到的最大的内存，以字节为单位
    private long totalMemory;//进程当时所占用的所有 内存
}
```

实现接口：

```java
public class MemInfoEndPoint implements Endpoint<MemInfo> {
    @Override
    public String getId() {
        return "meminfo";
    }

    @Override
    public boolean isEnabled() {
        return true;
    }

    @Override
    public boolean isSensitive() {
        return false;
    }

    @Override
    public MemInfo invoke() {

        MemInfo memInfo = new MemInfo();
        Runtime runtime = Runtime.getRuntime();

        memInfo.setRecordTime(new Date());
        memInfo.setMaxMemory(runtime.maxMemory() / (1024 * 1024));
        memInfo.setTotalMemory(runtime.totalMemory() / (1024 * 1024));
        return memInfo;
    }
}
```

## 4. 注册bean

```java
@Configuration
public class EndPointAutoConfig {
    @Bean
    public MemInfoEndPoint myEndPoint() {
        return new MemInfoEndPoint();
    }
}
```

## 5. 访问

[http://localhost:8091/meminfo](http://localhost:8091/meminfo)

```text
{"recordTime":"2017-08-08 10:58:30","maxMemory":2686,"totalMemory":201}
```

