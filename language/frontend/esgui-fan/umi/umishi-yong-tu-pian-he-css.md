# Umi-使用图片和CSS

# 1. CSS

* 全局样式
* CSS Modules
* CSS预处理器

## 1.1 全局样式

Umi 中约定 `src/global.css` 为全局样式，如果存在此文件，会被自动引入到入口文件最前面。

比如用于覆盖样式，

```css
.ant-select-selection {
  max-height: 51px;
  overflow: auto;
}
```

## 1.2 CSS Modules

Umi 会自动识别` CSS Modules` 的使用，你把他当做 CSS Modules 用时才是 CSS Modules。
```js
// CSS Modules
import styles from './foo.css';

// 非 CSS Modules
import './foo.css';
```

Umi 内置支持 `less`，不支持 sass 和 stylus。

# 2. 使用图片

## 2.1 JS 里使用图片

* 通过 `require` 引用**相对路径**的图片。
* 支持别名，比如通过 `@ 指向 src` 目录：


比如：
```js
export default () => <img src={require('./foo.png')} />
export default () => <img src={require('@/foo.png')} />
```

## 2.2 CSS 里使用图片

* 通过相对路径引用。

比如，

```css
.logo {
  background: url(./foo.png);
}
```

* CSS 里也支持别名，但需要在前面加 ~ 前缀，

```
.logo {
  background: url(~@/foo.png);
}
```
>注意：
* 这是 webpack 的规则，如果切到其他打包工具，可能会有变化
* less 中同样适用

## 2.3 图片路径问题

项目中使用图片有两种方式，

1. 先把图片传到 cdn，然后在 JS 和 CSS 中使用图片的绝对路径
2. 把图片放在项目里，然后在 JS 和 CSS 中通过`相对路径`的方式使用

前者不会有任何问题；后者，如果在 JS 中引用相对路径的图片时，在发布时会根据 `publicPath` 绝对引入路径，所以就算没有开启 dynamicImport 时，也需要注意 publicPath 的正确性。

## 2.4 Base64 编译

通过相对路径引入图片的时候，如果图片小于 10K，会被编译为 Base64，否则会被构建为独立的图片文件。

10K 这个阈值可以通过 `inlineLimit` 配置修改。
