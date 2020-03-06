# Synchronized与Lock的区别与应用场景

* 可中断锁（synchronized就不是可中断锁，而Lock是可中断锁）：在等待获取锁过程中可中断。

| 类别 | synchronized | Lock |
| :--- | :--- | :--- |
| 锁状态 | 无法判断 | 可以判断 |
| 锁类型 | 可重入 不可中断 非公平 | 可重入 可判断 可公平/公平 |
| 灵活性 | 不可控 | 可以灵活地参数控制。 |
