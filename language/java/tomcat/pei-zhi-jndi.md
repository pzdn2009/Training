# 配置Jndi

JNDI\(Java Naming and Directory Interface\)是一个标准规范，J2EE 规范要求所有 J2EE 容器都要提供 JNDI 规范的实现，因此Tomcat就实现了JNDI 规范。

* web.xml
* context.xml
* server.xml （全局的）

## db配置格式

```markup
<!--
  |- name：表示以后要查找的名称。通过此名称可以找到DataSource，此名称任意更换，但是程序中最终要查找的就是此名称，
           为了不与其他的名称混淆，所以使用jdbc/oracle，现在配置的是一个jdbc的关于oracle的命名服务。
  |- auth：由容器进行授权及管理，指的用户名和密码是否可以在容器上生效
  |- type：此名称所代表的类型，现在为javax.sql.DataSource
  |- maxActive：表示一个数据库在此服务器上所能打开的最大连接数
  |- maxIdle：表示一个数据库在此服务器上维持的最小连接数
  |- maxWait：最大等待时间。10000毫秒
  |- username：数据库连接的用户名
  |- password：数据库连接的密码
  |- driverClassName：数据库连接的驱动程序
  |- url：数据库连接的地址
-->

<!--配置Oracle数据库的JNDI数据源-->
<Resource 
        name="jdbc/oracle"
        auth="Container" 
        type="javax.sql.DataSource"
        maxActive="100" 
        maxIdle="30" 
        maxWait="10000"
        username="lead_oams" 
        password="p"
        driverClassName="oracle.jdbc.driver.OracleDriver"
        url="jdbc:oracle:thin:@192.168.1.229:1521:lead"/>

<!--配置MySQL数据库的JNDI数据源-->
<Resource 
        name="jdbc/mysql"
        auth="Container" 
        type="javax.sql.DataSource"
        maxActive="100" 
        maxIdle="30" 
        maxWait="10000"
        username="root" 
        password="root"
        driverClassName="com.mysql.jdbc.Driver"
        url="jdbc:mysql://192.168.1.144:3306/leadtest?useUnicode=true&amp;characterEncoding=utf-8"/>

<!--配置SQLServer数据库的JNDI数据源-->
<Resource 
        name="jdbc/sqlserver"
        auth="Container" 
        type="javax.sql.DataSource"
        maxActive="100" 
        maxIdle="30" 
        maxWait="10000"
        username="sa" 
        password="p@ssw0rd"
        driverClassName="com.microsoft.sqlserver.jdbc.SQLServerDriver"
        url="jdbc:sqlserver://192.168.1.51:1433;DatabaseName=demo"/>

  </GlobalNamingResources>
```

## context.xml

\Tomcat 8.5\conf

配置队列示例：

```markup
<Resource name="queue/connectionFactory"    
                auth="Container"    
                type="org.apache.activemq.ActiveMQConnectionFactory"  
                description="JMS Connection Factory"  
                factory="org.apache.activemq.jndi.JNDIReferenceFactory"  
                brokerURL="tcp://localhost:61616"  
                brokerName="LocalActiveMQBroker" />  

<Resource name="queue/queue0"    
                auth="Container"    
                type="org.apache.activemq.command.ActiveMQQueue"  
                description="My Queue"  
                factory="org.apache.activemq.jndi.JNDIReferenceFactory"  
                physicalName="TomcatQueue" />
```

配置DB示例：

```markup
<?xml version="1.0" encoding="UTF-8"?> 
<Context>
 <Resource name="jdbc/test" 
  auth="Container" 
  type="javax.sql.DataSource"
         maxActive="100" 
  maxIdle="30" 
  maxWait="10000"
         username="sa" password="" 
  driverClassName="net.sourceforge.jtds.jdbc.Driver"
         url="jdbc:jtds:sqlserver://localhost/pubs"/>
 </Context>
```

java代码

```java
 // Obtain our environment naming context
Context initCtx = new InitialContext();
Context envCtx = (Context) initCtx.lookup("java:comp/env");

// Look up our data source
DataSource ds = (DataSource)
  envCtx.lookup("jdbc/EmployeeDB");

// Allocate and use a connection from the pool
Connection conn = ds.getConnection();
... use this connection to access the database ...
conn.close();
```

