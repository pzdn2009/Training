# ES5-map

语法：
```js
var new_array = arr.map(function callback(currentValue[, index[,array]]) {
 // Return element for new_array 
}[, thisArg])

Eg:
arr.map(callback(currentValue))
arr.map(callback(currentValue,index))
arr.map(callback(currentValue,index,arr))
arr.map(callback(currentValue,index), this)
```

* **callback** 生成新数组元素的函数，使用三个参数：
* **currentValue** callback 数组中正在处理的当前元素。
* **index**可选 callback 数组中正在处理的当前元素的索引。
* **array**可选 map 方法调用的数组。
* **thisArg**可选 执行 callback 函数时值被用作this。

Eg:
```js
//callback
var numbers = [1, 4, 9];
var roots = numbers.map(Math.sqrt);


var kvArray = [{key: 1, value: 10}, 
               {key: 2, value: 20}, 
               {key: 3, value: 30}];

var reformattedArray = kvArray.map(function(obj) { 
   var rObj = {};
   rObj[obj.key] = obj.value;
   return rObj;
});

var map = Array.prototype.map
var a = map.call("Hello World", function(x) { 
  return x.charCodeAt(0); 
})
// a的值为[72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100]
```