# Umi-环境变量

# 1. 添加环境变量的方式

* 执行命令时添加
* 在 `.env` 文件中定义

```
# 执行命令
$ PORT=3000 umi dev

# .env
PORT=3000
BABEL_CACHE=none
```

# 2. 环境变量列表

* APP_ROOT：指定项目根目录。
* ANALYZE
* BABEL_POLYFILL：默认会根据 targets 配置打目标浏览器的全量补丁，设置为 none 禁用内置的补丁方案。
* COMPRESS：默认压缩 CSS 和 JS，值为 none 时不压缩，build 时有效。
* FORK_TS_CHECKER
* FRIENDLY_ERROR
* HMR：设为 none 时禁用代码热更新功能
* HTML：设为 none 时不输出 HTML，umi build 时有效
* HOST：默认是 0.0.0.0
* PORT：指定端口号，默认是 8000。
* PROGRESS
* UMI_ENV：指定不同环境各自的配置文件
* WATCH：设为 none 时不监听文件变更