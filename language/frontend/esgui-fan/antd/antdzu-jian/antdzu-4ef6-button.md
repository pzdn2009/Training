# Antd-组件-Button

在 Ant Design 中，我们有四种按钮。

* 主按钮：用于主行动点，一个操作区域只能有一个主按钮。
* 默认按钮：用于没有主次之分的一组行动点。
* 虚线按钮：常用于添加操作。
* 链接按钮：用于次要或外链的行动点。

以及四种状态属性与上面配合使用。

* 危险：删除/移动/修改权限等危险操作，一般需要二次确认。
* 幽灵：用于背景色比较复杂的地方，常用在首页/产品页等展示场景。
* 禁用：行动点不可用的时候，一般需要文案解释。
* 加载中：用于异步操作等待反馈的时候，也可以避免多次提交。


## 属性

* loading
* diabled
* danger
* block
* ghost
* size: small,middle,large
* type: primary,link,dashed
* icon: `<SearchOutlined />`
* `<DownOutlined />`overlay={menu}

## api

通过设置 Button 的属性来产生不同的按钮样式，推荐顺序为：type -> shape -> size -> loading -> disabled。

![](/assets/antd-Button.png)

## 多个按钮组合

```js
import { Button, Menu, Dropdown } from 'antd';
import { DownOutlined } from '@ant-design/icons';

function handleMenuClick(e) {
  console.log('click', e);
}

//定义Menu
const menu = (
  <Menu onClick={handleMenuClick}>
    <Menu.Item key="1">1st item</Menu.Item>
    <Menu.Item key="2">2nd item</Menu.Item>
    <Menu.Item key="3">3rd item</Menu.Item>
  </Menu>
);

ReactDOM.render(
  <div>
    <Button type="primary">primary</Button>
    <Button>secondary</Button>
    //下拉框
    <Dropdown overlay={menu}>
      //按钮
      <Button>
        Actions <DownOutlined /> //剩余的
      </Button>
    </Dropdown>
  </div>,
  mountNode,
);
```