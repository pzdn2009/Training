# router分析

* linkActiveClass: 'active-link' 

## nuxt自动示例

导入组件：
```js

import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)
```

导入page：

```js
const _5ea2fc2c = () => import('../pages/couponseed/index.vue'
/* webpackChunkName: "pages/couponseed/index" */)
.then(m => m.default || m)
```

创建路由(from nuxt)：

```js
export function createRouter () {
 return new Router({
 routes: [
 {
 path: "/couponseed",
 component: _5ea2fc2c,
 name: "couponseed"
 },
 {
 path: "/firstdiscount:activity",
 component: _9b757320,
 name: "firstdiscountactivity"
 },
 {
 path: "/",
 component: _15aea572,
 name: "index"
 }],
 fallback: false
 })
}
```
