---
title: JAVA-基础
date: 2021-04-04 18:57:41
tags:
- java基础
- 狂神说
categories: 
- java
cover:
- https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/JAVA.png
---





视频地址：https://www.kuangstudy.com/

<!-- more -->

# a++ a-- 先执行程序，再自增/减

​       ++a --a 先自增/减，再执行程序



# Math类 

幂运算 2^3， double pow = Math.pow(3, 2);



# 逻辑计算 与 &&、 或 ||、 非！



# 短路运算:  A&&B, A为false时，B不执行



# 位运算 与 或 取反  异或   &   |   ~   ^ 二进制运算  ，数字逻辑

A= 0011 1100  B= 0000 1101

A&B= 0000 1100   

 A|B= 0011 1101

A^B= 0011 0011  

~B= 1111 0010

<<左移 等价于乘2   >>右移 等价于除2    

  eg: 2x8 = 2x2x2x2 = 2<<3=16 

\>>>  正负取反



# a+=b  同  a=a+b ,  a-=b  同 a=a-b



#  字符串连接符 + 

a = 10 b=20 

println(""+a+b);   

结果：1020

println(a+b+"")

结果：30



# 三元运算符

x ? y : z

如果 x==true ,结果为 y，否则结果为 z

eg: int sc = 58;

String type = sc < 60 ? "不及格" : "及格"；



# 包机制

com.rhyme.shanxin 

一般为域名倒置

import a.b.c.d;  不要用 【.*】，找方法时浪费资源

《JAVA开发手册》 阿里



# javadoc

```java
代码中

/**
* @author shanxin
* @param name
* @return
* @throws Exception
*/
public class abc throw excpetion{
    
}
javadoc  -encoding UTF-8 -charset UTF-8 demo.java
```



# java可变参数

1、在方法声明中，在指定类型后加一个省略号（int… a）。
2、一个方法中只能指定一个可变参数，他必须是方法的最后一个参数，任何普通参数必须在它之前声明。
3、可变参数的本事还是一个数组



```java
public class Demo04 {
    //    可变参数
    public static void main(String[] args) {
        Demo04 demo04 = new Demo04();
        demo04.test(1,2,3,4,5,6);
    }

    public void test(int... i){
        System.out.println(i[0]);
        System.out.println(i[1]);
        System.out.println(i[2]);
        System.out.println(i[3]);
        System.out.println(i[4]);
        System.out.println(i[5]);
    }
}

```



# Arrays常用类

数组string输出

```java
int[] arr；
Arrays.tostring(arr); 
```



快速排序

```java
Arrays.sort(arr);
```



# 面向对象的本质就是：以类的方法组织代码，以对象的方法组织（封装）数据



# 静态方法、非静态方法

```java
//静态方法，可直接调用
public static void say(){
    System.out.println();
}

//非静态方法，通过new一个对象调用
public void shout(){

}
```



两个都是静态方法，可以直接调用



两个都是非静态方法，可以直接调用



静态方法不能直接调用非静态方法，因为静态方法是和类一起加载的，时间线特别早，而非静态方法是new之后才存在的。



# 一个项目应该只存在一个main方法，Application



# 构造器

1.和类名相同

2.没有返回值



作用：

1. new 本质在调用构造方法;
2. 初始化对象的值;



**注意点**：

1. 定义有参构造之后，如果想使用无参构造，现实的定义是一个无参的构造

   

# 封装 继承 多态

1. 封装：高内聚，低耦合；高内聚，内部数据操作自己完成，不允许外部干涉；低耦合，仅暴露少量方法给外部使用。

   数据的隐藏：通常，应禁止直接访问一个对象中的实际表示，而应该通过操作接口来访问； 属性私有，get/set  set方法内可以进行安全性控制，防止非法输入；

