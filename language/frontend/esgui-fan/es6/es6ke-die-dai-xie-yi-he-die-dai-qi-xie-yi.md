# ES6-可迭代协议和迭代器协议

* 可迭代协议

# 1. 可迭代协议

为了变成可迭代对象， 一个对象必须实现` @@iterator `方法, 意思是这个对象（或者它原型链 prototype chain 上的某个对象）必须有一个名字是` Symbol.iterator `的属性:
```js
 [Symbol.iterator]	
```
当一个对象需要被迭代的时候（比如开始用于一个for..of循环中），它的`@@iterator`**方法**被调用并且无参数，然后返回一个用于在迭代中获得值的`迭代器`。

# 2. 迭代器协议

迭代器协议定义了一种标准的方式来产生一个有限或无限序列的值。

* next()
 * done
 * value
 
# 3. 示例

`String, Array, TypedArray, Map and Set` 是所有内置可迭代对象， 因为它们的原型对象都有一个 `@@iterator` 方法.

## 3.1 自定义可迭代对象

```js
var myIterable = {};
//it属性，Generator
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};

//展开
[...myIterable]; // [1, 2, 3]
```

## 3.2 生成器式的迭代器

```js
//定义一个生成器
function* makeSimpleGenerator(array){
    var nextIndex = 0;
    
    while(nextIndex < array.length){
        yield array[nextIndex++];
    }
}

var gen = makeSimpleGenerator(['yo', 'ya']);

console.log(gen.next().value); // 'yo'
console.log(gen.next().value); // 'ya'
console.log(gen.next().done);  // true

// ...
```