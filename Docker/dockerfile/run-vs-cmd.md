# RUN Vs CMD Vs ENTRYPOINT

* **RUN**：设置build容器时就运行的命令以及提交运行结果，RUN**经常用于安装软件包**。  dockerfile中可以写多条RUN指令

* **CMD**：设置容器启动时执行的命令，在**build时并不运行**，CMD能够被docker run后面跟的命令行参数替换。 dockerfile中只能写一条CMD指令，如果写了多条，那么只有最后一条生效。

* **ENTRYPOINT**：设置容器启动时执行的命令，ENTRYPOINT不能被docker run后面跟的命令行参数替换。dockerfile中只能写一条ENTRYPOINT指令，如果写了多条，那么只有最后一条生效。

我们可用两种方式指定 RUN、CMD 和 ENTRYPOINT 要运行的命令

```java
shell格式：  <instruction> <command>
exec格式 ：  <instruction> ["executable", "param1", "param2", ...]
```
* 当指令执行时，shell格式底层会调用`/bin/sh -c <command>`。
* 当指令执行时，exec格式会直接调用`<command>`，不会被 shell解析。

```Dockerfile
# -c会自动解析变量$name
CMD ["/bin/sh","-c","echo hello,$name"]  
```