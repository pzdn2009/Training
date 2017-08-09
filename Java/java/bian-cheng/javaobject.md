# Java Object 类

![](/assets/java_programming_object.png)

Ref:http://www.importnew.com/10304.html

# Java 问答：终极父类(第一部分)

Java的一些特性会让初学者感到困惑，但在有经验的开发者眼中，却是合情合理的。例如，新手可能不会理解Object类。这篇文章分成三个部分讲跟Object类及其方法有关的问题。

## 上帝类

### 问：什么是Object类？

答：Object类存储在java.lang包中，是所有java类(Object类除外)的**终极父类**。当然，数组也继承了Object类。然而，接口是不继承Object类的，原因在这里指出：Section 9.6.3.4 of the Java Language Specification:“Object类不作为接口的父类”。
Object类中声明了以下函数，我会在下文中作详细说明。
```java
protected Object clone()
boolean equals(Object obj)
protected void finalize()
Class<?> getClass()
int hashCode()
void notify()
void notifyAll()
String toString()
void wait()
void wait(long timeout)
void wait(long timeout, int nanos)
```
java的任何类**都继承了这些函数**，并且可以覆盖不被final修饰的函数。例如，没有final修饰的toString()函数可以被覆盖，但是final wait()函数就不行。

### 问：可以声明要“继承Object类”吗？

答：可以。在代码中明确地写出继承Object类没有语法错误。参考代码清单1。

代码清单1:明确的继承Object类

```java
import java.lang.Object;

//去掉 extends Object同样的效果
public class Employee extends Object {
    private String name;

    public Employee(String name) {
        this.name = name;
    } 
    public String getName() {
        return name;
    }
 
    public static void main(String[] args) {
        Employee emp = new Employee("John Doe");
        System.out.println(emp.getName());
    }
}

output:
John Doe
```

## 克隆Object类

### 问：clone()函数是用来做什么的？

答：clone()可以产生一个相同的类并且返回给调用者。

### 问：clone()是如何工作的？

答：Object将clone()作为一个**本地方法**来实现，这意味着它的代码存放在本地的库中。当代码执行的时候，将会检查调用对象的类(或者父类)是否实现了java.lang.Cloneable接口(Object类不实现Cloneable)。如果没有实现这个接口，clone()将会抛出一个检查异常()——java.lang.CloneNotSupportedException,如果实现了这个接口，clone()会创建一个新的对象，并将原来对象的内容复制到新对象，最后返回这个新对象的引用。

### 问：怎样调用clone()来克隆一个对象？

答：用想要克隆的对象来调用clone()，将返回的对象从Object类转换到克隆的对象所属的类，赋给对象的引用。这里用代码清单3作一个示例。

代码清单3:克隆一个对象
```java
public class CloneDemo implements Cloneable {
    int x;
 
    public static void main(String[] args) throws CloneNotSupportedException {
        CloneDemo cd = new CloneDemo();
        cd.x = 5;
        System.out.printf("cd.x = %d%n", cd.x);
        CloneDemo cd2 = (CloneDemo) cd.clone();
        System.out.printf("cd2.x = %d%n", cd2.x);
    }
}

output:
cd.x = 5
cd2.x = 5
```

代码清单3声明了一个继承Cloneable接口的CloneDemo类。这个接口**必须实现**，否则，调用Object的clone()时将会导致抛出异常CloneNotSupportedException。

CloneDemo声明了一个int型变量x和主函数main()来演示这个类。其中，main()声明可能会向外抛出CloneNotSupportedException异常。

Main()先实例化CloneDemo并将x的值初始化为5。然后输出x的值，紧接着调用clone() ，将克隆的对象传回CloneDemo。最后，输出了克隆的x的值。

### 问：什么情况下需要覆盖clone()方法呢？

