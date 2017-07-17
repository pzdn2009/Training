# 反射

## 通过一个对象获得完整的包名和类名
```java
public class VersionOne {

	public static void main(String[] args) {

		VersionOne vOne = new VersionOne();
		System.out.println(vOne.getClass().getName());
	}
}
```

## 实例化Class类对象

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
## 获取一个对象的父类与实现的接口

