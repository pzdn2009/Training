# 特殊关键字

* native 本地
* strictfp 严格,精准
* synchronized 线程,同步
* transient 短暂
* volatile 易失
* assert 断言
* instanceof 测试它左边的对象是否是它右边的类的实例
* throws 只用在一个方法的末端，表示这个方法体内部如果有异常，这抛给它的调用者。

## native

Ref:http://blog.csdn.net/jiakw_1981/article/details/3073613
Ref:https://www.cnblogs.com/Alandre/p/4456719.html

   "A native method is a Java method whose implementation is provided by non-java code."

可以将native方法比作Java程序同Ｃ程序的接口，其实现步骤：

```
１、在Java中声明native()方法，然后编译；
２、用javah产生一个.h文件；
３、写一个.cpp文件实现native导出方法，其中需要包含第二步产生的.h文件
（注意其中又包含了JDK带的jni.h文件）；
４、将第三步的.cpp文件编译成动态链接库文件；
５、在Java中用System.loadLibrary()方法加载第四步产生的动态链接库文件，
这个native()方法就可以在Java中被访问了。
```

```java
java HelloNative.java
javah HelloNative
```

## strictfp

strict float point (精确浮点)。

strictfp 关键字可应用于**类、接口或方法**。

使用 strictfp 关键字声明一个方法时，该方法中所有的**float**和**double**表达式都严格遵守**FP-strict**的限制,符合**IEEE-754**规范。

当对一个类或接口使用 strictfp 关键字时，该类中的所有代码，包括嵌套类型中的初始设定值和代码，都将严格地进行计算。严格约束意味着所有表达式的结果都必须是 IEEE 754 算法对操作数预期的结果，以单精度和双精度格式表示。

如果你想让你的浮点运算更加精确，而且不会因为不同的硬件平台所执行的结果不一致的话，可以用关键字strictfp. 

## transient 

当串行化某个对象时，如果该对象的某个变量是transient，那么这个变量不会被串行化进去。

1）一旦变量被transient修饰，变量将不再是对象持久化的一部分，该变量内容在序列化后无法获得访问。

2）transient关键字只能修饰变量，而不能修饰方法和类。

```java
class User implements Serializable {
    private static final long serialVersionUID = 8294180014912103005L;  
 
    private String username;
    private transient String passwd;
 
    public String getUsername() {
        return username;
    }
 
    public void setUsername(String username) {
        this.username = username;
    }
 
    public String getPasswd() {
        return passwd;
    }
 
    public void setPasswd(String passwd) {
        this.passwd = passwd;
    }
}
```

## synchronized 

synchronized修饰方法和synchronized修饰代码块。

synchronized只是一个内置锁的加锁机制，当某个方法加上synchronized关键字后，就表明要获得该内置锁才能执行，并不能阻止其他线程访问不需要获得该内置锁的方法。

## volatile 