2. 继承：（extends）java中只有单继承，没有多继承；

   1. 继承是类和类之间的一种关系，除此之外类和类之间的关系还有依赖，组合，聚合等；

   2. 继承关系的两个类，一个为子类（派生类），一个为父类（基类）。子类继承父类，使用关键字extends来表示；

   3. 子类和父类之间，从意义上讲应该具有is a关系；

   4. object类 ：所有类的父类，包括自己写的类；祖宗类~

   5. super：子类通过super来访问父类的属性；无参构造方法有一个隐藏的super();

      有参构造需要自己写super();

   6. 方法重写：

      1. 前提，需要有继承关系，子类重写父类的方法；
         1. 方法名必须相同；
         2. 参数列表必须相同；
         3. 修饰符，范围可以扩大但不能缩小：public>protected>default>private;
         4. 抛出异常：范围可以缩小但不能扩大：ClassNotFoundException-->Exception(大)
      2. 子类的方法和父类必须一致，方法体不同；
      3. 为什么要重写：
         1. 父类的功能，子类不一定需要，或者不一定满足。

3. 多态：即同一方法可以根据发送对象的不同而采用多种不同的行为方式；

   一个对象的实际类型是确定的，但可以指向对象的引用的类型有很多（父类，有关系的类）；

   1. 多态是方法的多态，属性没有多态；

   2. 父类和子类，有联系，类型转换异常！ClassCastException！

   3. 存在条件：继承关系，方法需要重写；父类引用指向子类对象！father f1 = new son()；

      

   4. 父类引用指向子类的对象；

   5. 把子类转为父类，向上转型；

   6. 把父类转换为子类，向下转型；强制转换（Person）student

   7. 方便方法的调用，减少重复代码！简洁

   



# 修饰词

| 修饰词            | 本类 | 同一个包的类 | 继承类 | 其他类 |
| ----------------- | ---- | ------------ | ------ | ------ |
| private           | √    | ×            | ×      | ×      |
| 无（默认）default | √    | √            | ×      | ×      |
| protected         | √    | √            | √      | ×      |
| public            | √    | √            | √      | √      |



# A instanceof B，判断A,B之间是否有父子关系



# 修饰符

1. static 静态修饰，程序第一次加载时就会运行，之后不会再次运行；

```java
package com.shanxin.oop.demo06;

public class Person {
    {
        System.out.println("匿名代码块");
    }

    static {
        System.out.println("静态代码块");
    }

    public Person() {
        System.out.println("构造方法");
    }

    public static void main(String[] args) {
        Person person1 = new Person();
        System.out.println("=======================");
        Person person2 = new Person();
    }
}

```

静态导入包

```java
package com.shanxin.oop.demo06;
//静态导入包
import static java.lang.Math.random;
import static java.lang.Math.PI;

public class test {
    public static void main(String[] args) {
//        System.out.println(Math.random());
//        System.out.println(Math.PI);

        System.out.println(random());
        System.out.println(PI);
    }
}

```



2. final  修饰常量 final a = 5; 常量表示不能改变的值；

   修饰的类不能被继承

   

# 抽象类

1. abstract修饰符可以用来修饰方法也可以用来修饰类，如果修饰方法，那么方法就是抽象方法；如果修饰类，那么方法就是就是抽象类；
2. 抽象类中可以没有抽象方法，但是有抽象方法的类一定要声明为抽象类。
3. 抽象类，不能使用new关键字来创建对象，它是用来让子类继承的；
4. 抽象方法，只有方法的声明，没有方法的实现，它是用来让子类实现的；
5. 子类继承抽象类，那么就必须要实现抽象类没有实现的抽象方法，否则该子类也要声明为抽象类；



# 接口（可以实现多继承）

