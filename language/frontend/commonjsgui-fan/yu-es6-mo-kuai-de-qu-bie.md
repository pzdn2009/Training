# 与ES6 模块的区别

* ES6：export,import
* CommonJS：module,export,require，服务端的

# 1. ES6

```js
/** 定义模块 math.js **/
var basicNum = 0;
var add = function (a, b) {
    return a + b;
};

//语法：export { ... }
export { basicNum, add };

//导入模块
import { basicNum, add } from './math';
function test(ele) {
    ele.textContent = add(99 + basicNum);
}

```

# 2. 区别

** CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。**


** CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
**

> 运行时加载: CommonJS 模块就是**对象**；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。

> 编译时加载: ES6 模块不是对象，而是通过 export 命令显式指定输出的代码，import时采用静态命令的形式。即在import时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。

Eg：
```js
// lib.js
var counter = 3;
function incCounter() {
  counter++;
}
module.exports = {
  counter: counter,
  incCounter: incCounter,
};
// main.js
var mod = require('./lib');
 
console.log(mod.counter);  // 3
mod.incCounter();
console.log(mod.counter); // 3

//----------------

// lib.js
export let counter = 3;
export function incCounter() {
  counter++;
}
 
// main.js
import { counter, incCounter } from './lib';
console.log(counter); // 3
incCounter();
console.log(counter); // 4
```