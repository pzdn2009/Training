# 反射

## 反射Reflection

## 原理

在Java中，一切皆对象Object。 类Class只是对象Object中的一种。 而反射Reflection主要用来操作相关类实例Instance的。

```text
# 类的描述：
modifier constructor field method

# 方法的描述：
modifier return methodName (params)
```

相关的技术问题点： 1. Class，包 2. field、method、constructor 3. Array 4. Annotation 5. generic 6. 动态代理 7. ClassLoader 8. getter，setter術語方法，千萬別去從field獲取

Class：类

* Class.forName\(\)
* Object.getClass\(\)
* xx.class

Annotation：注解 Constructor：构造函数 Field：字段 Method：方法

API

* getX：返回public的X，包括集成的
* getDeclaredX：返回所有修饰的X
* invoke：调用方法
* getConstructors：返回所有构造函数
* getSuperclass：返回父类
* getInterfaces：返回接口
* getDeclaredField：返回属性
* setAccessible：授权访问属性

## 示例

### 通过一个对象获得完整的包名和类名

```java
public class VersionOne {

    public static void main(String[] args) {

        VersionOne vOne = new VersionOne();
        System.out.println(vOne.getClass().getName());
    }
}
```

### 实例化Class类对象

```java
public class VersionOne {

    public static void main(String[] args) throws Exception {

        VersionOne vOne = new VersionOne();
        System.out.println(vOne.getClass().getName());

        Class<?> class1 = null;
        Class<?> class2 = null;
        Class<?> class3 = null;

        class1 = Class.forName("VersionOne");
        class2 = new VersionOne().getClass();
        class3 = VersionOne.class;

        System.out.println(class1.getName());
        System.out.println(class2.getName());
        System.out.println(class3.getName());
    }
}
```

### 获取一个对象的父类与实现的接口

### 反射調用Getter

```java
var header = arg.getClass().getMethod("getHeader", null);
CatInfo.setRequestHeader((RequestHeader) header.invoke(arg));
```

