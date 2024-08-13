---
title: JAVA-Spring5
date: 2021-05-03 23:00:02
tags:
  - java基础
  - spring
categories:
  - java
cover:
  - https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/Spring.png
---

# Spring5

<!--more-->

## 简介

春天 —>给软件行业带来了春天
2002 年，Rod Jahnson 首次推出了 Spring 框架雏形 interface21 框架。
2004 年 3 月 24 日，Spring 框架以 interface21 框架为基础，经过重新设计，发布了 1.0 正式版。
Rod Johnson 的学历 , 他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。
Spring 理念 : 使现有技术更加实用 . 本身就是一个大杂烩 , 整合现有的框架技术
官方下载地址 ： https://repo.spring.io/libs-release-local/org/springframework/spring/

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>

```

## 优点

- Spring 是一个开源免费的框架 (容器)！
- Spring 是一个轻量级的框架 , 非侵入式的
- **控制反转 IoC , 面向切面 Aop**
- 对事务的支持 , 对框架整合的支持

**Spring 是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。**

## 组成

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式 .

![](img/article/JAVA-Spring5-20210503/20200628180016435.png)

![](img/article/JAVA-Spring5-20210503/20200628180025728.png)

- 核心容器：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用控制反转（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- Spring 上下文：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- Spring AOP：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP 的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- Spring DAO：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- Spring ORM：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- Spring Web 模块：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- Spring MVC 框架：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

## 拓展

![](img/article/JAVA-Spring5-20210503/20200628180035980.png)

- Spring Boot
  - 一个快速开发的脚手架
  - 基于 SpringBoot 可以快速的开发单个微服务
- Spring Cloud
  - Spring Cloud 是基于 SpringBoot 实现的

# IOC 理论推导

1. UserDao 接口

```java
public interface UserDao {
   void getUser();
}
```

2. UserDaoImpl 实现类

```java
public class UserDaoImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("获取用户数据");
  }
}
```

3. UserService 业务接口

```java
public interface UserService {
   void getUser();
}
```

4. UserServiceImpl 业务实现类

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoImpl();

   public void getUser() {
       userDao.getUser();
  }
}
```

5. 测试一下

```java
@Test
public void test(){
   UserService service = new UserServiceImpl();
   service.getUser();
}
```

把 Userdao 的实现类增加一个 .

```java
public class UserDaoMySqlImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("MySql获取用户数据");
  }
}
```

紧接着我们要去使用 MySql 的话 , 我们就需要去 service 实现类里面修改对应的实现

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoMySqlImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

**代码量十分大，修改一次的成本十分昂贵！**

我们使用一个 Set 接口实现，已经发生了革命性的变化！

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao;
	// 利用set实现
   public void setUserDao(UserDao userDao) {
       this.userDao = userDao;
  }

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

- 之前，程序是主动创建对象，控制权在程序员手上！
- 使用了 set 注入后，程序不再具有主动性，而是变成了被动的接受对象！

这种思想，从本质上解决了问题，我们程序员不用再去管对象的创建了。系统的耦合性大大降低，可以专注在业务的实现上！这是 IOC 的原型！

## IOC 本质

控制反转 IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现 IoC 的一种方法，也有人认为 DI 只是 IoC 的另一种说法。没有 IoC 的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![](img/article/JAVA-Spring5-20210503/20200628180055895.png)

采用 XML 方式配置 Bean 的时候，Bean 的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean 的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML 或注解）并通过第三方去生产或获取特定对象的方式。在 Spring 中实现控制反转的是 IoC 容器，其实现方法是依赖注入（Dependency Injection,DI）。**

# HelloSpring

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <!--使用spring创建对象，在spring中这些都称为bean-->
    <bean id="hello" class="com.shan.pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>

</beans>
```

pojo.hello.java

```java
package com.shan.pojo;

public class Hello {
    private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }
}
```

test

```java
@Test
    public void test01(){
        //获取spring的上下文对象
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        // 我们的对象现在都在spring管理了,我们要使用，直接去里面取出来就可以！
        Hello hello =(Hello) context.getBean("hello");
        System.out.println(hello);
    }
