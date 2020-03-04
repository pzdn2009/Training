# PointCut

Ref :[http://blog.csdn.net/kjfcpua/article/details/7513273](http://blog.csdn.net/kjfcpua/article/details/7513273) 切入点指示符用来指示切入点表达式目的.

格式：修饰符 包名 类 方法 参数。

## 1. 指示符

* **execution**：用于匹配方法执行的连接点；

## 2. 类型匹配语法

* \*  匹配任何字符
* ..  匹配任何数量字符的重复，如在类型模式中匹配任何**数量子包**；而在方法参数模式中匹配任何数量参数。
* +：匹配指定类型的子类型；仅能作为后缀放在类型模式后边。

  AspectJ使用 且（&&）、或（\|\|）、非（！）来组合切入点表达式。

## 3. 示例

```text
# 任何公共方法的执行
public * *(..)

# cn.javass包及所有子包下IPointcutService接口中的任何无参方法
* cn.javass..IPointcutService.*()

# cn.javass包及所有子包下任何类的任何方法
* cn.javass..*.*(..)

# cn.javass包及所有子包下IPointcutService接口的任何只有一个参数方法
* cn.javass..IPointcutService.*(*)

# 非“cn.javass包及所有子包下IPointcutService接口及子类型”的任何方法
* (!cn.javass..IPointcutService+).*(..)

# cn.javass包及所有子包下IPointcutService接口及子类型的的任何无参方法
* cn.javass..IPointcutService+.*()

# cn.javass包及所有子包下IPointcut前缀类型的的以test开头的只有一个参数类型为java.util.Date的方法，
# 注意该匹配是根据方法签名的参数类型进行匹配的，而不是根据执行时传入的参数类型决定的
# 如定义方法：public void test(Object obj);即使执行时传入java.util.Date，也不会匹配的；
* cn.javass..IPointcut*.test*(Java.util.Date)

# cn.javass包及所有子包下IPointcut前缀类型的的任何方法，且抛出IllegalArgumentException和ArrayIndexOutOfBoundsException异常
* cn.javass..IPointcut*.test*(..)  throws
 IllegalArgumentException, ArrayIndexOutOfBoundsException

# 任何实现了cn.javass包及所有子包下IPointcutService接口和java.io.Serializable接口的类型的任何方法
* (cn.javass..IPointcutService+
&& java.io.Serializable+).*(..)

# 任何持有@java.lang.Deprecated注解的方法
@java.lang.Deprecated * *(..)

# 任何持有@java.lang.Deprecated和@cn.javass..Secure注解的方法
@java.lang.Deprecated @cn.javass..Secure  * *(..)
```

