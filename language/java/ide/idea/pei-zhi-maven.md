# Idea 配置自定义Maven

Setting -&gt; Maven

1. Maven home directory：D:/apache-maven-3.5.0
2. User settings file：D:\apache-maven-3.5.0\conf\settings.xml
3. Local Repository：D:\apache-maven-3.5.0\localRepo

## 配置localRepository

```markup
<localRepository>D:\apache-maven-3.5.0\localRepo</localRepository>
```

## 配置镜像

```markup
<mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
     <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
  </mirrors>
```