```

# IOC 创建对象的方式

1. 使用无参构造创建对象，默认！

2. 假设我们要使用有参构造创建对象。

   1. 下标赋值

      ```xml
      <bean id="exampleBean" class="examples.ExampleBean">
          <constructor-arg index="0" value="7500000"/>
          <constructor-arg index="1" value="42"/>
      </bean>
      ```

   2. 构造参数类型

      ```xml
      <bean id="exampleBean" class="examples.ExampleBean">
          <constructor-arg type="int" value="7500000"/>
          <constructor-arg type="java.lang.String" value="42"/>
      </bean>
      ```

   3. 构造参数名

      ```xml
      <bean id="exampleBean" class="examples.ExampleBean">
          <constructor-arg name="years" value="7500000"/>
          <constructor-arg name="ultimateAnswer" value="42"/>
      </bean>
      ```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了

# Spring 配置

## 别名

```xml
<!--设置别名：在获取Bean的时候可以使用别名获取-->
<alias name="userT" alias="userNew"/>
```

## Bean 的配置

```xml
<!--bean就是java对象,由Spring创建和管理-->

<!--
   id : bean的标识符,要唯一,如果没有配置id,name就是默认标识符
   如果配置id,又配置了name,那么name是别名
   name可以设置多个别名,可以用逗号,分号,空格隔开
   如果不配置id和name,可以根据applicationContext.getBean(.class)获取对象;

   class : bean的全限定名=包名+类名
-->
<bean id="hello" name="hello2 h2,h3;h4" class="com.kuang.pojo.Hello">
   <property name="name" value="Spring"/>
</bean>
```

## import

这个 import,一般用于团队开发使用，他可以将多个配置文件，导入合并为一个；

假设，现在项目中有多个人开发，这三个人复制不同的类开发，不同的类需要注册在不同的 bean 中，我们可以利用 import 将所有人的 beans.xml 合并为一个总的！

applicationContext.xml

```xml
<import resource="{path}/beans.xml"/>
```

# 依赖注入 (DI)

## 构造器注入

前面已经说过了

## Set 方式注入 【重点】

- 依赖注入：Set 注入
  - 依赖：bean 对象的创建依赖于容器
  - 注入：bean 对象中的所有属性，由容器来注入

1. 模拟环境搭建

2. 两个实体类

   ```java
   @Data
   public class Student {

       private String name;
       private Address address;
       private String[] books;
       private List<String> hobbys;
       private Map<String,String> card;
       private Set<String> games;
       private String wife;
       private Properties info;
   }
   ```

   ```java
   @Data
   public class Address {
       private String address;
   }
   ```

3. 配置 applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">

       <bean id="address" class="com.kuang.pojo.Address">
           <property name="address" value="NJUPT9"/>
       </bean>
       <bean id="student" class="com.kuang.pojo.Student">
           <!--第一种，普通值注入，value-->
           <property name="name" value="狂神"/>

           <!--第二种,Bean注入-->
           <property name="address" ref="address"/>

           <!--数组-->
           <property name="books">
               <array>
                   <value>红楼</value>
                   <value>三国</value>
               </array>
           </property>

           <!--List-->
           <property name="hobbys">
               <list>
                   <value>music</value>
                   <value>swimming</value>
                   <value>coding</value>
               </list>
           </property>

           <!--Map-->
           <property name="card">
               <map>
                   <entry key="身份证" value="12312121212"/>
                   <entry key="银行卡" value="678112121111000"/>
               </map>
           </property>

           <!--Set-->
           <property name="games">
               <set>
                   <value>CF</value>
                   <value>LOL</value>
                   <value>GTA</value>
               </set>
           </property>

           <!--null-->
           <property name="wife">
               <null/>
           </property>

           <!--Properties-->
           <property name="info">
               <props>
                   <prop key="学号">20190526</prop>
                   <prop key="username">root</prop>
                   <prop key="password">root</prop>
               </props>
           </property>
       </bean>
   </beans>
   ```

4. 测试

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           Student student = (Student) context.getBean("student");
           System.out.println(student);
       }
   }
   ```

## 拓展方式注入

我们可以使用 p 命名空间和 c 命名空间进行注入

官方解释：

![](img/article/JAVA-Spring5-20210503/20200628180116257.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入，可以直接注入属性的值：property-->
    <bean id="user" class="com.kuang.pojo.User" p:name="狂神" p:age="18"/>

    <!--c命名空间注入，通过构造器注入：construt-args-->
    <bean id="user2" class="com.kuang.pojo.User" c:name="狂神2" c:age="11"/>

</beans>
```

**注意点：p 命名和 c 命名空间不能直接使用，需要导入 xml 约束！**

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