答：上面的例子中，**调用clone()的代码是位于被克隆的类(即CloneDemo类)里面的**，所以就不需要覆盖clone()了。但是，如果**调用别的类**中的clone()，就需要覆盖clone()了。否则，将会看到“**clone在Object中是被保护的**”提示，因为clone()在Object中的权限是protected。(译者注：protected权限的成员在不同的包中，只有子类对象可以访问。代码清单3的CloneDemo类和代码清单4的Data类是Object类的子类，所以可以调用clone()，但是代码清单4中的CloneDemo类就不能直接调用Data父类的clone())。代码清单4在代码清单3上稍作修改来演示覆盖clone()。

代码清单4:从别的类中克隆对象

```java
class Data implements Cloneable {
    int x;
 
    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
 
public class CloneDemo {
    public static void main(String[] args) throws CloneNotSupportedException {
        Data data = new Data();
        data.x = 5;
        System.out.printf("data.x = %d%n", data.x);
        Data data2 = (Data) data.clone();
        System.out.printf("data2.x = %d%n", data2.x);
    }
}

output：
data.x = 5
data2.x = 5
```

代码清单4声明了一个待克隆的Data类。这个类实现了Cloneable接口来防止调用clone()的时候抛出异常CloneNotSupportedException，声明了int型变量x，覆盖了clone()方法。这个方法通过执行**super.clone()**来调用父类的clone()(这个例子中是Object的)。通过覆盖来避免抛出CloneNotSupportedException异常。

代码清单4也声明了一个CloneDemo类来实例化Data，并将其初始化，输出示例的值。然后克隆Data的对象，同样将其值输出。

### 问：什么是浅克隆？

A:浅克隆(也叫做浅拷贝)仅仅复制了这个对象**本身的成员变量**，该对象如果引用了其他对象的话，也不对其复制。代码清单3和代码清单4演示了浅克隆。新的对象中的数据包含在了这个对象本身中，不涉及对别的对象的引用。

如果一个对象中的所有成员变量都是原始类型，并且其引用了的对象都是不可改变的(大多情况下都是)时，使用浅克隆效果很好！但是，如果其引用了可变的对象，那么这些变化将会影响到该对象和它克隆出的所有对象！代码清单5给出一个示例。

代码清单5：演示浅克隆在复制引用了可变对象的对象时存在的问题

```java
class Employee implements Cloneable {
    private String name;
    private int age;
    private Address address;
 
    Employee(String name, int age, Address address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }
 
    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
 
    Address getAddress() {
        return address;
    }
 
    String getName() {
        return name;
    }
 
    int getAge() {
        return age;
    }
}
 
class Address {
    private String city;
 
    Address(String city) {
        this.city = city;
    }
 
    String getCity() {
        return city;
    }
 
    void setCity(String city) {
        this.city = city;
    }
}
 
public class CloneDemo {
    public static void main(String[] args) throws CloneNotSupportedException {
        Employee e = new Employee("John Doe", 49, new Address("Denver"));
        System.out.printf("%s: %d: %s%n", e.getName(), e.getAge(),
                          e.getAddress().getCity());
        Employee e2 = (Employee) e.clone();
        System.out.printf("%s: %d: %s%n", e2.getName(), e2.getAge(),
                          e2.getAddress().getCity());
        e.getAddress().setCity("Chicago");
        System.out.printf("%s: %d: %s%n", e.getName(), e.getAge(),
                          e.getAddress().getCity());
        System.out.printf("%s: %d: %s%n", e2.getName(), e2.getAge(),
                          e2.getAddress().getCity());
    }
}

output:
John Doe: 49: Denver
John Doe: 49: Denver
John Doe: 49: Chicago
John Doe: 49: Chicago
```

代码清单5给出了Employee、Address和CloneDemo类。Employee声明了name、age、address成员变量，是可以被克隆的类；Address声明了一个城市的地址并且其值是可变的。CloneDemo类驱动这个程序。

CloneDemo的主函数main()创建了一个Employee对象并且对其进行克隆，然后，改变了原来的Employee对象中address值城市的名字。因为原来的Employee对象和其克隆出来的对象**引用了相同的Address对象**，所以两者都会提现出这个变化。

### 问：什么是深克隆？

