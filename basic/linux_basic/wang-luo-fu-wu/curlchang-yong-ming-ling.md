# curl常用命令

[https://curl.haxx.se/docs/httpscripting.html](https://curl.haxx.se/docs/httpscripting.html)

* -X METHOD  指定方法。
* --header 指定頭部。
* -d 指定使用POST方式传递数据。
* -u username:password，用戶名和密碼
* -O 使用URL中默认的文件名保存文件到本地
* -o 将文件保存为命令行中指定的文件名的文件中
* -L 进行强制重定向
* -C 可对大文件使用断点续传功能
* -T 选项可将指定的本地文件上传到FTP服务器上

```java
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ \ 
   "header": { \ 
     "cardInfoId": 0, \ 
     "channel": "string", \ 
   }, \ 
   "orderId": "string", \ 
   "secureId": "string", \ 
   "transactionId": "string" \ 
 }' 'http://172.18.21.192:18888/api/mpg/direct/retrieveTransaction'
```

```text
# 重定向
curl -L www.likegeeks.com

# 超时
curl -m 60 example.com

# 用户名和密码
curl -u username:password ftp://example.com

# 静默
curl -s http://example.com --output index.html
```

