# Modules

* `require`函数，用于导入模块
* `module.exports`变量，用于导出模块

# 1. 概述

CommonJS定义的模块分为:

1. 模块引用(require)

   `require()`用来引入外部模块；

2. 模块定义(exports)

   `exports`对象用于导出当前模块的方法或变量，唯一的导出口；

3. 模块标识(module)

   `module`对象就代表模块本身。

# 2. 模块上下文

>1. 在一个模块中，存在一个自由的变量”`require`”，它是一个函数。
   1. 这个“`require`”函数接收一个模块标识符。
   2. “`require`”返回外部模块所输出的API。
   3. 如果出现依赖闭环(dependency cycle)，那么外部模块在被它的传递依赖（transitive dependencies）所require的时候可能并没有执行完成；在这种情况下，”require”返回的对象必须至少包含此外部模块在调用require函数（会进入当前模块执行环境）之前就已经准备完毕的输出。
   4. 如果请求的模块不能返回，那么”require”必须抛出一个错误。
2. 在一个模块中，会存在一个名为”`exports`”的自由变量，它是一个对象，模块可以在执行的时候把自身的API加入到其中。
3. 模块必须使用”`exports`”对象来做为输出的唯一表示。

# 3. 定义模块

```js
// 定义模块math.js
var basicNum = 0;
function add(a, b) {
  return a + b;
}
module.exports = { //在这里写上需要向外暴露的函数、变量
  add: add,
  basicNum: basicNum
}
```

PS：使用`module.exports = {}`

# 4. 使用模块

```js
// 引用自定义的模块时，参数包含路径，可省略.js
var math = require('./math');
math.add(2, 5);

// 引用核心模块时，不需要带路径
var http = require('http');
http.createService(...).listen(3000);
```

PS：使用`require()`.

# 4. 实例代码

math.js

```javascript
exports.add = function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
        sum += args[i++];
    }
    return sum;
};
```

increment.js

```javascript
var add = require('math').add;
exports.increment = function(val) {
    return add(val, 1);
};
```

program.js

```javascript
var inc = require('increment').increment;
var a = 1;
inc(a); // 2
```

