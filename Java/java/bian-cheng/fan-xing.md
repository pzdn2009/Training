# 泛型

## 1. 泛型方法

1. 所有泛型方法声明都有一个类型参数声明部分（由尖括号分隔），该类型参数声明部分在方法返回类型之前。
2. 类型参数只能代表引用型类型，不能是原始类型（像int,double,char的等）。

```java
public static<T,E> void print(T value) {
}
```

## 2. 上下界

```java
<T extends Comparable<T>>
<T super F>
```

## 3. 泛型类

## 4. 通配符

```java
List<?>
List<? extends Number>
List<? extends T>
```