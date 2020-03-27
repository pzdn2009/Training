# Umi-配置

* 本地临时配置：`.umirc.local.ts`
* 多环境多份配置：`UMI_ENV`


Umi 在 `.umirc.ts` 或 `config/config.ts` 中配置项目和插件，支持 es6。

```js
export default {
  base: '/docs/',
  publicPath: '/static/',
  hash: true,
  history: {
    type: 'hash',
  },
}

export default defineConfig({
  layout:{},
  routes: [
    { path: '/', component: '@/pages/index' },
  ],
});
```

# 1. 本地临时配置

* config/config.ts 对应的是 `config/config.local.ts`
* .local.ts 是本地验证使用的临时配置，请将其添加到 `.gitignore`，务必不要提交到 git 仓库中
* `.local.ts` 配置的优先级最高，比 `UMI_ENV` 指定的配置更高

# 2. 多环境多份配置

* `.umirc.{env}.ts`
* .env内配置`UMI_ENV={env}`
* 其中，umirc.ts是根