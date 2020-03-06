# Dockerfile

**FROM**

```text
FROM <image>
FROM <image:tag>
FROM <image@digest>
```

* 必须是第一个指令，指定从哪个基镜像继承。
* FROM可以出现多次，用于创建多个镜像。
* tag和digest是可选的。tag默认为latest。

**MAINTAINER**

```text
MAINTAINER <name>
```

设置镜像的作者

**RUN**

RUN指令有两种格式

```text
RUN <command>，shell格式，command运行在shell中，
RUN [“executable”,”param1”,”param2”] exec格式。
```

**CMD**

CMD有三种格式

```text
CMD command param1 param2 (shell form)
CMD ["executable","param1","param2"] (exec form, this is the preferred form)
CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
```

**LABEL**

LABEL定义镜像的元数据。

```text
LABEL <key>=<value> <key>=<value> <key>=<value> ...
LABEL "com.example.vendor"="ACME Incorporated"
ABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```

最终的配置结果可以通过docker inspect中查看。

**EXPOSE**

```text
EXPOSE <port> [<port>...]
```

暴露容器的端口，需要使用-P来访问。

**ENV**

```text
ENV <key> <value>
ENV <key>=<value>
```

设置环境变量，在容器内保持。影响范围是容器域。

eg:

```text
ENV myName John Doe
ENV myDog Rex The Dog
```

**ADD**

```text
ADD <src>... <dest>
ADD ["<src>",... "<dest>"]
```

ADD指令，从指定的文件、目录、远程URL，复制到容器内的路径内。

eg:

```text
ADD hom* /mydir/ # adds all files starting with "hom"
ADD hom?.txt /mydir/ # ? is replaced with any single character, e.g., "home.txt"
ADD test relativeDir/ # adds "test" to `WORKDIR`/relativeDir/
ADD test /absoluteDir/ # adds "test" to /absoluteDir/
```

**COPY**

```text
COPY <src> … <dest>
```

复制本机的为容器中的。

当使用本地目录作为源目录是，建议使用COPY指令

**ENTRYPOINT**

```text
ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)
ENTRYPOINT command param1 param2 (shell form)
```

配置容器启动后执行的命令，并且不可被docker run指定的参数覆盖。

**VOLUME**

```text
VOLUME [“/data”]
```

创建一个挂载点。

**USER**

```text
USER daemon
```

指定用户名或者UID。

**WORKDIR**

```text
WORKDIR /path/to/workdir
```

为后续的指令配置工作目录

可以使用多次

```text
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```

则最终路径为/a/b/c

