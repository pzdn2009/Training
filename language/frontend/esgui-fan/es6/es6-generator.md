# ES6-Generator

Ref：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*

`function*` 这种声明方式(function关键字后跟一个星号）会定义一个生成器函数 (generator function)，它返回一个  `Generator`  对象。

语法：
```js
function* name([param[, param[, ... param]]]) { statements }
```

```js
function *gen(){
    yield 10;
    x=yield 'foo';
    yield x;
}

var gen_obj=gen();
console.log(gen_obj.next());// 执行 yield 10，返回 10
console.log(gen_obj.next());// 执行 yield 'foo'，返回 'foo'
console.log(gen_obj.next(100));// 将 100 赋给上一条 yield 'foo' 的左值，即执行 x=100，返回 100
console.log(gen_obj.next());// 执行完毕，value 为 undefined，done 为 true
```

规则：
>* 赋值上一条的左值
* 一次next对应一次yield

# 1. 语法

* value
* done 
* next()

```js
function* gen(){
    yield 10;
    x=yield 'foo';
    yield x;
}

var gen_obj=gen();
console.log(gen_obj.next());// 执行 yield 10，返回 10
console.log(gen_obj.next());// 执行 yield 'foo'，返回 'foo'
console.log(gen_obj.next(100));// 将 100 赋给上一条 yield 'foo' 的左值，即执行 x=100，返回 100
console.log(gen_obj.next());// 执行完毕，value 为 undefined，done 为 true
```

# 2. 特性

* 接收参数：

```js
function* idMaker() {
    var index = arguments[0] || 0;
    while(true)
        yield index++;
}

var gen = idMaker(5);
console.log(gen.next().value); // 5
console.log(gen.next().value); // 6
```

* yield*

```js
//Generator
function* anotherGenerator(i) {
  yield i + 1;
  yield i + 2;
  yield i + 3;
}

function* generator(i){
  yield i;
  //给另外一个，一共三次迭代
  yield* anotherGenerator(i);// 移交执行权
  yield i + 10;
}

var gen = generator(10);

console.log(gen.next().value); // 10
console.log(gen.next().value); // 11
console.log(gen.next().value); // 12
console.log(gen.next().value); // 13
console.log(gen.next().value); // 20
```

* 传递参数

```js
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2; // 4 + 2 
                                  // first =4 是next(4)将参数赋给上一条的
    yield second + 3;             // 5 + 3
}

let iterator = createIterator();

console.log(iterator.next());    // "{ value: 1, done: false }"
//赋值给上一条的左值
console.log(iterator.next(4));   // "{ value: 6, done: false }"
console.log(iterator.next(5));   // "{ value: 8, done: false }"
console.log(iterator.next());    // "{ value: undefined, done: true }"
```

# 3. 使用迭代器遍历二维数组并转换成一维数组

* 递归
* 展开迭代器

```js
//定义
function* iterArr(arr) {
  //如果是数组
  if (Array.isArray(arr)) { 
      //遍历
      for(let i=0; i < arr.length; i++) {
          //递归，移交执行权
          yield* iterArr(arr[i]);   // (*)递归
      }
  } else {   
      //非数组直接yield
      yield arr;
  }
}

// 使用 for-of 遍历:
var arr = ['a', ['b', 'c'], ['d', 'e']];
for(var x of iterArr(arr)) {
        console.log(x);               // a  b  c  d  e
 }

// 或者直接将迭代器展开:
var arr = [ 'a', ['b',[ 'c', ['d', 'e']]]];
var gen = iterArr(arr);
arr = [...gen];                        
// ["a", "b", "c", "d", "e"]

```