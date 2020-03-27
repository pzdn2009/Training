# Umi-插件

* 插件id和key
* 启用插件




# 1. 插件id和key

每个插件都会对应一个 id 和一个 key，
* id 是路径的简写，
* key 是进一步简化后用于配置的唯一值。

# 2. 启用插件

* package.json依赖
>package.json中：
包含插件和插件集，以
>* `@umijs/preset-`
>* `@umijs/plugin-`
>* `umi-preset-`
>* `umi-plugin-` 
开头的依赖会被自动注册为插件或插件集。
* 配置
>在配置里可通过 presets 和 plugins 配置插件
```js
export default {
  presets: ['./preset', 'foo/presets'],
  plugins: ['./plugin'],
}
```
* 环境变量

# 3. 检查插件注册情况

```
$ umi plugin list

# 顺便看看他们分别用了哪些 key
$ umi plugin list --key
```

```js
可通过 api.hasPlugins(pluginId[]) 
和 api.hasPresets(pluginId[]) 的方式感知其他插件
```

# 4. 禁用插件

* 配置 key 为 false
* 在插件里禁用其他插件：`api.skipPlugins(pluginId[])` 的方式禁用

# 5. 配置插件

```js
export default {
  mock: { exclude: ['./foo'] }
}
```
