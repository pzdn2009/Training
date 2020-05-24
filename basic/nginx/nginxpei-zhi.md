# Nginx配置

* 概述
* 内置变量
* [Location配置](/basic/nginx/nginxpei-zhi/nginxlocation.md)

## 1. 概述

配置的层次结构：
```
main-->http-->server-->location
```

基本格式：
```nginx
<section> {
   <directive> <parameters>;
}
```

* root: 站点目录
* index: 可以配置多个主页

## 2. 内置变量

分组：
* 客户端
* 服务端
* 请求：参数、Cookie
* 响应

```nginx
$remote_addr		//获取客户端ip
$binary_remote_addr	//客户端ip（二进制)
$remote_port		//客户端port，如：50472
$remote_user		//已经经过Auth Basic Module验证的用户名

$server_protocol	//请求使用的协议，通常是HTTP/1.0或HTTP/1.1，如：HTTP/1.1
$server_addr		//服务器IP地址，在完成一次系统调用后可以确定这个值
$server_name		//服务器名称，如：blog.sakmon.com
$server_port		//请求到达服务器的端口号,如：80

$host			//请求主机头字段，否则为服务器名称
$request		//用户请求信息，如：GET ?a=1&b=2 HTTP/1.1
$request_filename	//当前请求的文件的路径名，由root或alias和URI request组合而成，如：/2013/81.html

$status			//请求的响应状态码,如:200
$body_bytes_sent        // 响应时送出的body字节数数量。即使连接中断，这个数据也是精确的,如：40
$content_length	       // 等于请求行的“Content_Length”的值
$content_type	       // 等于请求行的“Content_Type”的值
$http_referer	       // 引用地址
$http_user_agent      // 客户端agent信息,如：Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.76 Safari/537.36
$args		     //与$query_string相同 等于当中URL的参数(GET)，如a=1&b=2
$document_uri	     //与$uri相同  这个变量指当前的请求URI，不包括任何参数(见$args) 如:/2013/81.html
$document_root	     //针对当前请求的根路径设置值
$hostname	     //如：centos53.localdomain

$http_cookie	    //客户端cookie信息
$cookie_COOKIE	    //cookie COOKIE变量的值

$is_args	//如果有$args参数，这个变量等于”?”，否则等于”"，空值，如?
$limit_rate	//这个变量可以限制连接速率，0表示不限速
$query_string	    // 与$args相同 等于当中URL的参数(GET)，如a=1&b=2
$request_body	   // 记录POST过来的数据信息
$request_body_file	//客户端请求主体信息的临时文件名
$request_method	      //客户端请求的动作，通常为GET或POST,如：GET
$request_uri	      //包含请求参数的原始URI，不包含主机名，如：/2013/81.html?a=1&b=2
$scheme		       //HTTP方法（如http，https）,如：http
$uri			//这个变量指当前的请求URI，不包括任何参数(见$args) 如:/2013/81.html
$request_completion	//如果请求结束，设置为OK. 当请求未结束或如果该请求不是请求链串的最后一个时，为空(Empty)，如：OK
```