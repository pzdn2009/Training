# 支付模块

支付方式：
* 微信
* 支付宝
* 钱包
* 银联
* 信用卡
* APP、H5、小程序、WEB、线下

# 1. 功能	

用户侧：

* 去支付：即进入支付页面
* 选择支付方式：提供列表选择
* 提交：确认
* 跳转：支付成功或者失败的跳转
* 退款
* 查询

系统侧：
* 支付单：如果做得通用一点，即需要专门来生成一个与业务系统无关的支付订单，专门关注支付。
* 支付方式配置：对于不同应用，不用产品，有可能配置不同的支付方式。
* 过期时间设置
* 支付回调：如支付宝，微信
* 主动查询

# 2. 设计

## 2.1 安全性

* 支付模块如果是独立的服务，则可采用客户端证书的方式去调用；
* 对外网采用RSA
* 内网采用对称
* API的调用加上签名机制
* 存储排除敏感信息

* 对于三方系统过来的报文，要做checkSign。

## 2.2 信用卡

支付状态：
* 预授权 Auth
* 扣款：Capture
* 直接支付：Pay
* 退款：Refund
* 取消：Void
* 取消预授权：VoidAuth

信用卡方式：
* 3D
* 非3D

信用卡信息：
* 卡号
* 有效期
* CVV
* 持卡人姓名

## 2.3 风控系统

风险值：

* 高
* 中
* 低

## 2.4 支付事件

支付成功或者失败，均靠支付事件进行通知，以解耦业务模块。
比如更新订单。 
																								
# 3. 抽象设计

## 3.1 API 层

* callBack
 * aliCallback
 * wexinCallback
 * unionCallback
* createPayOrder：对于独立支付系统可以这么做
* paySubmit：提交支付
* getPayStatus：在有效期内查询支付状态
* getPayDetails：查询支付结果
* getPaymentConfig：获取支付配置，首页显示有用

## 3.2 服务层

* PayService	
 * createPayOrder	
 * getPayStatus
 * paySubmit
 * payDetails	
 * refund：退款
 * cancel：取消支付，比如信用卡模式
 * autoQueryTask
* Events	
 * PaySubmitted	
 * Refunded	
 * Cancelled
 * PaySuccess

## 3.3 存储

关系数据库：
* pay_order表		
* pay_config表		
* pay_merchant表
* pay_transactions表
* pay_logs表

