# 配置虚拟主机

## 基于主机名的虚拟主机配置

conf/server.xml中：
```xml
<Host appBase="D:\pzdnAnotherHost" autoDeploy="true" name="pzdn.sigma.com" unpackWARs="true">
    <Context path="" docBase="." debug="0" />
</Host>
```

* **path** ：是要重命名后的路径，用/代表根路径，例如/abc
* **reloadable**：为true时会自动更新，context指定的应用。
* **appBase**：是可以自动部署war的路径，默认是在tomcat的安装路径下的webapps，如果 用tomcat的默认的话使用相对路径，也可以使用绝对路径指定一个非tomcat默认的路径。
* **docBase**：与appBase没什么直接关系，它指出特定的应用的单独设置。如果war包在 appbase下，可以使用相对路径，比如在appBase路径下有，a.war,设置 docBase时可以用a来设置。常用使用绝对路径定义。
