# 安装

## 1. Maven安裝

1. 从Apache Maven [http://maven.apache.org/](http://maven.apache.org/) 下载Maven包，
2. 配置M2\_HOME或者MAVEN\_HOME
3. 添加到Path
4. 验证：mvn -version
5. 配置{M2\_HOME}/conf/settings.xml。Proxy或者mirror。

## 2. Nexus Repository Manager

### 2.1 簡介與使用私服

功能強大的包管理器，通常可以用來搭建私服。 http://:8081/ Sign In，缺省账号密码：admin/admin123

* proxy：代理第三方仓库的
* hosted：存储本地上传的组建和资源
* group：一般包含多个proxy仓库和hosted仓库，在项目中一般引入这种类型的仓库就可以下载到proxy和hosted中的包

pom.xml中**引入私服**：

```markup
<repositories>  
    <repository>  
        <id>nexus</id>  
        <name>Nexus Repository</name>  
        <url>http://<ip>:8081/repository/maven-public/</url>  
    </repository>  
</repositories>  
<pluginRepositories>  
    <pluginRepository>  
        <id>nexus</id>  
        <name>Nexus Plugin Repository</name>  
        <url>http://<ip>:8081/repository/maven-public/</url>  
    </pluginRepository>  
</pluginRepositories>
```

### 2.2 創建私服

* admin,admin123。admin登录nexus，Repositories -&gt; Create repository -&gt; maven2 \(hosted\)
* 填入name
* 选择Blob store
* 选择Deployment policy：Allow redeploy

添加到maven-public Group中：

* 进入Repositories -&gt; maven-public
* 在Group中，加入剛創建的maven Repository
* 点击：Save

REF：[http://blog.csdn.net/clementad/article/details/52670968。](http://blog.csdn.net/clementad/article/details/52670968。)

### 2.3 上傳jar包

maven\_home/conf/setting.xml中增加：

```markup
<server>  
    <id>nexus-releases</id>  
    <username>admin</username>  
    <password>admin123</password>  
</server>  
<server>  
    <id>nexus-snapshots</id>  
    <username>admin</username>  
    <password>admin123</password>  
</server>
```

上傳單個文件：

```text
mvn deploy:deploy-file ^
-DgroupId=com.aliyun.api ^
-DartifactId=taobao-sdk-java-auto_1455552377940 ^
-Dversion=2016.03.01 ^
-Dpackaging=jar ^
-Dfile=D:\3rd_jars\taobao-sdk-java-auto_1455552377940-20160330.jar ^
-Durl=http://<ip>:8081/repository/maven-3rd/ ^
-DrepositoryId=nexus-releases
```

### 2.4 Deploy部署jar包

設置私服地址pom：

```markup
<distributionManagement>    
    <repository>    
        <id>nexus-releases</id>    
        <name>Nexus Release Repository</name>    
        <url>http://<ip>:8081/repository/maven-releases/</url>    
    </repository>    
        <snapshotRepository>    
        <id>nexus-snapshots</id>    
        <name>Nexus Snapshot Repository</name>    
        <url>http://<ip>:8081/repository/maven-snapshots/</url>    
    </snapshotRepository>    
</distributionManagement>
```

* ID名称必须要与settings.xml中Servers配置的ID名称保持一致。
* 项目版本号中有SNAPSHOT标识的，会发布到Nexus Snapshots Repository, 否则发布到Nexus Release Repository，并根据ID去匹配授权账号。

執行命令：

```text
mvn deploy
```

