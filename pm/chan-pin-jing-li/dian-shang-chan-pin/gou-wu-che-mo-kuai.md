# 购物车模块

# 1. 功能

用户侧：
* 未登录用户添加到购物车
* 已登录用户添加到购物车
* 删除购物车的商品
* 清空购物车
* 查询购物车列表
* 更改购物车的商品数量
* 根据选中的商品进行结算（支付）

系统侧：
* 未登录用户到登录用户的切换，合并购物车

# 2. 设计

## 2.1 未登录处理

* 未登录添加到Cookie
* 登录成功后，将Cookie里数据合并，持久化

# 3. 抽象设计

## 3.1 API 层

用户侧：
* getCartList：获取购物车列表
* updateCartItemNumber：修改购物车数量
* deleteCartItem：删除购物车项
* clearCart：清空购物车
* addItemToCart：添加到购物车
* toOrder：去下单

## 3.2 服务层

* CartService
* Events
 * addToCart
 * toOrder
 * changeNumer
 
## 3.3 存储层

* cart表：存储购物车
* redis存储：
 * hash
 * 用户ID作为key，商品ID，数量（key,value）
 * 满足增删查改

