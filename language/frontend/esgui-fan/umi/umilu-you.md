# Umi-路由

* 单页应用
* 路由格式
* 页面跳转
* hash路由：{ type: 'browser'|hash|memory }
* Link组件
* 路由组件参数
* 传递参数给子路由

# 1. 路由定义

```js
export default {
  //集合
  routes: [
    //路由对象
    { exact: true, path: '/', component: 'index' },
    { exact: true, path: '/user', component: 'user' },
  ],
}
```

* path：路径，`string，string[]`
* component：`string`，用于渲染的React组件，从`src/pages`开始找，如果是相对于`src`的，则使用@，EG：`component: '@/layouts/basic'`
* exact：path是否完全匹配，bool
* routes：配置子路由，通常在需要为多个路径增加 layout 组件时使用。`[]`
* redirect：配置路由跳转。 `string`
* wrappers：配置路由的高阶组件封装。`string[]`
* title：路由的标题。`string`

## 1.1 子路由

```js
export default {
  routes: [
    //登录
    { path: '/login', component: 'login' },
    //根
    {
      path: '/',
      //使用布局组件
      component: '@/layouts/index',
      //子路由
      routes: [
        //list路由
        { path: '/list', component: 'list' },
        //admin路由
        { path: '/admin', component: 'admin' },
      ],
    }, 
  ],
}
```

然后在` src/layouts/index` 中通过 `props.children` 渲染子路由，

```js
//定义函数式组件
export default (props) => {
  //div包裹
  //props.children：获取当前组件的子组件。
  return <div style={{ padding: 20 }}>{ props.children }</div>;
}
```

这样，访问 /list 和 /admin 就会带上 src/layouts/index 这个 layout 组件。

## 1.2 高阶组件封装——wrappers

```js
export default {
  routes: [
    { path: '/user', component: 'user',
      //配置一个包裹组件
      wrappers: [
        '@/wrappers/auth',
      ],
    },
    { path: '/login', component: 'login' },
  ]
}
```

然后在 `src/wrappers/auth` 中，

```js
//函数式组件
export default (props) => {
  //调用userAuth结构结构到isLogin
  const { isLogin } = useAuth();
  if (isLogin) {
    //登录，返回组件的包裹
    return <div>{ props.children }</div>;
  } else {
    //重定向到登录页
    redirectTo('/login');
  }
}
```

# 2. 页面跳转

* `import {history} from 'umi'`
* `push`
* `goBack`

```js
//导入历史组件
import { history } from 'umi';

// 跳转到指定路由
history.push('/list');

// 带参数跳转到指定路由
history.push('/list?a=b');
history.push({
  pathname: '/list',
  query: {
    a: 'b',
  },
});

// 跳转到上一个路由
history.goBack();
```

# 3. Link组件

* `import {Link} from 'umi'`
* `to`
* `Link` 只用于单页应用的内部跳转，如果是外部地址跳转请使用`a `标签

```js
import { Link } from 'umi';

//JSX
export default () => (
  <div>
    //然后点击 Users Page 就会跳转到 /users 地址。
    <Link to="/users">Users Page</Link>
  </div>
);
```

# 4. 路由组件参数

路由组件可通过 `props` 获取到以下属性，

* match，当前路由和 url match 后的对象，包含 params、path、url 和 isExact 属性
* location，表示应用当前出于哪个位置，包含 pathname、search、query 等属性
* history，同 api#history 接口
* route，当前路由配置，包含 path、exact、component、routes 等

比如：
```js
export default function(props) {
  console.log(props.route);
  return <div>Home Page</div>;
}
```

# 5. 传递参数给子路由

* cloneElement

```js
import React from 'react';

export default function Layout(props) {
  return React.Children.map(props.children, child => {
    return React.cloneElement(child, { foo: 'bar' });
  });
}
```