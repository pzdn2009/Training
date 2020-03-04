# yml

application.properties与application.yml可以等价替换。

## 基本yml与properties对比

```text
# yml
environments:  
    dev:  
        url: http://dev.bar.com  
        name: Developer Setup  
    prod:  
        url: http://foo.bar.com  
        name: My Cool App  

# properties
environments.dev.url=http://dev.bar.com  
environments.dev.name=Developer Setup  
environments.prod.url=http://foo.bar.com  
environments.prod.name=My Cool App
```

## 数组

```text
# yml
my:  
   servers:  
       - dev.bar.com  
       - foo.bar.com  


# yml
my.servers[0]=dev.bar.com  
my.servers[1]=foo.bar.com
```

对应的Java代码：

```java
@ConfigurationProperties(prefix="my")  
public class Config {  

    private List<String> servers = new ArrayList<String>();  

    public List<String> getServers() {  
        returnt his.servers;  
    }  
}
```

