# Umi-约定式路由

* 约定式路由
* 动态路由
* 嵌套路由
* 全局layout
* 404路由
* 扩展路由属性

# 1. 约定式路由

如果没有 `routes` 配置，Umi 会进入**约定式路由模式**，分析 `src/pages` 目录拿到路由配置。

需要注意的是，满足以下任意规则的文件不会被注册为路由，

>* 以 . 或 _ 开头的文件或目录
* 以 d.ts 结尾的类型定义文件
* 以 test.ts、spec.ts、e2e.ts 结尾的测试文件（适用于 .js、.jsx 和 .tsx 文件）
* components 和 component 目录
* utils 和 util 目录
* 不是 .js、.jsx、.ts 或 .tsx 文件
* 文件内容不包含 JSX 元素

# 2. 动态路由

约定 `[] `包裹的文件或文件夹为动态路由。

```js
.
  └── pages
    └── [post]
      ├── index.tsx
      └── comments.tsx
    └── users
      └── [id].tsx
    └── index.tsx

[
  { exact: true, path: '/', component: '@/pages/index' },
  { exact: true, path: '/users/:id', component: '@/pages/users/[id]' },
  { exact: true, path: '/:post/', component: '@/pages/[post]/index' },
  {
    exact: true,
    path: '/:post/comments',
    component: '@/pages/[post]/comments',
  },
];
```

# 3. 嵌套路由

Umi 里约定目录下有 `_layout.tsx` 时会生成嵌套路由，以 `_layout.tsx` 为该目录的 `layout`。`layout` 文件需要返回一个 `React` 组件，并通过 `props.children` 渲染子组件。

* _layout.tsx
 * React组件
 * pros.children
* 按功能划分，同级目录

```js
.
└── pages
    └── users
        ├── _layout.tsx
        ├── index.tsx
        └── list.tsx

[
  { exact: false, path: '/users', component: '@/pages/users/_layout',
    routes: [
      { exact: true, path: '/users', component: '@/pages/users/index' },
      { exact: true, path: '/users/list', component: '@/pages/users/list' },
    ]
  }
]
```

# 4. 全局 layout

约定 `src/layouts/index.tsx` 为全局路由。返回一个 `React` 组件，并通过 `props.children` 渲染子组件。

你可能需要针对不同路由输出不同的全局 layout，Umi 不支持这样的配置.

比如想要针对 /login 输出简单布局，

```js
export default function(props) {
  if (props.location.pathname === '/login') {
    return <SimpleLayout>{ props.children }</SimpleLayout>
  }

  return (
    <>
      <Header />
      { props.children }
      <Footer />
    </>
  );
}
```

# 5. 404 路由

约定` src/pages/404.tsx` 为 `404` 页面，需返回 `React` 组件。
```js
.
└── pages
    ├── 404.tsx
    ├── index.tsx
    └── users.tsx

[
  { exact: true, path: '/', component: '@/pages/index' },
  { exact: true, path: '/users', component: '@/pages/users' },
  { component: '@/pages/404' },
]
```

# 6. 扩展路由属性

支持在代码层通过导出静态属性的方式扩展路由。

```js
function HomePage() {
  return <h1>Home Page</h1>;
}

HomePage.title = 'Home Page';

export default HomePage;
```
其中的 `title` 会附加到路由配置中。


