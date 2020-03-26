# ES6

* [ES6-let & const](/language/frontend/esgui-fan/es6/es6-let-and-const.md)
* [ES6-解构](/language/frontend/esgui-fan/es6/es6jie-gou.md)
* [ES6-Symbol](/language/frontend/esgui-fan/es6/es6-symbol.md)
* [ES6-Promise](/language/frontend/esgui-fan/es6/promise.md)

## arrows箭头

和 C\# 的lambda表达式没什么区别。

## classes类

ES6 classes are a simple sugar over the **prototype-based** OO pattern.

Classes support prototype-based inheritance, super calls, instance and static methods and constructors.

类支持基于原型的继承，super调用，实例和静态方法和构造函数。

```javascript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

## enhanced object literals

## template strings字符串模板

```javascript
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```

## default + rest + spread默认+不定+延展

这三个均为参数的特性：

* 默认参数 

  ```javascript
  function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
  }
  f(3) == 15
  ```

* 不定参数

  ```javascript
  function f(x, ...y) {
  // y is an Array
  return x * y.length;
  }
  f(3, "hello", true) == 6
  ```

* 延展参数

  ```javascript
  function f(x, y, z) {
  return x + y + z;
  }
  // Pass each elem of array as argument
  f(...[1,2,3]) == 6
  ```

## iterators + for..of

## generators

## unicode

## modules模块

```javascript
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```

```javascript
// app.js
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```

```javascript
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

## module loaders

## map + set + weakmap + weakset

```javascript
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
// Because the added object has no other references, it will not be held in the set
```

## proxies代理

```javascript
// Proxying a normal object
var target = {};
var handler = {
  get: function (receiver, name) {
    return `Hello, ${name}!`;
  }
};

var p = new Proxy(target, handler);
p.world === 'Hello, world!';

// Proxying a function object
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() === 'I am the proxy';
```

更多的运行时元操作：

```javascript
var handler =
{
  get:...,
  set:...,
  has:...,
  deleteProperty:...,
  apply:...,
  construct:...,
  getOwnPropertyDescriptor:...,
  defineProperty:...,
  getPrototypeOf:...,
  setPrototypeOf:...,
  enumerate:...,
  ownKeys:...,
  preventExtensions:...,
  isExtensible:...
}
```

另一个例子：

```javascript
var engineer = { name: 'Joe Sixpack', salary: 50 };

var interceptor = {
  set: function (receiver, property, value) {
    console.log(property, 'is changed to', value);
    receiver[property] = value;
  }
};

engineer = Proxy(engineer, interceptor);
engineer.salary = 60;//salary is changed to 60
```

## subclassable built-ins

## math + number + string + array + object APIs

## binary and octal literals

## reflect api

## tail calls

