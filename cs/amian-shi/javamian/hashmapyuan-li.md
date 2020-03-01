# HashMap原理

Ref：https://blog.csdn.net/Yoga0301/article/details/84452104

![](/assets/HashMap原理.png)

## 面试题

### 1. HashMap底层是如何实现的？

首先底层数据结构是由数组+链表组成链表散列。HashMap先得到key的散列值，在通过扰动函数（减少碰撞次数）得到Hash值，接着通过hash & （n -1 ），n位table的长度，运算后得到数组的索引值。如果当前节点存在元素，则通过比较hash值和key值是否相等，相等则替换，不相等则通过拉链法查找元素，直到找到相等或者下个节点为null时。
1.8对扰动函数，扩容方法进行优化，并且增加了红黑树的数据结构。

### 2. HashMap 和 Hashtable 的区别

线程安全 HashMap是线程不安全的，而HashTable是线程安全的，每个人方法通过修饰synchronized来控制线程安全。
效率 HashMap比HashTable效率高，原因在于HashTable的方法通过synchronized修饰后，并发的效率会降低。
允不允许null HashMap运行只有一个key为null，可以有多个null的value。而HashTable不允许key，value为null。

### 3.HashMap的长度为什么是2的倍数
在HashMap的操作流程中，首先会对key进行hash算法得到一个索引值，这个索引值就是对应哈希桶数组的索引。为了得到这个索引值必须对扰动后的数跟数组长度进行取余运算。即 hash % n (n为hashmap的长度)，又因为&比%运算快。n如果为2的倍数，就可以将%转换为&，结果就是 hash & (n-1)。所以这就解释了为什么HashMap长度是2的倍数。

### 4.Jdk1.8中满足什么条件后将链表转化成红黑树？

很显然在putVal方法中是判断桶内的节点个数是否大于8，之后通过treeifyBin方法中判断长度是否大于最小红黑树容量64,小于则继续扩容，大于则转为红黑树。

```java
//putVal方法判断桶内元素是是否大于8
if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
    treeifyBin(tab, hash);
break;

//treeifyBin方法中判断长度是否大于最小红黑树容量64,小于则继续扩容，大于则转为红黑树
if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
    resize();
```

## hash()

```java
//这是根据key值获取value值的方法
public V get(Object key) {
    Node<K,V> e;
    //先调用hash(key)
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}

//Hash算法如下
static final int hash(Object key) {
    int h;
    //第一步：先获取key中的hashCode值
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    //第二步 再与hashcode向左移16位的值进行抑或 
}
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        //第三步 n是指数组长度，hash与(n-1)进行&与运算
        (first = tab[(n - 1) & hash]) != null) {
        //省略
        ...
        ...
    }
    return null;
}
```

## get()

```java
final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        //如果表为null或长度为0，或者经过hash算法后得到的哈希桶为null，则直接返回null
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            //如果链表中的第一个节点元素相等则直接返回该Node
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            //第二个节点不为空时继续往后找
            if ((e = first.next) != null) {
                //判断是否为红黑树，是则交给红黑树去查找
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                //否则循环链表找到对应相等的元素，直到找到或下个节点为null
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }

```

## put()

![](http://img.linyoga.com/2018-11-24-HashMap%E7%9A%84put%E6%96%B9%E6%B3%95%E6%80%9D%E7%BB%B4%E5%AF%BC%E5%9B%BE.png)

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        //table为null或length长度为0则扩容
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        //如果哈希桶为null，则创建节点放在该桶内
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            //如果桶内第一个元素hash相等，key相等，则更新此节点
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            //判断是否为红黑树，若是则调用红黑树的putTreeVal
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                //循环链表
                for (int binCount = 0; ; ++binCount) {
                    //知道下个节点为null时
                    if ((e = p.next) == null) {
                        //增加节点
                        p.next = newNode(hash, key, value, null);
                        //如果桶内的节点是否大于8
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        //这个方法里还会判断总节点数大于64则会转换为红黑树
                            treeifyBin(tab, hash);
                        break;
                    }
                    //如果找到相等的节点则退出循环
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            //只有找到相等节点是e不为null
            if (e != null) { // existing mapping for key
                //更新节点为新的值
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        //最后判断增加后的个数是否大于阈值,大于则扩容
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }

```

## resize()

```java
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            //如果容量大于了最大容量时，直接返回旧的table
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            //同时满足扩容两倍后小于最大容量和原先容量大于默认初始化的容量，对阈值增大两倍
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
  
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            //默认初始化容量和阈值
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            //接下来对哈希桶的所有节点转移到新的哈希桶中
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                //如果哈希桶为null，则不需任何操作
                if ((e = oldTab[j]) != null) {
                    //将桶内的第一个节点赋值给e
                    //将原哈希桶置为null，让gc回收
                    oldTab[j] = null;
                    if (e.next == null)
                        //如果e的下个节点（即第二个节点）为null，则只需要将e进行转移到新的哈希桶中
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        //如果哈希桶内的节点为红黑树，则交给TreeNode进行转移
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        //将桶内的转移到新的哈希桶内
                        //JDK1.8后将新的节点插在最后面
                        
                        //下面就是1.8后的优化
                        //1.7是将哈希桶的所有元素进行hash算法后转移到新的哈希桶中
                        //而1.8后，则是利用哈希桶长度在扩容前后的区别，将桶内元素分为原先索引值和新的索引值（即原先索引值+原先容量）。这里不懂为什么，可以看下一段图文讲解。
                        
                        //loHead记录低位链表的头部节点
                        //loTail是低位链表临时变量，记录上个节点并且让next指向当前节点
                        Node<K,V> loHead = null, loTail = null;
                        //hiHead，hiTail与上面的一样，区别在于这个是高位链表
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            //用于临时记录当前节点的next节点
                            next = e.next;
                            //e.hash & oldCap==0表示扩容前后对当前节点的索引值没有发生改变
                            if ((e.hash & oldCap) == 0) {
                                //loTail为null时，代表低位桶内无元素则记录头节点
                                if (loTail == null)
                                    loHead = e;
                                else
                                    //将上个节点next指向当前节点
                                    //即新的节点是插在链表的后面
                                    loTail.next = e;
                                //将当前节点赋值给loTail
                                loTail = e;
                            }
                            else {
                                //跟上面的步骤是一样的。
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                            //当next节点为null则退出循环
                        } while ((e = next) != null);
                        //如果低位链表记录不为null，则低位链表放到原index中
                        if (loTail != null) {
                            //将最后一个节点的next属性赋值为null
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        //如果高位链表记录不为null，则高位链表放到新index中
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
```