# vue 基礎

## 基礎指令

* **\{\{ expr \}\}**——Mustache 語法，插值
* **v-once**：只綁定一次，後續的data更新不會改變視圖
* **v-html**：輸出原始的html
* **v-bind**：綁定tag的屬性，作用子組件的輸入。Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令。
```js
<div v-bind:id="dynamicId"></div>
<a v-bind:href="url">...</a>
<!-- 缩写 -->
<a :href="url">...</a>
```
* **v-if**：值為Boolean，如果為true，則顯示，反之，則隱藏
* **v-for**： v-for="value in object" , v-for="(value, key) in object">
* **v-on**：綁定事件，指令添加一个事件监听器
```js
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>
<!-- 缩写 -->
<a @click="doSomething">...</a>
```
* v-show：
* **v-model**： 雙向綁定，用於input。

## 組件

props：接收父組件傳遞的數據，HTML 特性上。
camelCase (驼峰式命名) 的 prop 需要转换为相对应的 kebab-case (短横线分隔式命名)
   
Vue.components定義組件：template,props等屬性

Vue Instance，一個應用只有一個Root Instance，data必須保持一個初始值。

## 生命週期

* beforeCreate
* **created**
* beforeMout
* **mounted**
* beforeUpdate
* **updated**
* beforeDestroy
* **destroyed**

![](/assets/lifecycle.png)

如果有template，則編譯模板到render函數，反之，編譯el的HTML作為template。