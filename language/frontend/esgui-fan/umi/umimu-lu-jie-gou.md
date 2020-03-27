# Umi-目录结构

```
.
├── package.json
├── .umirc.ts：配置文件，包含 umi 内置功能和插件的配置。
├── .env：环境变量
├── dist：build输出目录
├── mock：此目录下所有 js 和 ts 文件会被解析为 mock 文件。
├── public：此目录下所有文件会被 copy 到输出路径
└── src
    ├── .umi
    ├── layouts/index.tsx：约定式路由时的全局布局文件
    ├── pages ：组件
        ├── index.less
        └── index.tsx
    └── app.ts：运行时配置文件，可以在这里扩展运行时的能力，比如修改路由、修改 render 方法等。
```

# 1. package.json

包含插件和插件集，以` @umijs/preset-、@umijs/plugin-、umi-preset- `和 `umi-plugin- `开头的依赖会被自动注册为插件或插件集。

```json
{
  "private": true,
  "scripts": {
    "start": "umi dev",
    "build": "umi build",
    "prettier": "prettier --write '**/*.{js,jsx,tsx,ts,less,md,json}'",
    "test": "umi-test",
    "test:coverage": "umi-test --coverage"
  },
  "gitHooks": {
    "pre-commit": "lint-staged"
  },
  "lint-staged": {
    "*.{js,jsx,less,md,json}": [
      "prettier --write"
    ],
    "*.ts?(x)": [
      "prettier --parser=typescript --write"
    ]
  },
  "dependencies": {
    "@umijs/preset-react": "1.x",
    "@umijs/test": "^3.0.14",
    "lint-staged": "^10.0.7",
    "prettier": "^1.19.1",
    "react": "^16.12.0",
    "react-dom": "^16.12.0",
    "umi": "^3.0.14",
    "yorkie": "^2.0.0"
  }
}
```