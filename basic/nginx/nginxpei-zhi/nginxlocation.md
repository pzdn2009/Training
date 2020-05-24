# Nginx Location配置

## 1. 语法

```nginx
location [ = | ~ | ~* | ^~ ] uri {
  ...
}

location @name { 
  ... 
}
```

## 2. 匹配规则

* = ：全等匹配，精确匹配
* ~ ：区分大小写的正则匹配
* ~* ：不区分大小写的正则匹配
* ^~ ：优先级 高于 正则匹配，前缀匹配
* 一般 ：一般匹配，默认匹配

**匹配过程**：


## 3. @name用法

```nginx
location / {
    try_files $uri $uri/ @name
}
location @name {
    # ...do something
    proxy_pass http://localhost:10001/
}
```

## 4. try_files

```
try_files指令
语法：try_files file ... uri 或 try_files file ... = code
默认值：无
作用域：server location
描述：按顺序检查文件是否存在，返回第一个找到的文件或文件夹(结尾加斜线表示为文件夹)，
如果所有的文件或文件夹都找不到，会进行一个内部重定向到最后一个参数。
```

```nginx
//1
try_files $uri $uri/ /index.php?q=$uri&$args;

//2
try_files /app/cache/ $uri @fallback; 
index index.php index.html;
```

## 4. proxy_pass

* proxy_pass后面的url加/，表示绝对根路径；
* 如果没有/，表示相对路径，把匹配的路径部分也给代理走。


```nginx
//第一种
location /proxy/ {
    proxy_pass http://127.0.0.1/;
}
代理到URL：http://127.0.0.1/test.html

//第二种（相对于第一种，最后少一个 / ）
location /proxy/ {
    proxy_pass http://127.0.0.1;
}
代理到URL：http://127.0.0.1/proxy/test.html

//第三种：
location /proxy/ {
    proxy_pass http://127.0.0.1/aaa/;
}
代理到URL：http://127.0.0.1/aaa/test.html

//第四种（相对于第三种，最后少一个 / ）
location /proxy/ {
    proxy_pass http://127.0.0.1/aaa;
}
代理到URL：http://127.0.0.1/aaatest.html
```

Ref:https://www.jianshu.com/p/b010c9302cd0