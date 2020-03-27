# Umi-Mock数据

* Umi 约定 /mock 文件夹下所有文件为 mock 文件。
* 编写

```ts
export default {
  // 支持值为 Object 和 Array
  'GET /api/users': { users: [1, 2] },

  // GET 可忽略
  '/api/users/1': { id: 1 },

  // 支持自定义函数，API 参考 express@4
  'POST /api/users/create': (req, res) => {
    // 添加跨域请求头
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.end('ok');
  },
}
```
* 配置：mock
* 关闭

```js
export default {
  mock: false,
};
```

## 引入Mock.js

Mock.js 是常用的辅助生成模拟数据的三方库，借助他可以提升我们的 mock 数据能力。

```ts
import mockjs from 'mockjs';

export default {
  // 使用 mockjs 等三方库
  'GET /api/tags': mockjs.mock({
    'list|100': [{ name: '@city', 'value|1-100': 50, 'type|0-2': 1 }],
  }),
};
```