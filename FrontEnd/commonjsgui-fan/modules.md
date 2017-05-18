# CommonJS Modules/1.0

## 概述

CommonJS定义的模块分为:

1. 模块引用(require)

  require()用来引入外部模块；
2. 模块定义(exports)

  exports对象用于导出当前模块的方法或变量，唯一的导出口；
3. 模块标识(module)

  module对象就代表模块本身。

# 译文正文
英文地址：http://www.commonjs.org/specs/modules/1.0/

此规范指出了如何编写可以在同类模块系统中所共用的模块，这类模块系统可以同时在客户端和服务端，以安全的或者不安全的方式已经被实现了或者通过语法扩展可以被未来的系统所支持。这些模块需要提供顶级作用域的私有性，并提供从其他模块导入单例对象到自身并且可以导出自身API的能力。含蓄的说，这个规范定义了如果一个模块系统要支持共用模块，那么它需要提供的最少的功能特性。

## 契约
### 模块上下文
1. 在一个模块中，存在一个自由的变量”require”，它是一个函数。
 1. 这个“require”函数接收一个模块标识符。
 2. “require”返回外部模块所输出的API。
 3. 如果出现依赖闭环(dependency cycle)，那么外部模块在被它的传递依赖（transitive dependencies）所require的时候可能并没有执行完成；在这种情况下，”require”返回的对象必须至少包含此外部模块在调用require函数（会进入当前模块执行环境）之前就已经准备完毕的输出。
 4. 如果请求的模块不能返回，那么”require”必须抛出一个错误。
2. 在一个模块中，会存在一个名为”exports”的自由变量，它是一个对象，模块可以在执行的时候把自身的API加入到其中。
3. 模块必须使用”exports”对象来做为输出的唯一表示。

### 模块标识符
1. 模块标识符是一个以正斜杠分隔的多个”term”组成的字符串。
2. 一个term必须是一个驼峰格式的标识符，”.”或者”..”。
3. 模块标识符可以不加文件扩展名，比如”.js”。
4. 模块标识符可以是「相对的」或者「顶级的」(top-level)。如果一个模块标识符的第一个term是 “.”或者”..”，那么它是「相对的」。
5. 顶级标识符是概念上的模块命名空间的根。
6. 相对标识符是相对于在其内部调用了”require”的模块的标识符来进行解析的。

### 未规范
此规范对如下关于协同工作能力方面的重要内容未进行规范：

1. 模块是否可以通过数据库，文件系统或者工厂函数进行存储，或者可以通过链接库进行内部交换。
2. 模块加载器是否应该支持PATH变量用来解析模块标识符。

### 单元测试

[Unit Tests at Google Code](http://code.google.com/p/interoperablejs/) by Kris Kowal
[Unit Tests Git Mirror](http://github.com/ashb/interoperablejs/tree/master) by Ash Berlin

### 实例代码

math.js
```js
exports.add = function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
        sum += args[i++];
    }
    return sum;
};
```

increment.js
```js
var add = require('math').add;
exports.increment = function(val) {
    return add(val, 1);
};
```

program.js
```js
var inc = require('increment').increment;
var a = 1;
inc(a); // 2
```