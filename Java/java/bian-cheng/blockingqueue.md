# BlockingQueue

BlockingQueue即阻塞队列。

隊列滿，隊列空都會被阻塞。

-	|Throws Exception|Special Value|Blocks|Times Out
--|--|--|--
Insert|add(o)|offer(o)|put(o)|offer(o, timeout, timeunit)
Remove|remove(o)|poll()|	take()|poll(timeout, timeunit)
Examine|element()|peek()	|	||

特點：

1. ThrowsException：如果操作不能马上进行，则抛出异常
2. SpecialValue：如果操作不能马上进行，将会返回一个特殊的值，一般是true或者false
3. Blocks:如果操作不能马上进行，操作会被阻塞
4. TimesOut:如果操作不能马上进行，操作会被阻塞指定的时间，如果指定时间没执行，则返回一个特殊值，一般是true或者false

需要注意的是，我们不能向BlockingQueue中插入null，否则会报NullPointerException。

實現類：

1. ArrayBlockingQueue
2. DelayQueue
3. LinkedBlockingQueue
4. PriorityBlockingQueue
5. SynchronousQueue

