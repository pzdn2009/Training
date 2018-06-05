# 計算屬性， 偵聽屬性，class，style

1.  computed；
2.  watch;
3.  v-bind:class
4.  組件上使用class
5. v-bind:style

## 計算屬性
```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

计算属性是基于它们的依赖进行**缓存**的。计算属性只有在它的相关依赖发生改变时才会重新求值。

计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：

```js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

## 侦听属性

当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch——特别是如果你之前使用过 AngularJS。

然而，**通常更好的做法是使用计算属性**而不是命令式的 watch 回调。

使用方式：
```js
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
```

## class & style

```js
<div v-bind:class="classObject"></div>
<div v-bind:style="[baseStyles, overridingStyles]"></div> 

```
