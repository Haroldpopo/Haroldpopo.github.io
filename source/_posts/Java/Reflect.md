---
title: 反射机制
categories: Java
tags: Java
---

:::info

:book: Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制。

:::

# 反射机制

框架设计的灵魂（写框架需要用到反射机制，使用框架不需要会）

- 框架：半成品软件，可以在框架的基础上进行软件开发，简化编码

- 反射：将类的各个组成部分封装为其它对象，这就是反射机制

  :heart:好处：

  1. 可以在程序运行过程中，操作这些对象
  2. 可以解耦，提高程序的可扩展性


[示例]{.label .primary}`Person`类：

```java Person
package cn.itcast.domain;

public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

## 获取Class对象的方式

1. Class.forName("全类名")：将字节码文件加载进内存，返回Class对象
   
   :bulb:：[&#x1F449;多用于配置文件，将类名定义在配置文件中，读取文件，加载类]{.label .warning}
   
2. 类名.class：通过类名的属性class获取
   
   :bulb:：[&#x1F449;多用于参数的传递]{.label .warning}
   
3. 对象.getClass()：getClass()方法在Object类中定义着
   
   :bulb:：[&#x1F449;多用于对象的获取字节码的方式]{.label .warning}

`ReflectDemo1`：

```java ReflectDemo1
public static void main(String[] args) throws ClassNotFoundException {
    // 1 Class.forName("全类名")
    Class cls1 = Class.forName("cn.itcast.domain.Person");
    System.out.println(cls1);
    // 2 类名.class
    Class cls2 = Person.class;
    System.out.println(cls2);
    // 3 对象.getClass()
    Person person = new Person();
    Class cls3 = person.getClass();
    System.out.println(cls3);

    // == 比较三个对象
    System.out.println(cls1 == cls2);
    System.out.println(cls1 == cls3);
}
```

![Get_Class](https://i.vgy.me/N9NpUu.png)

:::success

`结论：`同一个字节码文件(*.class)在一次程序运行过程中，只会被加载一次，无论通过哪种方式获取的Class对象都是同一个

:::

## Class对象功能

### 获取成员变量们

|                方法                 |                定义                |
| :---------------------------------: | :--------------------------------: |
|         Field[] getFields()         |    获取所有public修饰的成员变量    |
|     Field getField(String name)     | 获取指定名称的public修饰的成员变量 |
|     Field[] getDeclaredFields()     |  获取所有的成员变量，不考虑修饰符  |
| Field getDeclaredField(String name) |   获取指定成员变量，不考虑修饰符   |

`Person`类添加几个不同访问权限的Field，以及重写`toString()`方法

```java Person
public class Person {
    ...
    public String a;
    protected String b;
    String c;
    private String d;

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", a='" + a + '\'' +
                ", b='" + b + '\'' +
                ", c='" + c + '\'' +
                ", d='" + d + '\'' +
                '}';
    }
}
```

#### getFields()

```java ReflectDemo2
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;

    Field[] fields = personClass.getFields();
    for (Field field : fields) {
        System.out.println(field);
    }
}
```

![getFields](https://i.vgy.me/V5p1QP.png)

#### getField(String name)

```java ReflectDemo2
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;
    
    Field a = personClass.getField("a");
    // 获取成员变量 a 的值
    Person person1 = new Person();
    Object value1 = a.get(person1);
    System.out.println(value1);
    
    a.set(person1, "张三");	// 设置成员变量的值
    System.out.println(person1);
}
```

![getField](https://i.vgy.me/WvXzBx.png)

#### getDeclaredFields()

```java ReflectDemo2
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;
    
    Field[] declaredFields = personClass.getDeclaredFields();
    for (Field declaredField : declaredFields) {
        System.out.println(declaredField);
    }
}
```

![getDeclaredFields](https://i.vgy.me/JsuYHy.png)

#### getDeclaredField(String name)

```java ReflectDemo2 mark:8
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;
    
    Field d = personClass.getDeclaredField("d");
    System.out.println(d);
    // 忽略访问权限修饰符的安全检查
    d.setAccessible(true);  // 暴力反射
    Person person2 = new Person();
    Object value2 = d.get(person2);
    System.out.println(value2);
}
```

![getDeclaredField](https://i.vgy.me/waInDR.png)

#### Field：成员变量

1. 设置值

   void set(Object obj, Object value)

2. 获取值

   get(Object obj)

3. 忽略访问权限修饰符的安全检查

   setAccessible(true)：暴力反射

### 获取构造方法们

|                             方法                             |                     定义                     |
| :----------------------------------------------------------: | :------------------------------------------: |
|              Constructor<?>[] getConstructors()              |         返回所有public修饰的构造方法         |
|    Constructor<T> getConstructor(类<?>... parameterTypes)    |    返回参数配型符合的public修饰的构造方法    |
|          Constructor<?>[] getDeclaredConstructors()          |        返回所有构造函数，不考虑修饰符        |
| Constructor<T> getDeclaredConstructor(类<?>... parameterTypes) | 根据指定参数获取匹配的构造方法，不考虑修饰符 |

`Person`类添加几个不同访问权限的构造方法

```java Person
public class Person {
    ...
    private Person(String a, String b) {
        this.a = a;
        this.b = b;
    }

