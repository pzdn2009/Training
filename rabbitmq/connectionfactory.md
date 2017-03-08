# ConnectionFactory
## 基礎

屬性：

- 默認連接超時：30s
- 默認心跳：60ms
- 默認用戶名：guest
- 默認密碼：guest
- 默認VHOST：/

方法：

**CreateConnection**：創建連接
參數：
- Url：必須以amqp或者amqps開頭的，默認端口為5671。
  eg:amqp://admin:admin@192.168.2.106:5671/CGPDev
  
  可以解析出：UserName,Password,VHOST,HostName
- HostName：主機名。其他採用默認值。
