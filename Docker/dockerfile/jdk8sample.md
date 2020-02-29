# Jdk8Sample

## 启动应用

```docker
FROM openjdk:8-jre-alpine
MAINTAINER pzdn2009 1050244110@qq.com

ARG user=root
ARG http_port=8080

# 安装字体
RUN apk add --no-cache tzdata bash  ttf-dejavu fontconfig \
	&& fc-cache --force


ENV APP_HOME /app_home

VOLUME /app_home

WORKDIR /app_home

# 复制jar到容器里
COPY ./target/sigma-*.jar ${APP_HOME}/sigma.jar

USER ${user}

# command
ENTRYPOINT ["java", "-Duser.timezone=GMT+8", "-Xmx1024m", "-Xms512m", "-jar", "-server", "sigma.jar","--spring.profiles.active=dev"]

# CMD java -jar sigma.jar

# port
EXPOSE ${http_port}
```
