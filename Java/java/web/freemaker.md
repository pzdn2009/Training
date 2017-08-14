# Freemaker

## 處理缺失值

缺失：一个不存在(未定义)的变量和一个变量但是包含null值。

```ftl
Welcome ${user!"Anonymous"}!

<#if user??>

(animals.python.price)!0

(animals.python.price)??

```
* 第一條：如果確實，就返回Anonymous
* 第二條：??檢測是否缺失
* 第三條：多步變量，直到最後一個price不為空

## 定義局部變量

```ftl
<#assign x=0 />
<#assign x=x+1 />
<#assign loginname="pzdn">
<#assign page_title="資源管理">
```

## 佈局layout

Ref:https://www.oschina.net/code/snippet_1029551_48966

_layout.ftl：
```ftl
<#macro layout>
<html>
<body>
  <div id="head"><#nested "head" /></div>
  <div id="content"><#nested "content" /></div>
</body>
</html>
</#macro>
```

page2.ftl：
```ftl
<#include "_layout.ftl">
 
<@layout ; section> 
  <#if section = "head"> 
    <div>this is my header</div> 
  <#elseif section = "content" > 
    <div>this is my content.</div> 
  <#else> 
    <div>Unsupported section??</div> 
  </#if> 
</@layout>
```

## 宏

Ref：http://freemarker.org/docs/ref_directive_macro.html

語法：
```ftl
<#macro name param1 param2 ... paramN>
  ...
  <#nested loopvar1, loopvar2, ..., loopvarN>
  ...
  <#return>
  ...
</#macro>
```

@name調用宏。

## freemarker名稱

spring-boot-starter-freemarker
並非
spring-boot-starter-freeMarker，不然會出現引擎解析錯誤。


