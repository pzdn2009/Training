# 加载原理


Ref：https://www.ruanyifeng.com/blog/2015/05/commonjs-in-browser.html

# 1.原理

浏览器不兼容CommonJS的根本原因，在于缺少四个Node.js环境的变量。

* module
* exports
* require
* global

只要能够提供这四个变量，浏览器就能加载 CommonJS 模块。

Eg：
```js
var module = {
  exports: {}
};

(function(module, exports) {
  exports.multiply = function (n) { return n * 1000 };
}(module, module.exports))

var f = module.exports.multiply;
f(5) // 5000 
```

# 2. Browserify实现

```
# 将commonJS转为浏览器可运行的js
$ browserify main.js > compiled.js

# 安装此包，来查看browerify的原理
$ npm install browser-unpack -g
# 查看
$ browser-unpack < compiled.js

[
  {
    "id":1,
    "source":"module.exports = function(x) {\n  console.log(x);\n};",
    "deps":{}
  },
  {
    "id":2,
    "source":"var foo = require(\"./foo\");\nfoo(\"Hi\");",
    "deps":{"./foo":1},
    "entry":true
  }
]

```

# 3. tiny-browser-require

使用：

```js
//注册（路径，模块信息）
require.register("./foo.js", function(module, exports, require){
  module.exports = function(x) {
    console.log(x);
  };
});

var foo = require("./foo.js");
foo("Hi");
```

```js
//定义require函数：./foo.js
function require(p){
  //路径解析：./foo.js
  var path = require.resolve(p);
  //读取到模块：fn
  var mod = require.modules[path];
  if (!mod) throw new Error('failed to require "' + p + '"');
  //如果没有exports属性，初始化一下
  if (!mod.exports) {
    mod.exports = {};
    mod.call(mod.exports, mod, mod.exports, require.relative(path));
  }
  //console.log(x)
  return mod.exports;
}

//modules是一个对象（值拷贝）
require.modules = {};

//解析加载的路径
require.resolve = function (path){
  //./foo.js
  var orig = path;
  //正规化：./foo.js.js
  var reg = path + '.js';
  //同时寻找index.js：./foo.js/index.js
  var index = path + '/index.js';
  //尝试从modules对象中加载
  return require.modules[reg] && reg
    || require.modules[index] && index
    || orig;
};

//注册：./foo.js, fn
require.register = function (path, fn){
  //require.modules["./foo.js"] = fn
  require.modules[path] = fn;
};

require.relative = function (parent) {
  return function(p){
    if ('.' != p.charAt(0)) return require(p);
    var path = parent.split('/');
    var segs = p.split('/');
    path.pop();

    for (var i = 0; i < segs.length; i++) {
      var seg = segs[i];
      if ('..' == seg) path.pop();
      else if ('.' != seg) path.push(seg);
    }

    return require(path.join('/'));
  };
};
```