1. 普通类：只有具体实现；
2. 抽象类：具体实现和规范（抽象方法）都有!
3. 接口：只有规范！自己无法写方法~专业的约束！约束和实现分离：面向接口编程~
4. 借口就是规范，定义的是一组规则，体现了现实世界中的“如果你是。。。则必须能。。。”的思想，如果你是天使，则必须能飞，如果你是汽车，则必须能跑。如果你是好人，则必须干掉坏人。
5. 接口的本质是契约，就像我们人间的法律一样，制定好后大家都遵守。
6. OO的精髓，是对对象的抽象，最能体现这一点的就是接口，为什么我们讨论设计模式都只针对具备了抽象能力的语言，就是因为设计模式所研究的，实际上就是如何合理地去抽象。
7. 声明接口的关键字是class，声明扣扣的关键字是interface



# 内部类

```java
package com.shanxin.oop.demo09;

//成员内部类
public class Outer {

    private int id = 10;

    public void out() {
        System.out.println("这是外部类的方法");
    }

    //成员内部类
    class Inner {
        public void in() {
            System.out.println("这是成员内部类的方法");
        }

        //获得外部类的私有属性~
        public void getID() {
            System.out.println(id);
        }
    }

    //静态内部类
    public static void B() {

    }

    //局部内部类
    public void method() {
        class Inner {
            public void in() {
                System.out.println("这是局部内部类的方法");
            }
        }
    }
}

//一个java类中可以有多个class类，但是只能有一个public class
//可以在此写测试
//多的一个类
class A {

}

=====================================================================

package com.shanxin.oop.demo09;

public class Application {
    public static void main(String[] args) {
        Outer outer = new Outer();
        //通过外部类实现内部类
        outer.new Inner();

        Outer.Inner inner = outer.new Inner();
        inner.getID();


        //没有名字初始化类，不用将示例保存到变量中~
        //使用场景个，监听器，无限套娃~
        new Apple().eat();
    }
}

class Apple{
    public void eat() {
        System.out.println("1");
    }
}

interface UserService{

}

```



# 异常 Exception 和 Error

主要存在三种异常：

1. 检查性异常 CheckedException：用户错误或问题引起的异常，例如打开不存在的文件，在编译时不能不简单的忽略。

2. 运行时异常 RuntimeException:运行时异常是可能被程序员避免的异常，与检查性异常相反，运行时异常可以在编译时被忽略。

3. 错误 Error: 脱离程序员控制的问题，错误在代码中通常被忽略。例如。当栈溢出时，一个错误就发生了，他们在编译时也检查不到。

4. 异常处理框架：java把异常当做对象来处理，并定义了一个基类java.lang.Throwable作为所有异常的超类。javaAPI中已经定义了许多的异常类。这些异常分为两大类，错误Error和异常Exception。

   

Error: 

1. Error类对象由java虚拟机生成并抛出，大多数错误代码与代码编写者所执行的操作无关；
2. java虚拟机运行错误，当JVM不再有继续执行操作所需的内存资源时，将出现OutOfMemoryError,这些异常发生时，java虚拟机（JVM）一般会选择线程终止；
3. 还有发生在虚拟机试图执行应用时，如类定义错误（NoClassDefFoundError）、链接错误（LinkedError)。这些错误是不可查的，因为它们在应用程序的控制和处理能力之外，而且绝大多数是程序运行时不允许出现的状况。

Exception：

1. 在Exception分支中有一个重要的子类RuntimeException(运行时异常)

   1. ArrayIndexOutOfBoundsException  运行时异常
   2. NullPointerException 空指针异常
   3. ArithmeticException 算数异常
   4. MissingResourceException 丢失资源
   5. ClassNotFoundException 找不到类等异常，这些通常都是不检查异常，程序中可以选择捕获处理，也可以不处理。

   这些异常一般由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生

   

Error 和 Exception的区别：Error通常是灾难性的致命错误，是程序无法控制和处理的，当出现这些异常时，java虚拟机（JVM）一般会选择终止线程；Exception 通常情况下是可以被程序处理的，并且在程序中应该尽可能的去处理这些异常。



# 异常处理机制

抛出异常：throw ,throws

捕获异常：try{}catch{}finally{}

异常处理五个关键字

​	try, catch, finally, throw, throws