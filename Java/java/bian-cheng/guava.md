# Guava

* 可变集合
* Preconditions类

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