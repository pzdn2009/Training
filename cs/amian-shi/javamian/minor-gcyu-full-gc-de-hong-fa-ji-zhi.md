# Minor GC与Full GC的触发机制

当年轻代需要回收时会触发Minor GC\(也称作Young GC\)。

* Minor GC触发条件：当Eden区满时，触发Minor GC。
* Full GC触发条件：
  * 调用System.gc时，系统建议执行Full GC，但是不必然执行
  * 老年代空间不足
  * 方法去空间不足
  * 通过Minor GC后进入老年代的平均大小大于老年代的可用内存
  * 由Eden区、From Space区向To Space区复制时，对象大小大于To Space可用内存，则把该对象转存到老年代，且老年代的可用内存小于该对象大小。

