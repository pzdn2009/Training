# 各种 dependencies

* devDependencies


# 1. devDependencies

开发应用时所依赖的工具包。
通常是一些开发、测试、打包工具，例如 webpack、ESLint、Mocha。

应用正常运行并不依赖于这些包，用户在使用 npm install 安装你的包时也不会安装这些依赖。

```
npm install --save-dev
```

```json
  "name": "webpack-react-express",
  "version": "0.2.0",
  "private": true,
  "dependencies": {
    "antd": "^2.13.11",
    "babel-polyfill": "^6.26.0",
    "base-64": "^0.1.0",
    "bluebird": "^3.5.1",
    "css-loader": "^0.28.7",
    "echarts": "^3.7.2",
  },
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^6.4.1",
    "babel-plugin-transform-class-properties": "^6.24.1",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-polyfill": "^6.26.0",
    "babel-preset-es2015": "^6.24.1",
    "webpack": "^1.12.13",
    "webpack-hot-middleware": "^2.21.0"
  },
```

本地安装：
```
npm install packagename -dev
```