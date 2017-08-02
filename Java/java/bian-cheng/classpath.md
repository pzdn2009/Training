# classpath

classpath就是存放.class等编译后文件的路径。

1. **何时需要使用-classpath**：当你要编译或执行的类引用了其它的类，但被引用类的.class文件不在当前目录下时，就需要通过-classpath来引入类;
2. **何时需要指定路径**：当你要编译的类所在的目录和你执行javac命令的目录不是同一个目录时，就需要指定源文件的路径(CLASSPATH是用来指定.class路径的，不是用来指定.java文件的路径的) 

```java
ClassLoader 

//这里name是资源的类路径，它是相对与“/”根路径下的位置。
public URL  getResource (String name);  

public InputStream  getResourceAsStream (String name); 
```