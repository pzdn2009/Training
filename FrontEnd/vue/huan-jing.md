# Vue 環境

## 瀏覽器插件

安裝Devtools，啟用，允許訪問URL。

https://github.com/vuejs/vue-devtools#vue-devtools

## npm
使用
淘寶鏡像
```
// 配置后可通过下面方式来验证是否成功 npm 
npm config set registry https://registry.npm.taobao.org 
config get registry // 或 npm info express
```

或者使用cnpm
```
npm install -g cnpm --registry=https://registry.npm.taobao.org // 使用
cnpm install express
```

_ps: 不要在git bash 裡面使用npm install ，被坑了一把_

```
# 全局安装 vue-cli 
$ npm install --global vue-cli 

# 创建一个基于 webpack 模板的新项目 
$ vue init webpack my-project 

# 安装依赖，走你 
$ cd my-project 
$ npm install 
$ npm run dev
```