答：深克隆(也叫做深复制)会复制这个对象和它所引用的对象的成员变量，如果该对象引用了其他对象，深克隆也会对其复制。例如，代码清单6在代码清单5上稍作修改演示深克隆。同时，这段代码也演示了协变返回类型和一种更为灵活的克隆方式。

代码清单6：深克隆成员变量address
```java
class Employee implements Cloneable
{
   private String name;
   private int age;
   private Address address;
 
   Employee(String name, int age, Address address)
   {
      this.name = name;
      this.age = age;
      this.address = address;
   }
 
   @Override
   public Employee clone() throws CloneNotSupportedException
   {
      Employee e = (Employee) super.clone();
      e.address = address.clone();
      return e;
   }
 
   Address getAddress()
   {
      return address;
   }
 
   String getName()
   {
      return name;
   }
 
   int getAge()
   {
      return age;
   }
}
 
class Address
{
   private String city;
 
   Address(String city)
   {
      this.city = city;
   }
 
   @Override
   public Address clone()
   {
      return new Address(new String(city));
   }
 
   String getCity()
   {
      return city;
   }
 
   void setCity(String city)
   {
      this.city = city;
   }
}
 
public class CloneDemo
{
   public static void main(String[] args) throws CloneNotSupportedException
   {
      Employee e = new Employee("John Doe", 49, new Address("Denver"));
      System.out.printf("%s: %d: %s%n", e.getName(), e.getAge(), 
                        e.getAddress().getCity());
      Employee e2 = (Employee) e.clone();
      System.out.printf("%s: %d: %s%n", e2.getName(), e2.getAge(), 
                        e2.getAddress().getCity());
      e.getAddress().setCity("Chicago");
      System.out.printf("%s: %d: %s%n", e.getName(), e.getAge(), 
                        e.getAddress().getCity());
      System.out.printf("%s: %d: %s%n", e2.getName(), e2.getAge(), 
                        e2.getAddress().getCity());
   }
}

output:
John Doe: 49: Denver
John Doe: 49: Denver
John Doe: 49: Chicago
John Doe: 49: Denver
```

Java支持**协变返回类型**，代码清单6利用这个特性，在Employee类中覆盖父类clone()方法时，将返回类型从**Object类的对象改为Employee类型**。这样做的好处就是，Employee类之外的代码可以不用将这个类转换为Employee类型就可以对其进行复制。

Employee类的clone()方法首先调用super().clone()，对name,age,address这些成员变量进行浅克隆。然后，调用成员变量Address对象的clone()来对其引用Address对象进行克隆。

从Address类中的clone()函数可以看出，这个clone()和我们之前写的clone()有些不同：

Address类没有实现Cloneable接口。因为只有在Object类中的clone()被调用时才需要实现，而Address是不会调用clone()的，所以没有实现Cloneable()的必要。
这个clone()函数没有声明抛出CloneNotSupportedException。这个检查异常只可能在调用Object类clone()的时候抛出。clone()是不会被调用的，因此这个异常也就没有被处理或者传回调用处的必要了。
Object类的clone()没有被调用(这里没有调用super.clone())。因为这不是对Address的对象进行浅克隆——只是一个成员变量复制而已。
为了克隆Address的对象，需要创建一个新的Address对象并对其成员进行初始化操作。最后将新创建的Address对象返回。

### Q:如何克隆一个数组？

A:对数组类型进行浅克隆可以利用clone()方法。对数组使用clone()时，不必将clone()的返回值类型转换为数组类型，代码清单7示范了数组克隆。

