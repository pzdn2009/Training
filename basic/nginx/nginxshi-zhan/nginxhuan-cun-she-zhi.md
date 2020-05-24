# Nginx缓存设置

## 1. 如何禁用文件缓存?

```nginx
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|js|css)$ {
  # 禁止缓存，每次都从服务器请求
  add_header Cache-Control no-store;
}
```

## 2. 如果启用文件缓存？

```nginx
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|js|css)$ {
  #设置缓存上面定义的后缀文件缓存到浏览器的生存时间
  expires 3d;
  #expires 12h;
} 
```