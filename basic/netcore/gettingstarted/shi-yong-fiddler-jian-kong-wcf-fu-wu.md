```xml
<system.net>
  <defaultProxy>
    <proxy bypassonlocal="False" usesystemdefault="True" 
    proxyaddress="http://127.0.0.1:8888" />
  </defaultProxy>
</system.net>
```

1. 启动Fiddler；
2. 点击 Tools | Fiddler Options => Connections => 端口设置为：8888；
3. 即可捕获。