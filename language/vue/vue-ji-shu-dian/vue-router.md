# vue-router

Docs：[https://router.vuejs.org/zh/guide/essentials/navigation.html](https://router.vuejs.org/zh/guide/essentials/navigation.html)

* 一个路由结构：path、component、name
* 路由数组定义应该传递给Vue作为参数启动
* $route与$router
* 动态路由参数:id（传递），$route.**params**.id（获取）
* 查询参数对应的是query
* 嵌套路由，router-view，可以嵌套
* router-link :to用于标签导航
* push,replace,go
* 命名路由，通过name指定
* 重定向：redirect
* 别名：alias

## 官方示例

```javascript
// 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter) 

// 1. 定义 (路由) 组件。 
// 可以从其他文件 import 进来 
const Foo = { template: '<div>foo</div>' } 
const Bar = { template: '<div>bar</div>' } 

// 2. 定义路由 
// 每个路由应该映射一个组件。 其中"component" 可以是 
// 通过 Vue.extend() 创建的组件构造器， 
// 或者，只是一个组件配置对象。 
// 我们晚点再讨论嵌套路由。 

const routes = [ 
   { path: '/foo', component: Foo }, 
   { path: '/bar', component: Bar } 
] 

// 3. 创建 router 实例，然后传 `routes` 配置 
// 你还可以传别的配置参数, 不过先这么简单着吧。 

const router = new VueRouter({
   routes // (缩写) 相当于 routes: routes 
}) 

// 4. 创建和挂载根实例。 
// 记得要通过 router 配置参数注入路由， 
// 从而让整个应用都有路由功能 

const app = new Vue({ router }).$mount('#app') 

// 现在，应用已经启动了！
```

## 使用

```javascript
//访问当前路由
this.$route
return this.$route.params.username

// 字符串 
router.push('home') 
// 对象 
router.push({ path: 'home' }) 
// 命名的路由 
router.push({ name: 'user', params: { userId: 123 }}) 
// 带查询参数，变成 /register?plan=private 
router.push({ path: 'register', query: { plan: 'private' }})

const userId = 123 
router.push({ name: 'user', params: { userId }}) // -> /user/123 
router.push({ path: `/user/${userId}` }) // -> /user/123
```

想要导航到不同的 URL，则使用 router.push 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

* push
* replace
* go\(n\)

## 路由组件传递参数

```javascript
const User = {
 template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
 routes: [
 { path: '/user/:id', component: User }
 ]
})
```

替代

```javascript
const User = {
 props: ['id'],
 template: '<div>User {{ id }}</div>'
}

const router = new VueRouter({
 routes: [
 { path: '/user/:id', component: User, props: true },
 // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
 {
 path: '/user/:id',
 components: { default: User, sidebar: Sidebar },
 props: { default: true, sidebar: false }
 }
 ]
})
```

* props设置为true
* props可以是一个对象
* props可以是一个表达式

vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

