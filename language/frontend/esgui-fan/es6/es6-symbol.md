# ES6-Symbol

* 基础数据类型
* new出来的都是唯一的
* 作为KEY
* 代替常量
* 私有
* 全局

# 1. 基础数据类型

```js
let s1 = Symbol()
typeof s1  // 'symbol'
```

# 2. 作为key——对象属性名

```js
const PROP_NAME = Symbol()
const PROP_AGE = Symbol()

let obj = {
  [PROP_NAME]: "daniu"
}
obj[PROP_AGE] = 33

//output
obj[PROP_NAME]
obj[PROP_AGE]
```

一下场景会失效：
* Objects.keys
* for...in 
* JSON.stringify

# 3. 常量

```js
const TYPE_AUDIO = Symbol()
const TYPE_VIDEO = Symbol()
const TYPE_IMAGE = Symbol()
```

# 4. 全局


```
//类似单例
let gs1 = Symbol.for('全局1')
```