    protected Person(String a, String b, String c) {
        this.a = a;
        this.b = b;
        this.c = c;
    }

    Person(String a, String b, String c, String d) {
        this.a = a;
        this.b = b;
        this.c = c;
        this.d = d;
    }
    ...
}
```

方便测试添加一个`Student`类

```java Student
package cn.itcast.domain;

public class Student {
    private String name;
    private int age;

    private Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    Student(int age) {
        this.age = age;
    }

    protected Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

#### getConstructors()

```java ReflectDemo3
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;

    Constructor[] constructors = personClass.getConstructors();
    for (Constructor constructor : constructors) {
        System.out.println(constructor);
    }
}
```

![getConstructors](https://i.vgy.me/SN1lV0.png)

#### getConstructor(类<?>... parameterTypes)

```java ReflectDemo3
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;
    
    Constructor constructor1 = personClass.getConstructor();
    System.out.println(constructor1);
    Object person1 = constructor1.newInstance();	// 创建对象
    System.out.println(person1);

    System.out.println("--------------------");

    Constructor constructor2 = personClass.getConstructor(String.class, int.class);
    System.out.println(constructor2);
    Object person2 = constructor2.newInstance("张伟", 23);	// 创建对象
    System.out.println(person2);
}
```

![getConstructor](https://i.vgy.me/iQNriI.png)

#### getDeclaredConstructors()

```java ReflectDemo3
public static void main(String[] args) throws Exception {
    // 获取Student的class对象
    Class studentClass = Student.class;
    
    Constructor[] declaredConstructors = studentClass.getDeclaredConstructors();
    for (Constructor declaredConstructor : declaredConstructors) {
        System.out.println(declaredConstructor);
    }
}
```

![getDeclaredConstructors](https://i.vgy.me/e5k1pG.png)

#### getDeclaredConstructor(类<?>... parameterTypes)

```java ReflectDemo3
public static void main(String[] args) throws Exception {
    // 获取Student的class对象
    Class studentClass = Student.class;
    
    Constructor declaredConstructor1 = studentClass.getDeclaredConstructor();
    System.out.println(declaredConstructor1);
    // 忽略访问权限修饰符的安全检查
    declaredConstructor1.setAccessible(true);   // 暴力反射
    Object student1 = declaredConstructor1.newInstance();	// 创建对象
    System.out.println(student1);

    System.out.println("--------------------");
    Constructor declaredConstructor2 = studentClass.getDeclaredConstructor(String.class, int.class);
    System.out.println(declaredConstructor2);
    // 忽略访问权限修饰符的安全检查
    declaredConstructor2.setAccessible(true);   // 暴力反射
    Object student2 = declaredConstructor2.newInstance("李四", 23);	// 创建对象
    System.out.println(student2);
}
```

![getDeclaredConstructor](https://i.vgy.me/SQhcNG.png)

#### Constructor：构造方法

- 创建对象：
  - T newInstance(Object... initargs)
  - 如果使用空参数构造方法创建对象，操作可以简化：Class对象的newInstance方法

### 获取成员方法们

|                             方法                             |                             定义                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                    Method[] getMethods()                     |    获取本类以及父类或者父接口中所有的公共方法(public修饰)    |
|    Method getMethod(String name，类<?>... parameterTypes)    | 返回本类以及父类或者父接口中指定名称（、参数）的public修饰的方法 |
|                Method[] getDeclaredMethods()                 | 返回本类的所有方法，包括私有的(private、protected、默认以及public)的方法 |
| Method getDeclaredMethod(String name，类<?>... parameterTypes) |       返回本类的指定名称（、参数）的方法，不考虑修饰符       |

`Person`类添加几个不同访问权限的自定义方法

```java Person
public class Person {
    ...
    public void eat() {
        System.out.println("Eat...");
    }

    public void eat(String food) {
        System.out.println("Eat " + food);
    }

    private void run() {
        System.out.println("running...");
    }

    protected void run(String city) {
        System.out.println("running in " + city);
    }
}
```

#### getMethods()

```java ReflectDemo4
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;

    Method[] methods = personClass.getMethods();
    for (Method method : methods) {
        System.out.println(method);
    }
}
```

![getMethods](https://i.vgy.me/29Afzw.png)

#### getMethod(String name，类<?>... parameterTypes)

```java ReflectDemo4
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;
    
    Method eat_method1 = personClass.getMethod("eat");
    System.out.println(eat_method1);
    Person person1 = new Person();
    eat_method1.invoke(person1);    // 执行方法

    System.out.println("--------------------");
    Method eat_method2 = personClass.getMethod("eat", String.class);
    System.out.println(eat_method2);
    Person person2 = new Person();
    eat_method2.invoke(person2, "rice");    // 执行方法
}
```

![getMethod](https://i.vgy.me/nHDy28.png)

#### getDeclaredMethods()

```java ReflectDemo4
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;
    
    Method[] declaredMethods = personClass.getDeclaredMethods();
    for (Method declaredMethod : declaredMethods) {
        declaredMethod.setAccessible(true);
        System.out.println(declaredMethod);
        String name = declaredMethod.getName(); // 获取方法名称
        System.out.println(name);
    }
}
```

![getDeclaredMethods](https://i.vgy.me/KnW1WD.png)

#### getDeclaredMethod(String name，类<?>... parameterTypes)

```java ReflectDemo4
public static void main(String[] args) throws Exception {
    // 获取Person的class对象
    Class personClass = Person.class;
    
    Method run_method1 = personClass.getDeclaredMethod("run");
    System.out.println(run_method1);
    Person person3 = new Person();
    run_method1.setAccessible(true);    // 暴力反射
    run_method1.invoke(person3);    // 执行方法

    System.out.println("--------------------");
    Method run_method2 = personClass.getDeclaredMethod("run", String.class);
    System.out.println(run_method2);
    Person person4 = new Person();
    run_method2.setAccessible(true);    // 暴力反射
    run_method2.invoke(person4, "Beijing"); // 执行方法
}
```

![getDeclaredMethod](https://i.vgy.me/GoPbsP.png)

#### Method：方法对象

- 执行方法：
  - Object invoke(Object  obj, Object... args)
- 获取方法名称：
  - String getName：获取方法名

#### 获取类名

- String getName()

## 案例

需求：写一个“框架”，不能改变该类的任何代码的前提下，可以帮我们创建任意类的对象，并且执行其中任意方法实现

- 实现：
  1. 配置文件
  2. 反射
- 步骤：
  1. 将需要创建的对象的全类名和需要执行的方法定义在配置文件中
  2. 在程序中加载读取配置文件
  3. 使用反射技术来加载类文件进内存
  4. 创建对象
  5. 执行方法

新建`pro.properties`配置文件

```properties pro.properties
className=cn.itcast.reflect.Person
methodName=eat
```

使用反射机制创建对象并调用方法

```java ReflectTest
package cn.itcast.reflect;

import java.io.InputStream;
import java.lang.reflect.Method;
import java.util.Properties;

public class ReflectTest {
    /**
     * 假设为框架类
     * */
    public static void main(String[] args) throws Exception {
        // 可以创建任意类的对象，可以执行任意方法

        // 创建Properties对象
        Properties pro = new Properties();
        // 加载配置文件，转换为一个集合
        ClassLoader classLoader = ReflectTest.class.getClassLoader();
        // 获取class目录下的配置文件
        InputStream is = classLoader.getResourceAsStream("pro.properties");
        pro.load(is);

        // 获取配置文件中定义的数据
        String className = pro.getProperty("className");
        String methodName = pro.getProperty("methodName");
        // 加载该类进内存
        Class cls = Class.forName(className);
        // 创建对象
        Object obj = cls.newInstance();
        // 获取方法对象
        Method method = cls.getMethod(methodName);
        // 执行方法
        method.invoke(obj);
    }
}
```

如果不想调用`Person`类的方法，可以直接修改`pro.properties`配置文件，而不修改Java源代码。

```properties pro.properties
className=cn.itcast.reflect.Student
methodName=run
```

:::danger

当Java代码量庞大时，直接修改Java源代码，要经过重新测试、重新编译、重新上线，而仅仅修改配置文件，配置文件只是一个物理文件，改完就完事了。

:::
