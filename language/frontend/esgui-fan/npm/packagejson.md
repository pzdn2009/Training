# package.json


Ref：http://caibaojian.com/npm/files/package.json.html

```json
{
  "name": "ant-design-pro", //包名
  "version": "1.0.0",//包的版本号
  "private": true, //不发布
  "description": "An out-of-box UI solution for enterprise applications",//包描述
  "scripts": { //脚本
    "analyze": "cross-env ANALYZE=1 umi build",
    "build": "umi build",
    "deploy": "npm run site && npm run gh-pages",
    "dev": "npm run start:dev",
    "fetch:blocks": "pro fetch-blocks && npm run prettier",
    "gh-pages": "cp CNAME ./dist/ && gh-pages -d dist",
    "i18n-remove": "pro i18n-remove --locale=zh-CN --write",
    "lint": "npm run lint:js && npm run lint:style && npm run lint:prettier",
    "lint-staged": "lint-staged",
    "lint-staged:js": "eslint --ext .js,.jsx,.ts,.tsx ",
    "lint:fix": "eslint --fix --cache --ext .js,.jsx,.ts,.tsx --format=pretty ./src && npm run lint:style",
    "lint:js": "eslint --cache --ext .js,.jsx,.ts,.tsx --format=pretty ./src",
    "lint:prettier": "prettier --check \"**/*\" --end-of-line auto",
    "lint:style": "stylelint --fix \"src/**/*.less\" --syntax less",
    "prettier": "prettier -c --write \"**/*\"",
    "start": "umi dev",
    "start:dev": "cross-env REACT_APP_ENV=dev MOCK=none umi dev",
    "start:no-mock": "cross-env MOCK=none umi dev",
    "start:no-ui": "cross-env UMI_UI=none umi dev",
    "start:pre": "cross-env REACT_APP_ENV=pre MOCK=none umi dev",
    "start:test": "cross-env REACT_APP_ENV=test MOCK=none umi dev",
    "test": "umi test",
    "test:all": "node ./tests/run-tests.js",
    "test:component": "umi test ./src/components",
    "tsc": "tsc",
    "ui": "umi ui"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint-staged"
    }
  },
  "lint-staged": {
    "**/*.less": "stylelint --syntax less",
    "**/*.{js,jsx,ts,tsx}": "npm run lint-staged:js",
    "**/*.{js,jsx,tsx,ts,less,md,json}": [
      "prettier --write"
    ]
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 10"
  ],
  "dependencies": { //生产依赖包
    "@ant-design/icons": "^4.0.0-alpha.19",
    
    //升级[4.10.12,5.0.0)
    "@ant-design/pro-layout": "^4.10.13",
    
    //升级[0.11.0,0.12.0)
    "@antv/data-set": "^0.11.0",
    "@antv/l7": "^2.0.0",
    "@antv/l7-maps": "^2.0.0",
    "@types/lodash.debounce": "^4.0.6",
    "@types/lodash.isequal": "^4.5.5",
    "@types/react-router": "^5.0.2",
    "antd": "^3.23.6",
    "bizcharts": "^3.5.3-beta.0",
    "bizcharts-plugin-slider": "^2.1.1-beta.1",
    "classnames": "^2.2.6",
    "dva": "^2.6.0-beta.16",
    "gg-editor": "^2.0.2",
    "lodash": "^4.17.11",
    "lodash-decorators": "^6.0.0",
    "lodash.debounce": "^4.0.8",
    "lodash.isequal": "^4.5.0",
    "mockjs": "^1.0.1-beta3",
    "moment": "^2.24.0",
    "numeral": "^2.0.6",
    "nzh": "^1.0.3",
    "omit.js": "^1.0.2",
    "path-to-regexp": "2.4.0",
    "prop-types": "^15.5.10",
    "qs": "^6.9.0",
    "react": "^16.8.6",
    "react-copy-to-clipboard": "^5.0.1",
    "react-dom": "^16.8.6",
    "react-fittext": "^1.0.0",
    "react-helmet": "^5.2.1",
    "react-router": "^4.3.1",
    "redux": "^4.0.1",
    "umi": "^2.13.0",
    "umi-plugin-antd-theme": "^1.0.1",
    "umi-plugin-pro-block": "^1.3.2",
    "umi-plugin-react": "^1.9.5",
    "umi-request": "^1.0.8"
  },
  "devDependencies": { //开发依赖包
    "@ant-design/pro-cli": "^1.0.18",
    "@types/classnames": "^2.2.7",
    "@types/express": "^4.17.0",
    "@types/history": "^4.7.2",
    "@types/jest": "^25.1.0",
    "@types/lodash": "^4.14.144",
    "@types/qs": "^6.5.3",
    "@types/react": "^16.8.19",
    "@types/react-dom": "^16.8.4",
    "@types/react-helmet": "^5.0.13",
    "@umijs/fabric": "^2.0.2",
    "chalk": "^3.0.0",
    "cross-env": "^7.0.0",
    "cross-port-killer": "^1.1.1",
    "enzyme": "^3.9.0",
    "express": "^4.17.1",
    "gh-pages": "^2.0.1",
    "husky": "^4.0.7",
    "jest-puppeteer": "^4.2.0",
    "jsdom-global": "^3.0.2",
    "lint-staged": "^10.0.0",
    "mockjs": "^1.0.1-beta3",
    "node-fetch": "^2.6.0",
    "prettier": "^1.19.1",
    "pro-download": "1.0.1",
    "stylelint": "^13.0.0",
    "umi-plugin-antd-icon-config": "^1.0.2",
    "umi-plugin-ga": "^1.1.3",
    "umi-plugin-pro": "^1.0.2",
    "umi-types": "^0.5.0"
  },
  "optionalDependencies": {
    "puppeteer": "^2.0.0"
  },
  "engines": { //node的版本
    "node": ">=10.0.0"
  },
  "checkFiles": [
    "src/**/*.js*",
    "src/**/*.ts*",
    "src/**/*.less",
    "config/**/*.js*",
    "scripts/**/*.js"
  ]
}
```