## Bean 的作用域

![](img/article/JAVA-Spring5-20210503/20200628180128654.png)

1. 单例模式（Spring 默认机制）

```xml
<bean id="user2" class="com.kuang.pojo.User" c:name="狂神2" c:age="11" scope="singleton"/>
```

2. 原型模式：每次从容器中 get 的时候，都会产生一个新对象

```xml
<bean id="user2" class="com.kuang.pojo.User" c:name="狂神2" c:age="11" scope="prototype"/>
```

3. 其余的 request、session、application 这些只能在 web 开发中使用到

# Bean 的自动装配

- 自动装配是 Spring 满足 bean 依赖的一种方式
- Spring 会在上下文中自动寻找，并自动给 bean 装配属性

在 Spring 中有三种装配的方式

1. 在 xml 中显示的配置
2. 在 java 中显示的配置
3. 隐式的自动装配 bean 【重要】

## 测试

环境搭建：一个人有两个宠物

## byName 自定装配

```xml
<bean id="cat" class="com.kuang.pojo.Cat"/>
<bean id="dog" class="com.kuang.pojo.Dog"/>

	<!--
        byName : 会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean_id
    -->
<bean id="people" class="com.kuang.pojo.People" autowire="byName">
    <property name="name" value="狂神"/>
</bean>
```

## byTpye 自动装配

```xml
	<!--
        byName : 会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean_id
        byType : 会自动在容器上下文中查找，和自己对象属性类型相同的bean
    -->
<bean id="people" class="com.kuang.pojo.People" autowire="byType">
    <property name="name" value="狂神"/>
</bean>
```

**小结：**

- byName 的时候，需要保证所有 bean 的 id 唯一，并且这个 bean 需要和自动注入的属性的 set 方法的值一致
- byType 的时候，需要保证所有 bean 的 class 唯一，并且这个 bean 需要和自动注入的属性的类型一致

## 使用注解实现自动装配

jdk1.5 支持的注解，Spring2.5 就支持注解了！

要使用注解须知：

1. 导入约束
2. 配置注解的支持 <context:annotation-config/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

### @Autowired

直接在属性上使用即可，也可以在 set 方法上使用

使用 Autowired 我们可以不用编写 set 方法了，前提是你这个自动装配的属性在 IOC（Spring）容器中存在，且符合名字 byName

科普：

```
@Nullable	字段标记了这个注解，说明这个字段可以为null;
```

```java
public @interface Autowired {
    boolean required() default true;
}
```

测试代码：

```java
public class People {
    //如果显示定义了Autowired的required属性为false，说明这个对象可以为Null,否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

如果@Autowired 自动装配的环境比较复杂，自动装配无法通过一个注解【**@Autowired**】完成的时候，我们可以使用**@Qualifier(value = “xxx”)**去配合@Autowire 的使用，指定一个唯一的 bean 对象注入！

```java
public class People {

