# 订单模块

# 1. 功能	

用户侧：

* 创建订单：需要满足一个订单所需要的信息，如买家，卖家，配送地址，运单号，订单状态，金额，支付方式。
* 订单列表
* 订单状态：不同订单状态下展现	
* 订单明细
* 取消订单：设计到退款流程之类。
* 订单评价：可以依赖于评价模块，也可以内嵌。感觉最好是内嵌。
* 接收订单通知：短信、微信、邮件等。

系统侧：
* 拆单
* 编辑订单
* 取消订单		
* 订单状态
* 订单校验

# 2. 设计

## 2.1 订单号

常规来讲，订单号都需要比较规律的ID。

* 规律Id		

## 2.2 订单状态

订单状态包括了订单的生命周期，也是整个状态机的反应。

遵循两个原则：
* 大状态 + 小状态
* 运送流程 + 售后流程

常见状态：
* 下单（Created）
 * 已创建待支付
 * 创建失败取消
* 买家付款（Paied）
 * 已付款待发货
 * 付款中
 * 付款确认中
 * 完成付款
 * 付款取消订单
 * 过期未支付取消
* 卖家发货（Shipped）
 * 已发货待收货
* 买家收货（Finished）
 * 已收货
 * 运送流程完毕
* 退货（Refunded）
 * 此处已经进入了售后环节
* 已关闭（Closed）
* 已取消

## 2.3 冗余

主要是部分信息的冗余，可以加速查询

* 商品
* 支付信息
* 金额，优惠信息
* 买家
* 物流信息

## 2.4 校验

下单过程是一系列规则的校验。

对于规则的组织顺序以及效率需要考虑。

* 规则：订单规则的设计最好能够具备独立配置能力，尽量满足OCP + Configuable + Priority。
* 上下文：校验应该提供一个上下文，将需要校验的信息均组装在一个大对象中，这样就能链式校验。
* 效率：对于一些静态规则，尽量缓存起来，且提高优先级，同时也要考虑其缓存时效。

## 2.5 通知

将订单状态的变更，尽量进行事件发布。

* 短信通知
* 邮件通知
* 订单日志

## 2.6 订单聚合

有订单的通知，可以引出订单聚合架构，在微服务体系中，多产品先的订单，汇聚到个人中心。

* 定义订单报文结构
* 指定消息目的
* 进行订单推送

## 2.7 订单日志

是一种业务回滚机制的实现。

每次操作订单的信息，均把整个JSON存储起来，万一业务出现了问题，用JSON进行回滚。系统侧，可以提供高级回滚功能。
						
# 3. 抽象设计

## 3.1 API 层

* getOrderList		
* createOrder		
* cancelOrder		
* getOrderDetail		
* makeComment		

## 3.2 服务层

* OrderService	
 * getOrder	
 * getOrderList	
 * cancelOrder	
 * createOrder	
 * updateOrder	
* Events	
 * OrderCreated	
 * OrderUpdated	
 * OrderCancelled	
 * StatusChange	
* VO
 * ProductVO/ItemVO	
 * PaymentVO	
 * ShippingVO	
 * BuyerVO	
 * AmountVO：Amount，Discount
 * Status	

## 3.3 存储

关系数据库：
* order表		
* order_item表		
* order_shipping表
* order_logs表

ES：可以存储订单详情

Redis：可以缓存未完成的订单信息，按照T+1的方式。