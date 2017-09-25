# JUnit 
* JUnit是什么
* 注解与断言
* 测试套件
* 参数化测试
* 规则

## What?
JUnit 是java的一个单元测试框架。

* 提供断言测试预期结果。assert
* 提供了测试运行的运行测试。run,debug,coverage
* JUnit测试让您可以更快地编写代码，提高质量
* JUnit测试可以自动运行，检查自己的结果，并提供即时反馈。没有必要通过测试结果报告来手动梳理。
* JUnit测试可以组织成测试套件包含测试案例，甚至其他测试套件。
* Junit显示测试进度的，如果测试是没有问题条形是绿色的，测试失败则会变成红色。

## Annotation？ & Assert?

### Annotations
注解|作用
--|--
@Test (public  void *())|标记一个测试用例
@Before|每个测试用例执行之前，先执行此方法
@After|每个用例执行之后，再执行此方法
@Ingore|忽略测试此方法
@BeforeClass(public static void *())|在类的所有测试之前必须执行一次。如连接DB
@AfterClass|所有的测试执行后再执行

* @Test(timeout = 1000) 。测试超时时间
* @Test(expected = IllegalArgumentException.class)。测试指定异常
* @RunWith(Suite.class)
@Suite.SuiteClasses({
        JunitTest1.class,
        JunitTest2.class
})。测试套件


### Assert

* assertEquals。值相等
* assertTrue。为真
* assertNotNull。不为空
* assertSame。引用相等
* assertArrayEquals。数组相等

## Test Suit？

即测试组，一起执行。

@RunWith，当一个类被注解为@RunWith， JUnit 将调用被在其中注解，以便运行测试类，而不使用内置的 JUnit 运行方法。

```java
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class)
@Suite.SuiteClasses({ PrepareMyBagTest.class, AddPencilsTest.class })
public class SuitTest {

}
```

## 参数化测试？

该类被注解为 @RunWith(Parameterized.class)

* @RunWith(Parameterized.class)，Parameterized 是一个在JUnit内的运行器将运行相同的测试用例组在不同的输入。
* 这个类有一个构造函数，存储测试数据。
* 这个类有一个静态方法生成并返回测试数据，并注明@Parameters注解。
* 这个类有一个测试，它需要注解@Test到方法。

```java
import static org.junit.Assert.assertEquals;
import java.util.Arrays;
import java.util.Collection;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.Parameters;

@RunWith(Parameterized.class)
public class CalculateTest {

	private int expected;
	private int first;
	private int second;

	public CalculateTest(int expectedResult, int firstNumber, int secondNumber) {
		this.expected = expectedResult;
		this.first = firstNumber;
		this.second = secondNumber;
	}

	@Parameters
	public static Collection addedNumbers() {
		return Arrays.asList(new Integer[][] { { 3, 1, 2 }, { 5, 2, 3 },
				{ 7, 3, 4 }, { 9, 4, 5 }, });
	}

	@Test
	public void sum() {
		Calculate add = new Calculate();
		System.out.println("Addition with parameters : " + first + " and "
				+ second);
		assertEquals(expected, add.sum(first, second));
	}
}
```

## Rule？

@Rule注解被使用来标出测试类的公共字段。

这些字段类型为MethodRule，这是测试方法如何运行并报告。多个MethodRules可以应用到一个测试方法。

MethodRule接口有很多的实现，如ErrorCollector在发现了第一个问题之后，也允许继续执行一个测试，ExpectedException 允许在测试规范预期的异常类型和消息，TestName 使得目前的测试名称在测试方法内部可用，以及其他许多。

TestName示例：
```java
import static org.junit.Assert.*;

import org.junit.*;
import org.junit.rules.TestName;

public class NameRuleTest {
	@Rule
	public TestName name = new TestName();

	@Test
	public void testA() {
		System.out.println(name.getMethodName());
		assertEquals("testA", name.getMethodName());

	}

	@Test
	public void testB() {
		System.out.println(name.getMethodName());
		assertEquals("testB", name.getMethodName());
	}
}
```