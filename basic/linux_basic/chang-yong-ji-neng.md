# 常用技能

## 常用技能

## 取文件的第一列？

```bash
cat file | awk '{print $1}'
```

### 查看端口被佔用？

```bash
netstat –apn | grep 8080
```

### 代理上網？

#### wget

```bash
# 编辑/etc/wgetrc，在最后加入  
sudo vim /etc/wgetrc

# Proxy  
http_proxy=http://username:password@proxy_ip:port/  
ftp_proxy=http://username:password@proxy_ip:port/
```

#### YUM

```text
# 编辑/etc/yum.conf，在最后加入  
sudo vim /etc/yum.conf

# Proxy  
proxy=http://username:password@proxy_ip:port/
```

系统全局代理  
如果需要为某个用户设置一个系统级的代理，可以在~/.bash\_profile中设置：

```bash
http_proxy="http://username:password@proxy_ip:port"  
export  http_proxy
```

