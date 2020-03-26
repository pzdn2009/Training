# React-JSX

* javascript-xml
 * 必须以<>包裹
 * 只能有一个Root元素，树形
 * 标签：小写为dom标签
 * 标签：大写为组件
 * 本质上是一种DSL
 * 需要使用babel来进行解析，解析的结果为js代码，createElement
 * 可以直接引入`<text/babel>`

* 表达式：{}
 * 遇到{}进行解析
 * style：style={myStyle}，style={{}}绑定多个
 * 数组{arr}
 * 嵌套的
 
# 1. 转换示例

转化前：
```jsx
<div id="div" className="div" key="div">
  <div>
    <span>2</span>
  </div>
  <span>1</span>
  <span>1</span>
</div>
```

转化后：
```js
"use strict";
 
React.createElement("div", {
    id: "div",
    className: "div",
    key: "div"
  }, 
  React.createElement("div", null, React.createElement("span", null, "2")), 
  React.createElement("span", null, "1"), React.createElement("span", null, "1")
);
```

转换前：
```jsx
//组件
function Comp() {
	return <div>111</div>
}
 
<Comp id="div" className="div" key="div">
  <span>222</span>
  <span>222</span>
</Comp>
```

转换后：
```
"use strict";
 
function Comp() {
  return React.createElement("div", null, "111");
}
 
React.createElement(Comp, {
    id: "div",
    className: "div",
    key: "div"
  }, 
  React.createElement("span", null, "222"), 
  React.createElement("span", null, "222")
);
```

# 2. Render JSX

React 中的模板语法：JSX。它允许js代码中直接使用HTML标签。

```javascript
<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
```

# 3. Use JavaScript in JSX

&lt; 符号作为开始HTML，{ 开始JS代码。

```javascript
<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      var names = ['Alice', 'Emily', 'Kate'];

      ReactDOM.render(
        <div>
        {
          names.map(function (name) {
            return <div>Hello, {name}!</div>
          })
        }
        </div>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
```

