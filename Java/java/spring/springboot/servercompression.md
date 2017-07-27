# server.compression

```properties
server.compression.enabled=true
server.compression.mime-types=application/json,application/xml,text/html,text/xml,text/plain

#默认就是2048 byte，默认情况下，仅会压缩2048字节以上的内容
server.compression.min-response-size=2048
```

more：
* org.springframework.boot.context.embedded.**Compression**
* org.springframework.boot.autoconfigure.web.**ServerProperties**