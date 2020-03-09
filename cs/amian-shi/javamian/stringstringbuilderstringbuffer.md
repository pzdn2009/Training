# String\|StringBuilder\|StringBuffer

主要从两点来讲：

* 可变性：String不可变
* 线程安全性：StringBuilder，线程不安全。

## 引申

String为什么是不可变的？

## StringBuilder源码

![](/assets/StringBuilder.png)

* 可追加接口
* 字符序列接口
* 接口，抽象类，实现
* 非线程安全
* 初始字符长度为16

链式编程 + 模板方法：
```java
@Override
public StringBuilder append(CharSequence s) {
    super.append(s);
    return this;
}
```

## AbstractStringBuilder源码

* 扩容，本质`value = Arrays.copyOf(value, newCapacity);`，新建了一个
* appendNull


