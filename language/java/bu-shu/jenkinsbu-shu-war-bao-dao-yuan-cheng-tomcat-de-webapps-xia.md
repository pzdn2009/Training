# Jenkins部署war包到远程tomcat的webapps下

Ref：[http://blog.csdn.net/songjiaping/article/details/51454243](http://blog.csdn.net/songjiaping/article/details/51454243)

首先需要下载一个Jenkins的插件：Deploy to container Plugin 。 安装好后，在增加构建后操作步骤中会多出一个选项Deploy war/ear to a Container。

1. 加你一个Maven项目；
2. war包部署。

![](../../../.gitbook/assets/java_deploy_war.png)

* WAR/EAR files：输入war包的相对路径，如我的war包在新建目录的target下。EG：\*\*/cms-site.war。
* context path：输入用来访问tomcat的名称。默认什么也不填。
* add container：增加容器，一般选tomcat 7X就可以。这里的username与password需要到tomcat的conf文件夹中的tomcat-users.xml修改。tomcat URL就是你希望把war包部署到的tomcat所在IP地址，最后面不需要再加斜杠/。
* tomcat-users.xml中的用户名及密码默认是注释掉的，所以需要删除注释，也可以直接复制以下代码到&lt;/tomcat-users&gt;之前。如果只是删除注释的话好像部署不会成功，还需要增加**manager开头的三个role才可以**。

  ```markup
  <role rolename="tomcat"/>  
  <role rolename="role1"/>  
  <role rolename="manager-gui" />  
  <role rolename="manager-script" />  
  <role rolename="manager-status" />  
  <user username="tomcat" password="tomcat" roles="tomcat"/>  
  <user username="both" password="tomcat" roles="tomcat,role1"/>  
  <user username="role1" password="tomcat" roles="role1"/>  
  <user username="deploy" password="tomcat" roles="manager-gui,manager-script,manager-status" />
  ```

* “  The username you provided is not allowed to use the text-basedTomcat Manage”。  需要manager的三个权限。
* Tomcat的不同主机访问：Manager-UI：/webapps/manager/META-INF/context.xml。

  ```markup
  <Context antiResourceLocking="false" privileged="true" >
    <!--
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
    -->
  </Context>
  ```

或者

```markup
 <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
```

