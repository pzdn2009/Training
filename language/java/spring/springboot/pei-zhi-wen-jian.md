# 配置文件

配置文件的讀取方式。

## 1. 默認

Spring Boot程序默认从application.properties或者application.yaml读取配置。

## 2. 命令行指定

SpringApplication会默认将命令行选项参数转换为配置信息，其優先級最高。

```text
java -jar myproject.jar --server.port = 9000
```

禁用命令行配置：

```java
SpringApplication.setAddCommandLineProperties(false).
```

## 3. 外置配置文件

Spring程序会按优先级从下面这些路径来加载application.properties配置文件

* 当前目录下的/config目录
* 当前目录
* classpath里的/config目录
* classpath 跟目录

因此，要外置配置文件就很简单了，在jar所在目录新建config文件夹，然后放入配置文件，或者直接放在配置文件在jar目录。

## 4. 自定义配置文件

使用外部配置文件：

```text
java -jar myproject.jar --spring.config.location=classpath:/default.properties,classpath:/override.properties

java -jar -Dspring.config.location=D:\config\config.properties springbootrestdemo-0.0.1-SNAPSHOT.jar
```

* spring.config.location
* Dspring.config.location

当然，还能在代码里指定

```java
@SpringBootApplication
@PropertySource(value={"file:config.properties"})
public class SpringbootrestdemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootrestdemoApplication.class, args);
    }
}
```

## 5. 指定文件

在application.properties中指定使用哪一个文件：

```text
spring.profiles.active = dev
```

命令行啟動的時候指定：

```text
java -jar myproject.jar --spring.profiles.active = prod
```

結合Jenkins的時候，可以指定配置文件。

