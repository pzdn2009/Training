# Jenkins部署Springboot

```text
cd ***-webapi
set JAVA_HOME=C:\Program Files (x86)\Java\jdk1.8.0_112
call mvn clean package
```

复制文件：

```text
**/classes/config/*.*
***-webapi/target/classes

**/***-webapi*.jar
***-webapi/target

/usr/local/sys/run.sh
```

Shell： _\*_代表某系统的名称：

```bash
#!/bin/bash
echo "Stopping SpringBoot Application"
pid=`ps -ef | grep ***-webapi.jar | grep -v grep | awk '{print $2}'`
if [ -n "$pid" ]
then
   kill -9 $pid
fi

file="/usr/local/***/***-webapi.jar"
if [ -f "$file" ]
then
   mv /usr/local/***/***-webapi.jar /usr/local/***/backup/***-webapi.jar.`date +%Y%m%d%H%M%S`
fi

cp /root/published/***-webapi-*.jar  /usr/local/***/***-webapi.jar
\cp -rf /root/published/config  /usr/local/***/

echo "get privilege"
chmod 777 /usr/local/***/***-webapi.jar
nohup java -jar /usr/local/***/***-webapi.jar &
exit 0
```

Ref：[http://blog.csdn.net/u013066244/article/details/52788407](http://blog.csdn.net/u013066244/article/details/52788407)

