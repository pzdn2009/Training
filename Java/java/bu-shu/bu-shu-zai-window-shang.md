# 部署在Windows上

```bat
@echo off
tasklist /FI "WINDOWTITLE eq sth-webapi-boot" /FI "IMAGENAME eq java.exe"
start "sth-webapi-boot" java -jar -Dspring.profiles.active=prod -Dserver.port=8090 sth-webapi-boot.jar
```