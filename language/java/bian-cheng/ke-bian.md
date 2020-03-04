# 可变

## 1. 可变参数Varargs

```java
public int getMax(int... items){
public int getMax(int[] items){
```

## 2. 协变返回类型

Java 5.0添加了对协变返回类型的支持，即子类覆盖（即重写）基类方法时，返回的类型可以是基类方法返回类型的**子类**。协变返回类型允许返回更为具体的类型。

