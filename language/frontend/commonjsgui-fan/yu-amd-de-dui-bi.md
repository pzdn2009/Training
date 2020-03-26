# 与AMD的对比

# 1. AMD

* `require([module], callback)`
* `define(id, [depends], callback)`

```js
test.js
//id可以省略，依赖包，回调函数
define(['package/lib',...], function(lib) {
    function foo () {
        lib.log('hello world');
    }
    
    //公布对象
    return {
      foo: foo
    }
});

//加载，模块名，回调函数
require(['test'], function(test) {
  test.foo()
})
```

# 2. 区别

* CommonJS：同步的，服务器端的，Node执行如此。
* AMD，异步的，浏览器端的
* ADM主要实现：require.js和curl.js
