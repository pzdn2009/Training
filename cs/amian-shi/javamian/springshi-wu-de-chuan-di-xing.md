# Spring事务的传递性

* 在同一类方法间相互调用，如果调用方无事务控制，被调用方有事务控制，则被调用方也无事务。——发起原则。
* 事务控制A调用事务控制B，如果B抛异常，A处理异常，则整个事务会回滚，同时报错Transaction rolled back because it has been arked as rollback-only
* 事务控制的方法中异步线是没有事务控制的.

## 原理