代码清单7：对两个数组进行浅克隆
```java
class City {
    private String name;
 
    City(String name) {
        this.name = name;
    }
 
    String getName() {
        return name;
    }
 
    void setName(String name) {
        this.name = name;
    }
}
 
public class CloneDemo {
    public static void main(String[] args) {
        double[] temps = { 98.6, 32.0, 100.0, 212.0, 53.5 };
        for (double temp : temps)
            System.out.printf("%.1f ", temp);
        System.out.println();
        double[] temps2 = temps.clone();
        for (double temp : temps2)
            System.out.printf("%.1f ", temp);
        System.out.println();
 
        System.out.println();
 
        City[] cities = { new City("Denver"), new City("Chicago") };
        for (City city : cities)
            System.out.printf("%s ", city.getName());
        System.out.println();
        City[] cities2 = cities.clone();
        for (City city : cities2)
            System.out.printf("%s ", city.getName());
        System.out.println();
 
        cities[0].setName("Dallas");
        for (City city : cities2)
            System.out.printf("%s ", city.getName());
        System.out.println();
    }
}

output:
98.6 32.0 100.0 212.0 53.5 
98.6 32.0 100.0 212.0 53.5 
 
Denver Chicago 
Denver Chicago 
Dallas Chicago
```
代码清单7声明了一个City类存储名字，还有一些有关城市的数据(比如人口)。CloneDemo类提供了主函数main()来演示数组克隆。

main()函数首先声明了一个双精度浮点型数组来表示温度。在输出数组的值之后，克隆这个数组——注意没有运算符。之后，输出克隆的完全相同的数据。

紧接着，main()声明了一个City对象的数组，输出城市的名字，克隆这个数组，输出克隆的这个数组中城市的名字。为了证明浅克隆完成(比如，这两个数组引用了相同的City对象)，main()最后改变了原来的数组中第一个城市的名字，输出第二个数组中所有城市的名字。我们马上就可以看到，第二个数组中的名字也改变了。

## Equality

### 问：euqals()函数是用来做什么的？

答：equals()函数可以用来检查一个对象与调用这个equals()的这个对象是否相等。

### 问：为什么不用“==”运算符来判断两个对象是否相等呢？

答：虽然“==”运算符可以比较两个数据是否相等，但是要来比较对象的话，恐怕达不到预期的结果。就是说，“==”通过是否**引用**了同一个对象来判断两个对象是否相等，这被称为“引用相等”。这个运算符不能通过比较两个对象的内容来判断它们是不是逻辑上的相等。

### 问：使用Object类的equals()方法可以用来做什么样的对比？

答：Object类默认的eqauls()函数进行比较的依据是：调用它的对象和传入的对象的引用是否相等。也就是说，**默认的equals()进行的是引用比较**。如果两个引用是相同的，equals()函数返回true；否则，返回false.

### 问：覆盖equals()函数的时候要遵守那些规则？

答：覆盖equals()函数的时候需要遵守的规则在Oracle官方的文档中都有申明：

* 自反性：对于任意非空的引用值x，x.equals(x)返回值为真。
* 对称性：对于任意非空的引用值x和y，x.equals(y)必须和y.equals(x)返回相同的结果。
* 传递性：对于任意的非空引用值x,y和z,如果x.equals(y)返回真，y.equals(z)返回真，那么x.equals(z)也必须返回真。
* 一致性：对于任意非空的引用值x和y，无论调用x.equals(y)多少次，都要返回相同的结果。在比较的过程中，对象中的数据不能被修改。
* 对于任意的非空引用值x，x.equals(null)必须返回假。

### 问：能提供一个正确覆盖equals()的示例吗？

答：当然，请看代码清单8。

