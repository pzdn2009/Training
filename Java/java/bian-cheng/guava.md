# Guava

* 可变集合
* Preconditions类
* EventBus

## 可变集合
可变集合类型|可变集合源：JDK or Guava?|Guava不可变集合
--|--|--
Collection|JDK|ImmutableCollection
List|JDK|ImmutableList
Set|JDK|ImmutableSet
SortedSet/NavigableSet|JDK|ImmutableSortedSet
Map|JDK|ImmutableMap
SortedMap|JDK|ImmutableSortedMap
Multiset|Guava|ImmutableMultiset
SortedMultiset|Guava|ImmutableSortedMultiset
Multimap|Guava|ImmutableMultimap
ListMultimap|Guava|ImmutableListMultimap
SetMultimap|Guava|ImmutableSetMultimap
BiMap|Guava|ImmutableBiMap
ClassToInstanceMap|Guava|ImmutableClassToInstanceMap
Table|Guava|ImmutableTable

- Collection。集合，子接口，Set、List。
- List。列表，實現類：LinkedList、Vector、ArrayList。
- Set。二叉樹。實現類：HashSet、LinkedHashSet。
 - 子接口：SortSet，TreeSet。
- SortedSet。排序集。
- Map。哈希表。

## Preconditions类

参数门卫。

1. checkArgument(Boolean)。

   功能描述：检查boolean是否为真。 用作方法中检查参数
   失败时抛出的异常类型: IllegalArgumentException
2. checkNotNull

   功能描述：检查value不为null， 直接返回value；
   失败时抛出的异常类型：NullPointerException
3. checkState。检查迭代器状态
4. checkElementIndex。索引范围是否对。 IndexOutOfBoundsException
5. checkPositionIndex。IndexOutOfBoundsException
6. checkPositionIndexes。IndexOutOfBoundsException

```java
public PostExample(final String title, final Date date, final String author) {  
    this.title = checkNotNull(title);  
    this.date = checkNotNull(date);  
    this.author = checkNotNull(author);  
} 

Preconditions.checkArgument(count > 0, "must be positive: %s", count);  

```

## EventBus

Guava中EventBus是一个消息处理总线，基于观察者模式设计和实现。
EventBus主要分为两种，一种是同步消息总线（EventBus）；另一种是异步消息总线（AsyncEventBus）。
方法：
* register。注册一个事件
* unregister。取消一个事件
* post。发送一个事件数据

spring中使用EventBus
```java
@Component
public class EventBusManager {

    private List<EventHandler<?>> handlers;

    @Autowired
    public EventBusManager(PaymentLogHandler paymentLogHandler) {
        handlers = new ArrayList<>();
        handlers.add(new NoOpHandler());
        handlers.add(new TimeoutHandler());
        handlers.add(paymentLogHandler);
    }

    @Bean
    public AsyncEventBus getEventBus() {
        var asyncEventBus = new AsyncEventBus(MpgConst.MPG_GATEWAY_NAME,Executors.newCachedThreadPool());

        handlers.forEach(zw -> {
            asyncEventBus.register(zw);
        });

        return asyncEventBus;
    }
}

@Slf4j
@Component
public class PLogHandler implements EventHandler<PaymentLog> {

    private PLogRepository repository;

    @Autowired
    public PLogHandler(PLogRepository repository) {
        this.repository= repository;
    }

    @Subscribe
    @Override
    public void handle(PaymentLog event) {
        try {
            repository.save(event);
        } catch (Exception e) {
            log.debug("寫數據失敗");
        }
    }
}
```

REF:http://blog.csdn.net/yxp20092010/article/details/46537333