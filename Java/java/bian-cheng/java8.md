# Java 8

- lambda
- 接口的默认方法与静态方法
- 方法引用
- 重复注解
- 更好的类型推测机制
- 参数名字
- Optional
- Stream
- Date/Time API (JSR 310)
- Base64
- 并行（parallel）数组
- 并发（Concurrency）

## lambda
```java
Arrays.asList( "a", "b", "d" ).forEach( e -> System.out.println( e ) );

Arrays.asList( "a", "b", "d" ).sort( ( e1, e2 ) -> {
    int result = e1.compareTo( e2 );
    return result;
} );
```

## 默認方法

類似C# virtual

```java
private interface Defaulable {
    // Interfaces now allow default methods, the implementer may or 
    // may not implement (override) them.
    default String notRequired() { 
        return "Default implementation"; 
    }        
}
         
private static class DefaultableImpl implements Defaulable {
}
     
private static class OverridableImpl implements Defaulable {
    @Override
    public String notRequired() {
        return "Overridden implementation";
    }
}
```

## 靜態方法

```java
private interface DefaulableFactory {
    // Interfaces now allow static methods
    static Defaulable create( Supplier< Defaulable > supplier ) {
        return supplier.get();
    }
}

public static void main( String[] args ) {
    Defaulable defaulable = DefaulableFactory.create( DefaultableImpl::new );
    System.out.println( defaulable.notRequired() );
         
    defaulable = DefaulableFactory.create( OverridableImpl::new );
    System.out.println( defaulable.notRequired() );
}
```
## 方法引用

方法引用提供了非常有用的语法，可以直接引用已有Java类或对象（实例）的方法或构造器。
```java
public static class Car {
    public static Car create( final Supplier< Car > supplier ) {
        return supplier.get();
    }              
         
    public static void collide( final Car car ) {
        System.out.println( "Collided " + car.toString() );
    }
         
    public void follow( final Car another ) {
        System.out.println( "Following the " + another.toString() );
    }
         
    public void repair() {   
        System.out.println( "Repaired " + this.toString() );
    }
}
```

第一种方法引用是构造器引用，它的语法是**Class::new**，或者更一般的**Class< T >::new**。请注意构造器没有参数。
```java
final Car car = Car.create( Car::new );
final List< Car > cars = Arrays.asList( car );
```

第二种方法引用是静态方法引用，它的语法是**Class::static_method**。请注意这个方法接受一个Car类型的参数。
```java
cars.forEach( Car::collide );
```

第三种方法引用是特定类的任意对象的方法引用，它的语法是**Class::method**。请注意，这个方法没有参数。
```java
cars.forEach( Car::repair );
```
最后，第四种方法引用是特定对象的方法引用，它的语法是instance::method。请注意，这个方法接受一个Car类型的参数

```java
final Car police = Car.create( Car::new );
cars.forEach( police::follow );
```
运行上面的Java程序在控制台上会有下面的输出（Car的实例可能不一样）：
```
Collided com.javacodegeeks.java8.method.references.MethodReferences$Car@7a81197d
Repaired com.javacodegeeks.java8.method.references.MethodReferences$Car@7a81197d
Following the com.javacodegeeks.java8.method.references.MethodReferences$Car@7a81197d
```

## Optional

類似 Nullable

APIS：
* public<U> Optional<U> map(Function<? super T, ? extends U> mapper)

map 是可能无限级联的, 比如再深一层, 获得用户名的大写形式

```java
return user.map(u -> u.getOrders()).orElse(Collections.emptyList())

return user.map(u -> u.getUsername())
           .map(name -> name.toUpperCase())
           .orElse(null);
```
* public T **orElse**(T other)
存在即返回, 无则提供默认值。
```java
return user.orElse(null);  //而不是 return user.isPresent() ? user.get() : null;
return user.orElse(UNKNOWN_USER);
```
* public T orElseGet(Supplier<? extends T> other)
   通过回调函数来产生一个默认值：
```java
orElseGet( () -> "[none]" ) ); 
```
* public void ifPresent(Consumer<? super T> consumer)

   isPresent()來判斷是否為空，true非空；
