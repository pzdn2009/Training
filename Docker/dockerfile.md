#1. 基础指令

**FROM**
```
FROM <image>
FROM <image:tag>
FROM <image@digest>
```
- 必须是第一个指令，指定从哪个基镜像继承。
- FROM可以出现多次，用于创建多个镜像。
- tag和digest是可选的。tag默认为latest。

**MAINTAINER**
```
MAINTAINER <name>
```
设置镜像的作者

**RUN**

RUN指令有两种格式
```
RUN <command>，shell格式，command运行在shell中，
RUN [“executable”,”param1”,”param2”] exec格式。
```
**CMD**

CMD有三种格式
```
CMD command param1 param2 (shell form)
CMD ["executable","param1","param2"] (exec form, this is the preferred form)
CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
```

**LABEL**

LABEL定义镜像的元数据。
```
LABEL <key>=<value> <key>=<value> <key>=<value> ...
LABEL "com.example.vendor"="ACME Incorporated"
ABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```
最终的配置结果可以通过docker inspect中查看。

**EXPOSE**
```
EXPOSE <port> [<port>...]
```
暴露容器的端口，需要使用-P来访问。