    @Autowired
    @Qualifier(value = "cat2")
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

### @Resource

```java
public class People {

    @Resource( name = "cat3")
    private Cat cat;
    @Resource
    private Dog dog;
    private String name;
}
```

小结：

@Resource 和@Autowired 的区别：

都是用来自动转配的，都可以放在属性字段上
@Autowired 是通过 byType 的方式实现，而且必须要求这个对象存在！【常用】
@Resource 默认通过 byName 的方式实现，如果找不到名字，则通过 byType 实现！如果两个都找不到的情况下，就报错！【常用】
执行顺序不同: @Autowired 通过 byType 的方式实现。@Resource 默认通过 byName 的方式实现。

原理实现：通过反射机制来实现访问，所以不需要写 set 方法

可以配合@qualifier 来指定一个唯一的 bean 对象注入

@Nullable 如果某个字段标记了这个注解后，说明这个字段可以为 null

@Autowired 里面有个参数 required 可以为 false，说明这个字段可以为 null，否则不允许为空

@Resource 注解 也可以实现和@autowired 一样的功能

注意：@Resource 注解是 Java 自带的注解

使用注解开发

注解说明

@Component：组件，放在类上，说明这个类被 Spring 管理了就是 Bean

衍生的注解

dao 层 @Repository

service 层 @Service

controller 层 @Controller

@Value 可以用来注入属性 可以在 setXXX()方法上直接注入值

@Scope 作用域注解 里面可以有 prototype 和 singleton

xml 与注解：

xml 更加万能，适用于任何场合！

注解 不是自己的类使用不了 维护相对复杂

xml 与注解最佳实践

1.xml 用来管理 bean

2.注解只负责完成属性的注入

3.在使用的过程，想让注解生效，必须开启对注解的支持

```xml
<context:component-scan base-package="com.shan.pojo"/>
<context:annotation-config/>
```

# Java 类实现 Spring 配置

```java
@Configuration
@ComponentScan("pojo")
//这个类也会被注入到Spring容器中，其本质就是一个@Component
//@Configuration代表这是一个配置类，用于替代beans.xml
public class LewisConfig {
    @Bean
    //注册一个bean，相当于xml里的一个bean标签
    //方法的名字相当于bean标签的id值
	//方法的返回类型相当于bean标签的class属性值
    public User getUser(){
        return new User();
    }
}
```

使用下面的代码来获得 Spring 容器，同样用 getBean 方法来获得容器中的对象

```java
ApplicationContext context = new AnnotationConfigApplicationContext(LewisConfig.class);
```

# 面向切面编程 AOP（重点）

- 横切关注点：横跨多个模块的方法或功能，待加入到业务层实现功能扩充，如日志功能
- 切面（aspect）：横切关注点被模块化的特殊对象，是一个类，即下文中的 afterLog 类
- 通知（advice）：切面必须完成的工作，即类中的一个方法
- 目标（target）：被通知对象，下文中的 UserServiceImpl 类的对象
- 代理（proxy）：向目标对象加入通知创建的新对象
- 切入点（pointcut）：切面通知执行的“地点”的定义
- 连接点（jointpoint）：与切入点匹配的执行点

## Spring API 接口实现

在保持 service 层代码不变的基础上，通过 AOP 新增一些其他的功能。

```java
//UserService接口
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void select();
}
//UserServiceImpl实现类
public class UserServiceImpl implements UserService{
    public void add() {
        System.out.println("增加了一个用户");
    }

    public void delete() {
        System.out.println("删除了一个用户");
    }

    public void update() {
        System.out.println("修改了一个用户");
    }

    public void select() {
        System.out.println("查询了一个用户");
    }
}
//afterLog
public class afterLog  implements AfterReturningAdvice {
    public void afterReturning(Object returnValue, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法，返回的结果为"+returnValue);
    }
}
//beforeLog
public class beforeLog implements MethodBeforeAdvice {
    public void before(Method method, Object[] objects, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}
```

xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-2.0.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
">
<!--    注册bean-->
    <bean id="userService" class="com.lewis.service.UserServiceImpl"/>
    <bean id="afterLog" class="com.lewis.log.afterLog"/>
    <bean id="beforeLog" class="com.lewis.log.beforeLog"/>

    <aop:config>
        <aop:pointcut id="pointcup" expression="execution(* com.lewis.service.UserServiceImpl.*(..))"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcup"/>
        <aop:advisor advice-ref="beforeLog" pointcut-ref="pointcup"/>
    </aop:config>
</beans>
```

## 自定义实现 AOP（主要是切面定义）推荐使用

自定义接口类

```java
public class DiyPointCut {
    public void before(){
        System.out.println("===========方法执行前================");
    }
    public void after(){
        System.out.println("===========方法执行后================");
    }
}
```

配置文件

```xml
<bean id="diy" class="diy.DiyPointCut"/>
<aop:config>
    <!--切面：一个类-->
    <aop:aspect ref="diy">
    <!--切入点-->
        <aop:pointcut id="point" expression="execution(* service.UserServiceImpl.*(..))"/>
    <!--通知-->
        <aop:after method="after" pointcut-ref="point"/>
        <aop:before method="before" pointcut-ref="point"/>
    </aop:aspect>
</aop:config>
```

## 使用注解实现 AOP

在类上使用`@Aspect`，表明这个类是一个切面；在方法上使用`@Before,@After`等实现通知

```xml
<bean id="annotationPointCut" class="service.UserServiceImpl"/>
<!--开启注解支持-->
<aop:aspectj-autoproxy/>
```

```java
@Aspect
public class AnnotationPointCut {
    @Before("execution(* service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("===========方法执行前===========");
    }
    @After("execution(* service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("===========方法执行后===========");
    }
     @Around("execution(* service.UserServiceImpl.*(..))")
    public void around(){
        System.out.println("===========环绕后===========");
    }
}
```
