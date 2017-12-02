# Linux部署jar包

例如：部署mpg.jar

将jar程序设置成后台运行，并且将标准输出的日志重定向至文件mpg.log

```
netstat -nlp | grep 8888 | awk '{print $7}' | awk -F/ '{print $1}' | kill
nohup java -jar mpg.jar >mpg.log 2>&1 &
```

## STDIN STDOUT STDERR
分別用0，1，2表示

& 表示全部1和2的信息

```
#表示将错误信息重定向至标准输出，并用less进行分页显示。
2>&1 |less
```