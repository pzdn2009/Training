# serialVersionUID

```java
public class Person implementsJava.io.Serializable {
    private static  final  long  serialVersionUID;
}
```

Java的序列化机制是通过在运行时判断类的serialVersionUID来验证版本一致性的， serialVersionUID 用来表明实现序列化类的不同版本间的兼容性。

在JDK中，可以利用JDK的bin目录下的serialver.exe工具产生这个serialVersionUID。 如对于Test.class可执行如下命令：serialver Test。

serialVersionUID有两种显示的生成方式：

1. 一个是默认的1L，比如：private static final long serialVersionUID = 1L;        
2. 一个是根据类名、接口名、成员方法及属性等来生成一个64位的哈希字段，