代码清单8：对两个对象进行逻辑比较
```java
class Employee
{
   private String name;
   private int age;
 
   Employee(String name, int age)
   {
      this.name = name;
      this.age = age;
   }
 
   @Override
   public boolean equals(Object o)
   {
      if (!(o instanceof Employee))
         return false;
 
      Employee e = (Employee) o;
      return e.getName().equals(name) && e.getAge() == age;
   }
 
   String getName()
   {
      return name;
   }
 
   int getAge()
   {
      return age;
   }
}
 
public class EqualityDemo
{
   public static void main(String[] args)
   {
      Employee e1 = new Employee("John Doe", 29);
      Employee e2 = new Employee("Jane Doe", 33);
      Employee e3 = new Employee("John Doe", 29);
      Employee e4 = new Employee("John Doe", 27+2);
      // 验证自反性。
      System.out.printf("Demonstrating reflexivity...%n%n");
      System.out.printf("e1.equals(e1): %b%n", e1.equals(e1));
      // 验证对称性。
      System.out.printf("%nDemonstrating symmetry...%n%n");
      System.out.printf("e1.equals(e2): %b%n", e1.equals(e2));
      System.out.printf("e2.equals(e1): %b%n", e2.equals(e1));
      System.out.printf("e1.equals(e3): %b%n", e1.equals(e3));
      System.out.printf("e3.equals(e1): %b%n", e3.equals(e1));
      System.out.printf("e2.equals(e3): %b%n", e2.equals(e3));
      System.out.printf("e3.equals(e2): %b%n", e3.equals(e2));
      // 验证传递性。
      System.out.printf("%nDemonstrating transitivity...%n%n");
      System.out.printf("e1.equals(e3): %b%n", e1.equals(e3));
      System.out.printf("e3.equals(e4): %b%n", e3.equals(e4));
      System.out.printf("e1.equals(e4): %b%n", e1.equals(e4));
      // 验证一致性。
      System.out.printf("%nDemonstrating consistency...%n%n");
      for (int i = 0; i < 5; i++)
      {
         System.out.printf("e1.equals(e2): %b%n", e1.equals(e2));
         System.out.printf("e1.equals(e3): %b%n", e1.equals(e3));
      }
      // 验证传入非空集合时，返回值为false。
      System.out.printf("%nDemonstrating null check...%n%n");
      System.out.printf("e1.equals(null): %b%n", e1.equals(null));
   }
}
```

### 问：可以使用equals()函数来判断两个数组是否相等吗？

答：可以调用equals()函数来比较数组的引用是否相等。但是，由于在数组对象中无法覆盖equals()，所以只能对数组的引用进行比较，因为不是很常用。参见代码清单9。
代码清单9：尝试通过equals()函数来比较两个数组
```java
public class EqualityDemo
{
   public static void main(String[] args)
   {
  int x[] = { 1, 2, 3 };
  int y[] = { 1, 2, 3 };
 
  System.out.printf("x.equals(x): %b%n", x.equals(x));
  System.out.printf("x.equals(y): %b%n", x.equals(y));
   }
}
```
代码清单9的main()函数中声明了一对类型与内容完全相等的数组。然后尝试对第一个数组和它自己、第一个数组和第二个数组分别进行比较。由于equals()对数组来说比较的仅仅是引用，而不比较内容，所以x.equals(x)返回true（因为自反性——一个对象与它自己相等），但是x.equals(y)返回false。

如果你想要比较的是两个数组的内容，也不要绝望。 可以使用java.util.Arrays 类中声明的 static boolean deepEquals(Object[] a1, Object[] a2) 方法来实现。代码清单10演示了这个方法。

代码清单10：通过deepEquals()函数来比较两个数组
```java
import java.util.Arrays;
 
public class EqualityDemo
{
   public static void main(String[] args)
   {
  Integer x[] = { 1, 2, 3 };
  Integer y[] = { 1, 2, 3 };
  Integer z[] = { 3, 2, 1 };
 
  System.out.printf("x.equals(x): %b%n", Arrays.deepEquals(x, x));
  System.out.printf("x.equals(y): %b%n", Arrays.deepEquals(x, y));
  System.out.printf("x.equals(z): %b%n", Arrays.deepEquals(x, z));
   }
}
```
由于deepEquals()方法要求传入的数组元素必须是对象，所以之前在代码清单9中的元素类型要从int[]改为Integer[]。Java语言的自动封装特性会把integer常量转换成Integer对象存放在数组中。接下来要将数组传入到deepEquals()就是小事一桩了。

用deepEquals()方法比较的相等是“深度”的相等：这要求每个元素对象所包含的的成员、对象相等。成员对象如果还包含了对象，也要相等，以此类推，才算是“相等”（另外，两个空的数组引用也是“深度”的相等，因此Arrays.deepEquals(null, null)返回true）。