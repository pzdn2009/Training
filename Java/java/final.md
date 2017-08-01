# Final

## 獲取機器信息

```java
    public static void main(String[] args) throws Exception {
         InetAddress addr = InetAddress.getLocalHost();  
         String ip=addr.getHostAddress().toString(); //获取本机ip  
         String hostName=addr.getHostName().toString(); //获取本机计算机名称  
         System.out.println(ip);
         System.out.println(hostName);
    }
```

## 當前線程

```java
Thread.currentThread();
```

## Json首字母大寫

```java
@JsonProperty("CreateTime")
private LocalDateTime createTime;
```