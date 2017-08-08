# BeanFactory详解（源码阅读笔记）

* 位置Location
* 意图Intention
* 实用Usage
* 关联Relation
* 层次Hierarchical
* 积累Dot
* 问题Problem

## 1. 位置Location

```java
package org.springframework.beans.factory;
public interface BeanFactory {}
```

![](/assets/java_spring_code_reading_BeanFactory.png)

## 2.意图Intention

1. 提供访问Bean容器的根接口，按照客户端视图ClientView来设计，即从客户端访问（调用）的角度来设计；
2. 可以记为“兵工厂”。里面的每一个Bean，都可以通过**name**来获取；
3. 接近于提供给应用组件集中注册、集中配置；

## 3.使用Usage

提供的都是**查询接口**：

1. 根据Bean名称获取一个Bean对象；
2. 根据Bean名称、Bean的类型、所需参数Optional，返回一个匹配的对象；如果有传递参数的话，可以显式指定参数名；
3. 断言：是否包含Bean，是否为SingleTon，是否为Prototype；
4. 返回Bean的别名。如果给的是一个别名，那么第一个返回值是原名。按照继承层次查找。
5. 返回Bean的类型。
6. 判断类型是否匹配。

## 4.关联Relation

1. BeansException。Bean的异常基类
2. NoSuchBeanDefinitionException。如果找不到Bean，或者Bean不唯一
3. BeanDefinitionStoreException。找不到匹配的Bean**定义**
4. BeanNotOfRequiredTypeException。与需要的Bean的**类型**不匹配

## 5.层次Hierarchical
![](/assets/java_spring_code_reading_BeanFactory_subclasses.png)
* HierarchicalBeanFactory。树形层次。
* ListableBeanFactory。列表式查询接口。
* AutowireCapableBeanFactory。自动注入的接口。
* SimpleJndiBeanFactory。基于Jndi的接口。

## 6.积累Dot
1. @Nullable（org.springframework.lang），方法、参数、类型参数、字段，可为空标记。
2. @NonNullApi，不可为空。

## 7.问题Problem

1. Spring中Prototype design pattern是怎么设计的？
2. FactoryBean又是一个什么梗？
3. Class<T>？Class<?>？
4. Bean的别名？以及模型？别名与原名的叫法以及唯一性映射关系？别名转化机制？
5. ResolvableType？
6. Jndi？



