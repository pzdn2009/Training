# npm-scripts

* npm run 
* npm run-script
* 智能路径：node_module/.bin：
* `npm start * npm test`，内置
* 支持bash脚本：&& ,| , & ，--指定参数
* npm-run-all：运行多个脚本的插件
* pre- post-，两个钩子，Eg：pretest,posttest

```shell
npm run

Lifecycle scripts included in ant-design-pro:
  start //内置
    umi dev
  test  //内置
    umi test

available via `npm run-script`:
  analyze
    cross-env ANALYZE=1 umi build
  build
    umi build
  deploy
    npm run site && npm run gh-pages
  dev
    npm run start:dev
  fetch:blocks
    pro fetch-blocks && npm run prettier
  gh-pages
    cp CNAME ./dist/ && gh-pages -d dist
  i18n-remove
    pro i18n-remove --locale=zh-CN --write
  lint
    npm run lint:js && npm run lint:style && npm run lint:prettier
  lint-staged
    lint-staged
  lint-staged:js
    eslint --ext .js,.jsx,.ts,.tsx 
  lint:fix
    eslint --fix --cache --ext .js,.jsx,.ts,.tsx --format=pretty ./src && npm run lint:style
  lint:js
    eslint --cache --ext .js,.jsx,.ts,.tsx --format=pretty ./src
  lint:prettier
    prettier --check "**/*" --end-of-line auto
  lint:style
    stylelint --fix "src/**/*.less" --syntax less
  prettier
    prettier -c --write "**/*"
  start:dev
    cross-env REACT_APP_ENV=dev MOCK=none umi dev
  start:no-mock
    cross-env MOCK=none umi dev
  start:no-ui
    cross-env UMI_UI=none umi dev
  start:pre
    cross-env REACT_APP_ENV=pre MOCK=none umi dev
  start:test
    cross-env REACT_APP_ENV=test MOCK=none umi dev
  test:all
    node ./tests/run-tests.js
  test:component
    umi test ./src/components
  tsc
    tsc
  ui
    umi ui
```

## 常见脚本命令

* start
* test
* dev
* server
* prod
* help
* docs