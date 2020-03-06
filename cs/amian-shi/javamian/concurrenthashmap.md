# ConcurrentHashMap

![](../../../.gitbook/assets/concurrenthashmap-yuan-li.png)

* java8 CAS
* 数组+链表+红黑树
* 可见性刷新：volatile

## get

```java
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        volatile V val;
        volatile Node<K,V> next;

        Node(int hash, K key, V val, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.val = val;
            this.next = next;
        }
     --------------------省略---------------------
 }
```

```java
 public V get(Object key) {
        Node<K,V>[] tab; Node<K,V> e, p; int n, eh; K ek;
        //计算hash值
        int h = spread(key.hashCode());
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (e = tabAt(tab, (n - 1) & h)) != null) {
            //判断头节点是否是我们需要的节点
            if ((eh = e.hash) == h) {
                if ((ek = e.key) == key || (ek != null && key.equals(ek)))
                    return e.val;
            }
            // 如果头节点的 hash 值小于 0，说明正在扩容或者该节点是红黑树
            else if (eh < 0)
                return (p = e.find(h, key)) != null ? p.val : null;
            //链表遍历
            while ((e = e.next) != null) {
                if (e.hash == h &&
                    ((ek = e.key) == key || (ek != null && key.equals(ek))))
                    return e.val;
            }
        }
        return null;
    }
```

* 根据 key 计算 hash 值
* 根据 hash 值找到数组对应位置: \(n – 1\) & h
* 判断该位置是链表还是红黑树
  * 如果该位置为 null，那么直接返回 null
  * 如果该位置的节点刚好就是我们需要的，返回该节点的值
  * 果该位置节点的 **hash 值小于 0，说明正在扩容，或者是红黑树**
  * 如果以上都不满足，那就是链表，进行遍历查找

## put

```java
/** Implementation for put and putIfAbsent */
    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        //计算key的hash值
        int hash = spread(key.hashCode());
        //记录链表长度
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            //数据 tab 为空，初始化数组
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            //根据 hash 值，找到对应数组下标，并获取对应位置 的第一个节点 f
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
            // 如果数组该位置为空，用 CAS 操作将这个新值放入这个位置
            // 如果 CAS 失败，那就是有并发操作，继续下一个循环
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
            //f 的 hash 值等于 MOVED，说明在扩容
            else if ((fh = f.hash) == MOVED)
                //进行数据迁移
                tab = helpTransfer(tab, f);
            else {
                //f 是该位置的首节点，而且不为空
                V oldVal = null;
                // 获取数组该位置的首节点的监视器锁
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        // 首节点 hash 值大于 0，说明是链表
                        if (fh >= 0) {
                            //记录链表长度
                            binCount = 1;
                            //链表遍历
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                //// 如果找到了相等的 key，判断是否要进行值覆盖
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                //到了链表的尾部，将新值放到链表的尾部
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        //如果当前位置是红黑树
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            //红黑树插入新节点
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                //如果链表长度不为0
                if (binCount != 0) {
                    //如果链表长度大于等于临界值【8】，将链表转换为红黑树
                    if (binCount >= TREEIFY_THRESHOLD)
                        //将链表转换成红黑树前判断，如果当前数组的长度小于 64，那么会选择进行数组扩容，而不是转换为红黑树
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }
```

```text
1、根据 key 计算 hash 值

2、遍历数组 tab 
2.1、如果数组为空，进行数组初始化 initTable()
2.2、如果数组不为空，根据 hash 值找到在数组 tab 中的位置，并获取该位置的第一个节点元素对象
    2.2.1、如果数组该位置为空，使用 CAS 操作将值放入该位置
    2.2.2、如果数组该位置的首个元素的hash 值等于 MOVED ，说明在扩容，帮助数据迁移 
    helpTransfer

2.3、如果不满足以上（2.1、2.2）条件，执行以下逻辑：
    2.3.1、获取数组该位置的首节点的监视器锁
    2.3.2、首节点 hash 值大于 0，说明是链表，链表遍历插入值
    2.3.3、否则为红黑树，使用红黑树的 putTreeVal 方法，插入新值
    2.3.4、如果以上执行的是链表操作，判断链表长度是否达到限定值，是否需要转换为红黑树
```

