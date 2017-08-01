# Run in Docker

官方教程：https://spring.io/guides/gs/spring-boot-docker/

Dockerfile：

```docker
FROM frolvlad/alpine-oraclejdk8:slim
VOLUME /tmp
ADD target/gs-spring-boot-docker-0.1.0.jar app.jar
ENV JAVA_OPTS=""
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar" ]
```

* ADD 复制到容器内
* ENV 设置环境变量
* ENTRYPOINT ，启动容器之后执行的命令
* egd：http://hongjiang.info/jvm-random-and-entropy-source/
* VOLUME 表示挂载点

## Build Dockerfile with Maven

```xml
<properties>
   <docker.image.prefix>springio</docker.image.prefix>
</properties>
<build>
    <plugins>
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>dockerfile-maven-plugin</artifactId>
            <version>1.3.4</version>
            <configuration>
                <repository>${docker.image.prefix}/${project.artifactId}</repository>
            </configuration>
        </plugin>
    </plugins>
</build>
```

构建：
```
$ ./mvnw install dockerfile:build
```

