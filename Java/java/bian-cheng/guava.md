# Guava

* 可变集合
* Preconditions类
* EventBus

## 1. What is Guava? 

Guava是一种基于开源的Java库，其中包含谷歌正在由他们很多项目使用的很多核心库。这个库是为了方便编码，并减少编码错误。这个库提供用于集合，缓存，支持原语，并发性，常见注解，字符串处理，I/O和验证的实用方法。

## 2. Preconditions类

参数门卫。

1. checkArgument(Boolean)。

   功能描述：检查boolean是否为真。 用作方法中检查参数
   失败时抛出的异常类型: **IllegalArgumentException**
2. checkNotNull

   功能描述：检查value不为null， 直接返回value；
   失败时抛出的异常类型：**NullPointerException**
3. checkState。检查迭代器状态
4. checkElementIndex。索引范围是否对。 **IndexOutOfBoundsException**
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

## 3. Ordering 

```java
Ordering ordering = Ordering.natural();
Collections.sort(numbers , ordering);

System.out.println("List is sorted: " + ordering.isOrdered(numbers));
System.out.println("Minimum: " + ordering.min(numbers));
System.out.println("Maximum: " + ordering.max(numbers));
```

## 4. Range

可以靈活地表示數學上的區間。

## 5. 可变集合
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

Bimap是一一映射。
```java
import com.google.common.collect.BiMap;
import com.google.common.collect.HashBiMap;

public class GuavaTester {

   public static void main(String args[]){
      BiMap<Integer, String> empIDNameMap = HashBiMap.create();

      empIDNameMap.put(new Integer(101), "Mahesh");
      empIDNameMap.put(new Integer(102), "Sohan");
      empIDNameMap.put(new Integer(103), "Ramesh");

      //Emp Id of Employee "Mahesh"
      System.out.println(empIDNameMap.inverse().get("Mahesh"));
   }	
}
```

Table:
Table代表一个特殊的映射，其中两个键可以在组合的方式被指定为单个值。它类似于创建映射的映射。

```java
import java.util.Map;
import java.util.Set;

import com.google.common.collect.HashBasedTable;
import com.google.common.collect.Table;

public class GuavaTester {

   public static void main(String args[]){
      //Table<R,C,V> == Map<R,Map<C,V>>
      /*
      *  Company: IBM, Microsoft, TCS
      *  IBM 		-> {101:Mahesh, 102:Ramesh, 103:Suresh}
      *  Microsoft 	-> {101:Sohan, 102:Mohan, 103:Rohan } 
      *  TCS 		-> {101:Ram, 102: Shyam, 103: Sunil } 
      * 
      * */
      //create a table
      Table<String, String, String> employeeTable = HashBasedTable.create();

      //initialize the table with employee details
      employeeTable.put("IBM", "101","Mahesh");
      employeeTable.put("IBM", "102","Ramesh");
      employeeTable.put("IBM", "103","Suresh");

      employeeTable.put("Microsoft", "111","Sohan");
      employeeTable.put("Microsoft", "112","Mohan");
      employeeTable.put("Microsoft", "113","Rohan");

      employeeTable.put("TCS", "121","Ram");
      employeeTable.put("TCS", "122","Shyam");
      employeeTable.put("TCS", "123","Sunil");

      //get Map corresponding to IBM
      Map<String,String> ibmEmployees =  employeeTable.row("IBM");

      System.out.println("List of IBM Employees");
      for(Map.Entry<String, String> entry : ibmEmployees.entrySet()){
         System.out.println("Emp Id: " + entry.getKey() + ", Name: " + entry.getValue());
      }

      //get all the unique keys of the table
      Set<String> employers = employeeTable.rowKeySet();
      System.out.print("Employers: ");
      for(String employer: employers){
         System.out.print(employer + " ");
      }
      System.out.println();

      //get a Map corresponding to 102
      Map<String,String> EmployerMap =  employeeTable.column("102");
      for(Map.Entry<String, String> entry : EmployerMap.entrySet()){
         System.out.println("Employer: " + entry.getKey() + ", Name: " + entry.getValue());
      }		
   }	
}
```

## 6. Loading Cache

Guava通过接口LoadingCache提供了一个非常强大的基于内存的LoadingCache<K，V>。在缓存中自动加载值，它提供了许多实用的方法，在有缓存需求时非常有用。

```java
LoadingCache employeeCache = 
         CacheBuilder.newBuilder()
            .maximumSize(100) // maximum 100 records can be cached
            .expireAfterAccess(30, TimeUnit.MINUTES) // cache will expire after 30 minutes of access
            .build(new CacheLoader(){ // build the cacheloader
               @Override
               public Employee load(String empId) throws Exception {
                  //make the expensive call
                  return getFromDatabase(empId);
               }							
            });
```

## 7. EventBus

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