# 存储库——Repository

## 1. 本地存储库

原理：

运行Maven的时候，Maven所需要的任何构件都是直接从本地仓库获取的。如果本地仓库没有，它会首先尝试从远程仓库下载构件至本地仓库，然后再使用本地仓库的构件。

默認為：${user.home}/.m2/repository。通過localRepository修改

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
   -->
   
  <localRepository>D:\apache-maven-3.5.0\localRepo</localRepository>
</setting>
```

指定本地倉庫：
```
mvn clean install -Dmaven.repo.local=D:\apache-maven-3.5.0\localRepo
```

## 2. 中央存储库

```xml
<repositories>  
  <repository>  
    <id>central</id>  
    <name>Maven Repository Switchboard</name>  
    <layout>default</layout>  
    <url>http://repo1.maven.org/maven2</url>  
    <snapshots>  
      <enabled>false</enabled>  
    </snapshots>  
  </repository>  
</repositories>  
```

中央仓库的id为central，远程url地址为http://repo1.maven.org/maven2，它关闭了snapshot版本构件下载的支持。

## 3. 远程仓库配置
傳說中的私服。

如中國鏡像：
```xml
<project>  
...   
  <repositories>  
    <repository>  
      <id>maven-net-cn</id>  
      <name>Maven China Mirror</name>  
      <url>http://maven.net.cn/content/groups/public/</url>  
      <releases>  
        <enabled>true</enabled>  
      </releases>  
      <snapshots>  
        <enabled>false</enabled>  
      </snapshots>  
    </repository>  
  </repositories>  
  <pluginRepositories>  
    <pluginRepository>  
      <id>maven-net-cn</id>  
      <name>Maven China Mirror</name>  
      <url>http://maven.net.cn/content/groups/public/</url>  
      <releases>  
        <enabled>true</enabled>  
      </releases>  
      <snapshots>  
        <enabled>false</enabled>  
      </snapshots>       
    </pluginRepository>  
  </pluginRepositories>  
...   
</project>  
```

Setting.xml中配置：

```xml
<settings>  
  ...  
  <profiles>  
    <profile>  
      <id>dev</id>  
      <!-- repositories and pluginRepositories here-->  
    </profile>  
  </profiles>  
  <activeProfiles>  
    <activeProfile>dev</activeProfile>  
  </activeProfiles>  
  ...  
</settings>  
```

REF：http://blog.csdn.net/joewolf/article/details/4876604
