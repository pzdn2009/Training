# ADD Vs COPY

格式：

```text
ADD <src>... <dest>
COPY <src>... <dest>
```

## 区别

ADD指令可以让你使用URL作为`<src>`参数。

当遇到URL时候，可以通过URL下载文件并且复制到`<dest>`。

ADD的另外一个特性是有能力自动解压文件。如果`<src>`参数是一个可识别的压缩格式（tar, gzip, bzip2, etc）的本地文件（所以实现不了同时下载并解压），就会被解压到指定容器文件系统的路径`<dest>`。

```text
ADD http://foo.com/bar.go /tmp/
ADD /foo.tar.gz /tmp/
```

优先使用COPY。

实现下载一个压缩包，管道进入解压，编译

```text
RUN curl http://foo.com/package.tar.bz2 \
| tar -xjC /tmp/package \
&& make -C /tmp/package
```