* public Optional<T> filter(Predicate<? super T> predicate)
* public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper)
* public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X
* Optional.ofNullable( null )
  它以一种智能的, 宽容的方式来构造一个 Optional 实例. 传 null 进到就得到 Optional.empty(), 非 null 就调用 Optional.of(obj).
* Optional.of("Tom")。
   
   它要求传入的 obj 不能是 null 值的

```java
Optional< String > fullName = Optional.ofNullable( null );
System.out.println( "Full Name is set? " + fullName.isPresent() );        
System.out.println( "Full Name: " + fullName.orElseGet( () -> "[none]" ) ); 
System.out.println( fullName.map( s -> "Hey " + s + "!" ).orElse( "Hey Stranger!" ) );
```

使用任何像 Optional 的类型作为字段或方法参数都是不可取的. Optional 只设计为类库方法的, 可明确表示可能无值情况下的返回类型. Optional 类型不可被序列化, 用作字段类型会出问题的。


## Stream

一串支持连续、并行聚集操作的元素。
```java
// Calculate total points of all active tasks using sum()
final long totalPointsOfOpenTasks = tasks
    .stream()
    .filter( task -> task.getStatus() == Status.OPEN )
    .mapToInt( Task::getPoints )
    .sum();
         
System.out.println( "Total points: " + totalPointsOfOpenTasks );


Total points (all tasks): 26.0
```

_怎麼感覺像極了Linq。_
我们需要按照某种准则来对集合中的元素进行分组。
```java
// Group tasks by their status
final Map< Status, List< Task > > map = tasks
    .stream()
    .collect( Collectors.groupingBy( Task::getStatus ) );
System.out.println( map );

{CLOSED=[[CLOSED, 8]], OPEN=[[OPEN, 5], [OPEN, 13]]}
```

让我们来计算整个集合中每个task分数（或权重）的平均值来结束task的例子。
```java
// Calculate the weight of each tasks (as percent of total points) 
final Collection< String > result = tasks
    .stream()                                        // Stream< String >
    .mapToInt( Task::getPoints )                     // IntStream
    .asLongStream()                                  // LongStream
    .mapToDouble( points -> points / totalPoints )   // DoubleStream
    .boxed()                                         // Stream< Double >
    .mapToLong( weigth -> ( long )( weigth * 100 ) ) // LongStream
    .mapToObj( percentage -> percentage + "%" )      // Stream< String> 
    .collect( Collectors.toList() );                 // List< String > 
         
System.out.println( result );

[19%, 50%, 30%]
```

## Date/Time API (JSR 310)

第一个是Clock类，它通过指定一个时区，然后就可以获取到当前的时刻，日期与时间。Clock可以替换System.currentTimeMillis()与TimeZone.getDefault()。

```java
// Get the system clock as UTC offset 
final Clock clock = Clock.systemUTC();
System.out.println( clock.instant() );
System.out.println( clock.millis() );

2014-04-12T15:19:29.282Z
1397315969360
```

```java
// Get the local date and local time
final LocalDate date = LocalDate.now();
final LocalDate dateFromClock = LocalDate.now( clock );
         
System.out.println( date );
System.out.println( dateFromClock );
         
// Get the local date and local time
final LocalTime time = LocalTime.now();
final LocalTime timeFromClock = LocalTime.now( clock );
         
System.out.println( time );
System.out.println( timeFromClock );

2014-04-12
2014-04-12
11:25:54.568
15:25:54.568
```

```java
// Get the local date/time
final LocalDateTime datetime = LocalDateTime.now();
final LocalDateTime datetimeFromClock = LocalDateTime.now( clock );
         
System.out.println( datetime );
System.out.println( datetimeFromClock );

2014-04-12T11:37:52.309
2014-04-12T15:37:52.309
```

让我们看一下Duration类：在秒与纳秒级别上的一段时间。Duration使计算两个日期间的不同变的十分简单。
```java
// Get duration between two dates
final LocalDateTime from = LocalDateTime.of( 2014, Month.APRIL, 16, 0, 0, 0 );
final LocalDateTime to = LocalDateTime.of( 2015, Month.APRIL, 16, 23, 59, 59 );
 
final Duration duration = Duration.between( from, to );
System.out.println( "Duration in days: " + duration.toDays() );
System.out.println( "Duration in hours: " + duration.toHours() );

Duration in days: 365
Duration in hours: 8783
```