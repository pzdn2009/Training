# IntelliJ IDEA安装lombok插件

Ref：http://www.cnblogs.com/jeffen/p/6008570.html

1. Settings→Plugins→Browse repositories
2. 输入lom后选择Install Plugin
3. 按照提示重启IDEA

![](/assets/idea_lom_plugins.png)


## 使用 var

classpath里面增加一个文件：

lombok.config
```config
lombok.var.flagUsage = ALLOW
```

pom中引入依赖：
```xml
<dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
</dependency>
```