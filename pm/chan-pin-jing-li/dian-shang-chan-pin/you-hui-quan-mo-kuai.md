# 优惠券模块 

# 1. 功能

用户侧：
* 领券：需要判断是否可领。主动领取，被动push。
* 优惠券列表：三态（未使用，已使用，已过期）
* 使用
* 回退

系统侧：
* 优惠券定义：创建、修改、删除、发布
* 优惠券规则：过期时间，面值，面值计算规则，适用条件集

报表侧：
* 优惠券使用记录 
* 优惠券拉新

# 2. 设计

## 2.1 优惠券三态

深入一点做，即是一个TCC模式。
* Use：使用
* UnUse：未使用
* Void：自动取消

分布式环境中，可以结合消息队列来做。
* 使用GTS框架
* 公布 UseEvent
* 公布 UnUseEvent
* 公布 VoidEvent

## 2.2 优惠券面值计算

* 满减
* 立减
* 折扣

## 2.3 优惠券ID生成策略

如果需要对优惠券ID进行生成

* 自增ID
* 规律ID，可以结合分布式唯一ID生成策略，比如RedisID
* 任意ID，如UUID，Idworker

## 2.4 可领规则

在业务场景中，要判断是否可以领取

* 首先得是会员
* 其次，各个规则集，要参与进来，可以使用规则引擎来做，轻量级就写死，可配置就做成DSL；

## 2.5 使用规则

使用的产品得符合场景。

* 会员
* 产品在使用规则当中

## 2.6 时效性

及时：
* 扣减（使用）
* 领取
* 规则校验

定时：
* 异步回退
* ID生成缓存

# 3. 抽象设计

## 3.1 API 层

用户侧：
* getCouponList
* receiveCoupon
* tryUseCoupon
* confirmCoupon
* cancelCoupon
* autoCanelCoupon

系统侧：
* crud
* addReceiveRules
* addUseRules
* publishCoupon
* couponUsedReport
* couponNewMemberReport

## 3.2 服务层

* CouponService
 * tryUse
 * tryUse(facts)
 * confirm
 * canel
 * autoCancel
 * pushCoupon
* Events
 * use
 * confirm
 * cancel
 
## 3.3 存储层

* coupon表：实际的优惠券
* coupon_member表：即领券表
* coupon_tcc表：优惠券使用记录表
* coupon_logs表：操作日志

