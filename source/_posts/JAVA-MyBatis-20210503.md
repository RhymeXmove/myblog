---
title: JAVA-MyBatis
date: 2021-05-03 01:40:46
tags:
  - 狂神说
categories:
  - java
cover: https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/MyBatis.png
---

# MyBatis

<!--more-->

## 简介

- mybatis 是一个持久层框架；
- 支持定制 SQL、存储过程以及高级映射；
- MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。
- Mybatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 java 的 POJO（Plain Old Java Objects，普通老式 java 对象）为数据库中的记录。

## 什么是持久化？

数据持久化

- 持久化就是将程序的数据在持久状态和瞬间状态转化的过程；
- 内存：断电即失；
- 数据库（jdbc）, io 文件持久化。
- 生活：冷藏、罐头；

为什么需要持久化？

- 有一些对象不能让丢掉；
- 内存在系统中很珍贵；

## 持久层

Dao 层、Service 层、Controller 层.......

- 完成持久化工作的代码块；
- 层界限十分明显。

## 为什么需要 Mybatis？

- 帮助程序员将数据存入到数据库中；
- 方便；
- 传统的 JDBC 代码太复杂了。简化。框架。自动化。
- 不用 MyBatis 也可以。但 MyBatis 更容易上手。技术没有高低之分
- 优点
  - 简单易学
  - 灵活
  - sql 和代码的分离，提高了可维护性。
  - 提供映射标签，支持对象与数据库的 orm 字段映射。
  - 提供对象关系映射标签，支持对象关系组件维护。
  - 提供 xml 标签，支持编写动态 sql。

**最重要的一点：使用的人多！**

## 第一个 MyBatis 程序 `mybatis-01`

- 搭建环境

```sql
CREATE DATABASE `mybatis`;

USE `mybatis`;

CREATE TABLE `user` (
`id` INT(20) NOT NULL PRIMARY KEY,
`name` VARCHAR(30) DEFAULT NULL,
`pwd` VARCHAR(30) DEFAULT NULL
) ENGINE=INNODB DEFAULT CHARSET=utf8;


INSERT INTO `user` (`id`,`name`,`pwd`) VALUES
(1, '狂神', '123456'),
(2, '李四', '123456'),
(3, '张三', '123890')
```

- 新建项目(创建 maven 父子模块)

  - 配置 mybatis 的核心配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
        <environments default="development">
            <environment id="development">
                <transactionManager type="JDBC"/>
                <dataSource type="POOLED">
                    <property name="driver" value="com.mysql.jdbc.Driver"/>
                    <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                    <property name="username" value="root"/>
                    <property name="password" value="123456"/>
                </dataSource>
            </environment>
        </environments>
        <mappers>
            <mapper resource="com/shan/dao/UserMapper.xml"/>
        </mappers>
    </configuration>
    ```

  - 编写 mybatis 工具类

    ```java
    package com.shan.utils;

    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;

    import java.io.IOException;
    import java.io.InputStream;

    //sqlSessionFactory  -->  sqlSession
    public class MybatisUtils {
        private static SqlSessionFactory sqlSessionFactory;

        static {
            try {
                //获取sqlSessionFactory对象
                String resource = "mybatis-config.xml";
                InputStream is = Resources.getResourceAsStream(resource);
                sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        //既然有了sqlSessionFactory, 我们可以从中获取sqlSession实例
        //SqlSession 完全包含了面向数据库执行 SQL 命令所需要的所有方法

        public static SqlSession getSqlSession() {
            return sqlSessionFactory.openSession();
        }
    }

    ```

- 编写实体类

  - 实体类

    ```java
    package com.shan.pojo;

    public class User {
        private int id;
        private String name;
        private String pwd;
        public User() {
        }
        public User(int id, String name, String pwd) {
            this.id = id;
            this.name = name;
            this.pwd = pwd;
        }
        public int getId() {
            return id;
        }
        public void setId(int id) {
            this.id = id;
        }
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public String getPwd() {
            return pwd;
        }
        public void setPwd(String pwd) {
            this.pwd = pwd;
        }
        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", name='" + name + '\'' +
                    ", pwd='" + pwd + '\'' +
                    '}';
        }
    }
    ```

  - Dao 接口

    ```java
    package com.shan.dao;

    import com.shan.pojo.User;

    import java.util.List;

    public interface UserDao {
        List<User> getUserList();
    }
    ```

  - 接口实现类由原本的 UserDaoOmpl 转变成一个 Mapper 配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.shan.dao.UserDao">
        <select id="getUserList" resultType="com.shan.pojo.User">
            select * from user
        </select>
    </mapper>
    ```

- 测试 (测试的包尽量一一对应)

  - junit 测试

    ```shell
    # 错误1
    org.apache.ibatis.binding.BindingException: Type interface com.xiaofan.dao.UserDao is not known to the MapperRegistry.

    # 解决方案
    <mappers>
    	<mapper resource="com/xiaofan/dao/UserMapper.xml"/>
    </mappers>

    # 错误2
    Cause: org.apache.ibatis.builder.BuilderException: Error parsing SQL Mapper Configuration. Cause: java.io.IOException: Could not find resource com/xiaofan/dao/UserMapper.xml

    # 解决方案
        <build>
            <resources>
                <resource>
                    <directory>src/main/resources</directory>
                    <includes>
                        <include>**/*.properties</include>
                        <include>**/*.xml</include>
                        <include>**/*.tld</include>
                    </includes>
                    <filtering>true</filtering>
                </resource>
                <resource>
                    <directory>src/main/java</directory>
                    <includes>
                        <include>**/*.properties</include>
                        <include>**/*.xml</include>
                        <include>**/*.tld</include>
                    </includes>
                    <filtering>true</filtering>
                </resource>
            </resources>
        </build>

    ```

可能遇到的问题：

1. 配置文件没有注册
2. 绑定接口错误
3. 方法名不对
4. 返回类型不对

# CRUD

## 1、namespace

namespace 中的包名要和 Dao/Mapper 接口的包名一致！

## 2、select

选择，查询语句；

- id：就是对应的 nameSpace 中的方法名；
- resultType：Sql 语句执行的返回值！
- parameterType：参数类型

## 增删改需要提交事务

```java
sqlSession.commit();
sqlSession.close();
```

## 3、insert,update,delete

```java
int addUser(User user);
int updUser(User user);
int deleteUser(int id);
```

```java
@Test
    public void addUser(){
        //获得sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        int res = mapper.addUser(new User(5, "haha", "123456"));
        if (res!=0){
            System.out.println("success");
        }
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void updUser() {
        //获得sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        int res = mapper.updUser(new User(4, "aa", "123456"));
        if (res!=0){
            System.out.println("success");
        }
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void deleteUser() {
        //获得sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        int res = mapper.deleteUser(5);
        if (res!=0){
            System.out.println("success");
        }
        sqlSession.commit();
        sqlSession.close();
    }
```

```xml
<!--    对象中的属性可以直接取出来-->
    <insert id="addUser" parameterType="com.shan.pojo.User">
        insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd})
    </insert>

    <update id="updUser" parameterType="com.shan.pojo.User">
        update mybatis.user set name=#{name}, pwd=#{pwd} where id=#{id}
    </update>

    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id=#{id}
    </delete>
```

# 万能 Map

假如我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用 Map 传入数据，而不是对象实体（User）

map 传递参数，直接在 sql 中取出 key 即可！ parameterType="map"

对象传递参数，直接在 sql 中取出对象的属性即可！

只有一个基本类型参数的情况下，可以直接在 sql 中取到！

多个参数用 map, 或者注解

```java
@Test
    public void getUserByIdandName(){
        //获得sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        HashMap<String, Object> objectMap = new HashMap<String, Object>();
        objectMap.put("userid", 7);
        objectMap.put("username", "zz");
        User user = mapper.getUserByIdandName(objectMap);
        System.out.println(user.toString());
        sqlSession.close();
    }
```

```xml
<select id="getUserByIdandName"  parameterType="map" resultType="com.shan.pojo.User">
        select * from mybatis.user where id = #{userid} and name = #{username}
    </select>
```

# 模糊查询

不要让 java 传值拼接通配符`% _`，有 SQL 注入风险，比如传入`or 1=1`，恒定条件会导致全查出来；

sql map 中使用通配符；

```xml
<select id="getUserLike" parameterType="string" resultType="com.shan.pojo.User">
        select * from mybatis.user where name like "%"#{key}"%"
    </select>
```

# 配置之属性优化 mybatis-02

## 核心配置文件

- mybatis-config.xml
- Mybatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。

```
configuration（配置）
    properties（属性）
    settings（设置）
    typeAliases（类型别名）
    typeHandlers（类型处理器）
    objectFactory（对象工厂）
    plugins（插件）
    environments（环境配置）
    	environment（环境变量）
    		transactionManager（事务管理器）
    		dataSource（数据源）
    databaseIdProvider（数据库厂商标识）
    mappers（映射器）

```

## 环境配置

Mybatis 可以配置成适应多种环境

不过要记住，尽管可以配置多个环境，但每个 sqlsessionFactory 实例只能选择一种环境

学会使用配置多套运行环境！------ `<environments default="development">`

MyBatis 默认的事务管理器就是 JDBC，连接池：POOLED

## 属性（properties）

我们可以通过 properties 属性来实现引用配置文件

这些属性都是可外部配置且可动态替换的，既可以在典型的 java 属性文件中配置，也可通过 properties 元素的子元素来传递。 【db.properties】

db.properties

```xml
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8
username=root
password=123456
```

在 mybatis-config.xml 中映入

```xml
<!--引入外部配置文件-->
    <properties resource="db.properties">
    	<property name="username" value="root"/>
	</properties>
```

- 可以直接引入外部配置文件

- 可以在其中增加一些配置属性

- 如果两个文件有同一个字段，优先使用 db.properties 外部配置文件的（先引入本文件属性，在引入外部文件属性，外部文件属性会覆盖本文件属性）

## 类型别名

- 类型别名是为 java 类型设置一个短的名字。
- 存在的意义仅在于用来减少类完全限定名的冗余。

```xml
<!--类型别名-->
    <typeAliases>
        <typeAlias type="com.shan.pojo.User" alias="User"/>
    </typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

扫描实体类的包，他的默认别名就为这个类的类名，首字母小写！

```xml
	<typeAliases>
        <package name="com.shan.pojo"/>
    </typeAliases>
```

两种方法，在实体类比较少的时候，使用第一种方式。

如果实体类身份多，建议使用第二种。

第一种可以 DIY 别名，第二种则不行，如果非要改，需要在实体上增加注解。

```java
@Alias(value = "hello")
public class User {}
```

## 设置 Settings

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。

![](img/article/JAVA-MyBatis-20210503/2020062316474822.png)

## 其他配置

typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins 插件
mybatis-generator-core
mybatis-plus
通用 mapper

## 映射器 mappers

MapperRegistry：注册绑定我们的 Mapper 文件；

方式一：【推荐使用】

```xml
<!--每一个Mapper.xml都需要在MyBatis核心配置文件中注册-->
<mappers>
    <mapper resource="com/kuang/dao/UserMapper.xml"/>
</mappers>
```

方式二：使用 class 文件绑定注册

```xml
<!--每一个Mapper.xml都需要在MyBatis核心配置文件中注册-->
<mappers>
    <mapper class="com.kuang.dao.UserMapper"/>
</mappers>
```

注意点：

接口和他的 Mapper 配置文件必须同名
接口和他的 Mapper 配置文件必须在同一个包下

方式三：使用包扫描进行注入

```xml
<mappers>
    <package name="com.kuang.dao"/>
</mappers>
```

## 作用域和生命周期

![](img/article/JAVA-MyBatis-20210503/20200623164809990.png)

声明周期和作用域是至关重要的，因为错误的使用会导致非常严重的并发问题。

**SqlSessionFactoryBuilder:**

一旦创建了 SqlSessionFactory，就不再需要它了
局部变量
SqlSessionFactory:

说白了就可以想象为：数据库连接池
SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建一个实例。
因此 SqlSessionFactory 的最佳作用域是应用作用域（ApplocationContext）。
最简单的就是使用单例模式或静态单例模式。
SqlSession：

连接到连接池的一个请求
SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
用完之后需要赶紧关闭，否则资源被占用！
![](img/article/JAVA-MyBatis-20210503/20200623164833872.png)

# 解决属性名和字段名不一致的问题

### 问题

数据库中的字段

![](img/article/JAVA-MyBatis-20210503/20200623164845962.png)

新建一个项目，拷贝之前的，测试实体类字段不一致的情况

![](img/article/JAVA-MyBatis-20210503/20200623164853569.png)

测试出现问题
![](img/article/JAVA-MyBatis-20210503/20200623164901861.png)

```xml
// select * from user where id = #{id}
// 类型处理器
// select id,name,pwd from user where id = #{id}
```

解决方法：

- 起别名

```xml
<select id="getUserById" resultType="com.kuang.pojo.User">
    select id,name,pwd as password from USER where id = #{id}
</select>
```

### resultMap

结果集映射

id name pwd

id name password

```xml
<!--结果集映射-->
<resultMap id="UserMap" type="User">
    <!--column数据库中的字段，property实体类中的属性-->
    <result column="id" property="id"></result>
    <result column="name" property="name"></result>
    <result column="pwd" property="password"></result>
</resultMap>

<select id="getUserList" resultMap="UserMap">
    select * from USER
</select>
```

- resultMap 元素是 MyBatis 中最重要最强大的元素。

- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。

- ResultMap 的优秀之处——你完全可以不用显式地配置它们。

  如果这个世界总是这么简单就好了。

# 日志

## 日志工厂

如果一个数据库操作，出现了异常，我们需要排错，日志就是最好的助手！

曾经：sout、debug

现在：日志工厂

![](img/article/JAVA-MyBatis-20210503/20200623164920502.png)

- SLF4J
- LOG4J 【掌握】
- LOG4J2
- JDK_LOGGING
- COMMONS_LOGGING
- STDOUT_LOGGING 【掌握】
- NO_LOGGING

在 MyBatis 中具体使用哪一个日志实现，在设置中设定

**STDOUT_LOGGING**

```xml
<settings>
    <!--标注的日志工厂-->
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

![](img/article/JAVA-MyBatis-20210503/2020062316493391.png)

## Log4j

什么是 Log4j？

Log4j 是 Apache 的一个开源项目，通过使用 Log4j，我们可以控制日志信息输送的目的地是控制台、文件、GUI 组件；

我们也可以控制每一条日志的输出格式；

通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程；

最令人感兴趣的就是，这些可以通过一个配置文件来灵活地进行配置，而不需要修改应用的代码。

1. 先导入 log4j 的包

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

2. log4j.properties

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file
​
#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/rzp.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sq1.PreparedStatement=DEBUG
```

3. 配置 settings 为 log4j 实现
4. 测试运行

**Log4j 简单使用**

1. 在要使用 Log4j 的类中，导入包 import org.apache.log4j.Logger; //注意要导对包
2. 日志对象，参数为当前类的 class 对象

```properties
Logger logger = Logger.getLogger(UserDaoTest.class);
```

3. 日志级别

```properties
logger.info("info: 测试log4j");
logger.debug("debug: 测试log4j");
logger.error("error:测试log4j");
```

1. info
2. debug
3. error

# 分页

**思考：为什么分页？**

- 减少数据的处理量

## 使用 Limit 分页

```sql
SELECT * from user limit startIndex,pageSize   #[0,3)
```

**使用 MyBatis 实现分页，核心 SQL**

1. 接口

   ```java
   //分页
   List<User> getUserByLimit(Map<String,Integer> map);
   ```

2. Mapper.xml

   ```xml
   <!-- 结果集映射 -->
       <resultMap id="UserMap" type="User"/>
   <!--分页查询-->
   <select id="getUserByLimit" parameterType="map" resultMap="UserMap">
       select * from user limit #{startIndex},#{pageSize}
   </select>
   ```

3. 测试

   ```java
       @Test
       public void getUserByLimit(){
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           HashMap<String, Integer> map = new HashMap<String, Integer>();
           map.put("startIndex",1);
           map.put("pageSize",2);
           List<User> list = mapper.getUserByLimit(map);
           for (User user : list) {
               System.out.println(user);
           }
       }
   ```

## RowBounds 分页

不再使用 SQL 实现分页，不建议在开发中使用

1. 接口

   ```java
   //分页2
       List<User> getUserByRowBounds();
   ```

2. mapper.xml

   ```xml
   <!--分页查询2-->
       <select id="getUserByRowBounds" resultMap="UserMap">
           select * from mybatis.user
       </select>
   ```

3. 测试

   ```java
   @Test
       public void getUserByRowBounds(){
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           //RowBounds实现
           RowBounds rowBounds = new RowBounds(1, 2);
           //通过Java代码层面实现分页
           List<User> userList = sqlSession.selectList("com.shan.dao.UserMapper.getUserByRowBounds", null, rowBounds);
           for (User user : userList) {
               System.out.println(user);
           }
           sqlSession.close();
       }
   ```

## 分页插件

![](img/article/JAVA-MyBatis-20210503/20200623164958936.png)

# 使用注解开发

## 面向接口开发

**三个面向区别**

- 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性和方法；
- 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现；
- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题，更多的体现就是对系统整体的架构；

## 使用注解开发

1. 注解在接口上实现

   ```java
   @Select("select * from user")
   List<User> getUsers();
   ```

2. 需要在核心配置文件中绑定接口

   ```xml
   <mappers>
       <mapper class="com.kuang.dao.UserMapper"/>
   </mappers>
   ```

3. 测试

本质：反射机制实现

底层：动态代理

![](img/article/JAVA-MyBatis-20210503/20200623165014965.png)

**MyBatis 详细执行流程**

![](img/article/JAVA-MyBatis-20210503/20200623165030775.png)

## 注解 CURD

```java
//方法存在多个参数，所有的参数前面必须加上@Param("id")注解
@Delete("delete from user where id = ${uid}")
int deleteUser(@Param("uid") int id);
```

**关于@Param( )注解**

- 基本类型的参数或者 String 类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上
- 我们在 SQL 中引用的就是我们这里的@Param()中设定的属性名

**#{} 和 ${}**

# Lombok

Lombok 项目是一个 Java 库，它会自动插入编辑器和构建工具中，Lombok 提供了一组有用的注释，用来消除 Java 类中的大量样板代码。仅五个字符(@Data)就可以替换数百行代码从而产生干净，简洁且易于维护的 Java 类。

使用步骤：

1. 在 IDEA 中安装 Lombok 插件; file--setting--plugin -- 搜索安装 lombok 插件

2. 在项目中导入 lombok 的 jar 包

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.10</version>
    <scope>provided</scope>
</dependency>

```

3. 在程序上加注解

```
@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data //无参构造，get，set，tostring，hashcode，equals
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
```

说明

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String password;
}

```

# 多对一处理

```sql
CREATE TABLE `teacher` (
                           `id` INT(10) NOT NULL,
                           `name` VARCHAR(30) DEFAULT NULL,
                           PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');

CREATE TABLE `student` (
                           `id` INT(10) NOT NULL,
                           `name` VARCHAR(30) DEFAULT NULL,
                           `tid` INT(10) DEFAULT NULL,
                           PRIMARY KEY (`id`),
                           KEY `fktid` (`tid`),
                           CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `student` (`id`, `name`, `tid`) VALUES (1, '小明', 1);
INSERT INTO `student` (`id`, `name`, `tid`) VALUES (2, '小红', 1);
INSERT INTO `student` (`id`, `name`, `tid`) VALUES (3, '小张', 1);
INSERT INTO `student` (`id`, `name`, `tid`) VALUES (4, '小李', 1);
INSERT INTO `student` (`id`, `name`, `tid`) VALUES (5, '小王', 1);

alter table student ADD CONSTRAINT fk_tid foreign key (tid) references teacher(id)
```

多个学生一个老师；

```sql
alter table student ADD CONSTRAINT fk_tid foreign key (tid) references teacher(id)
```

# 一对多处理

> 一个老师多个学生；
>
> 对于老师而言，就是一对多的关系；

## 环境搭建

**实体类**

```java
@Data
public class Student {
    private int id;
    private String name;
    private int tid;
}
```

```java
@Data
public class Teacher {
    private int id;
    private String name;

    //一个老师拥有多个学生
    private List<Student> students;
}
```

```java
<!--按结果嵌套查询-->
<select id="getTeacher" resultMap="StudentTeacher">
    SELECT s.id sid, s.name sname,t.name tname,t.id tid FROM student s, teacher t
    WHERE s.tid = t.id AND tid = #{tid}
</select>
<resultMap id="StudentTeacher" type="Teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <!--复杂的属性，我们需要单独处理 对象：association 集合：collection
    javaType=""指定属性的类型！
    集合中的泛型信息，我们使用ofType获取
    -->
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```

### 小结

1. 关联 - association 【多对一】
2. 集合 - collection 【一对多】
3. javaType & ofType
   1. JavaType 用来指定实体类中的类型
   2. ofType 用来指定映射到 List 或者集合中的 pojo 类型，泛型中的约束类型

**注意点：**

- 保证 SQL 的可读性，尽量保证通俗易懂

- 注意一对多和多对一，属性名和字段的问题

- 如果问题不好排查错误，可以使用日志，建议使用 Log4

  面试高频

  - Mysql 引擎
  - InnoDB 底层原理
  - 索引
  - 索引优化
  -

# 动态 SQL

什么是动态 SQL：动态 SQL 就是根据不同的条件生成不同的 SQL 语句

所谓的动态 SQL，本质上还是 SQL 语句，只是我们可以在 SQL 层面，去执行一个逻辑代码

动态 SQL 是 MyBatis 的强大特性之一。如果你使用过 JDBC 或其它类似的框架，你应该能理解根据不同条件拼接 SQL 语句有多痛苦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL，可以彻底摆脱这种痛苦。

## 搭建环境

```sql
CREATE TABLE `mybatis`.`blog`  (
  `id` int(10) NOT NULL AUTO_INCREMENT COMMENT '博客id',
  `title` varchar(30) NOT NULL COMMENT '博客标题',
  `author` varchar(30) NOT NULL COMMENT '博客作者',
  `create_time` datetime(0) NOT NULL COMMENT '创建时间',
  `views` int(30) NOT NULL COMMENT '浏览量',
  PRIMARY KEY (`id`)
)
```

创建一个基础工程

1. 导包
2. 编写配置文件
3. 编写实体类

```java
@Data
public class Blog {
    private int id;
    private String title;
    private String author;

    private Date createTime;// 属性名和字段名不一致
    private int views;
}
```

4. 编写实体类对应 Mapper 接口和 Mapper.xml 文件

### IF

```java
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <if test="title!=null">
            and title = #{title}
        </if>
        <if test="author!=null">
            and author = #{author}
        </if>
    </where>
</select>
```

### choose (when, otherwise)

### trim、where、set

### SQL 片段

有的时候，我们可能会将一些功能的部分抽取出来，方便服用！

1. 使用 SQL 标签抽取公共部分可

   ```xml
   <sql id="if-title-author">
       <if test="title!=null">
           title = #{title}
       </if>
       <if test="author!=null">
           and author = #{author}
       </if>
   </sql>
   ```

   2. 在需要使用的地方使用 Include 标签引用即可

      ```xml
      <select id="queryBlogIF" parameterType="map" resultType="blog">
          select * from blog
          <where>
              <include refid="if-title-author"></include>
          </where>
      </select>

      ```

注意事项：

- 最好基于单标来定义 SQL 片段
- 不要存在 where 标签

**动态 SQL 就是在拼接 SQL 语句，我们只要保证 SQL 的正确性，按照 SQL 的格式，去排列组合就可以了**

建议：

- 先在 Mysql 中写出完整的 SQL，再对应的去修改成我们的动态 SQL 实现通用即可

# 缓存

## 简介

查询 ： 连接数据库，耗资源

一次查询的结果，给他暂存一个可以直接取到的地方 --> 内存：缓存

我们再次查询的相同数据的时候，直接走缓存，不走数据库了

1. 什么是缓存[Cache]？
   1. 存在内存中的临时数据
   2. 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上（关系型数据库文件）查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题
2. 为什么使用缓存？
   1. 减少和数据库的交互次数，减少系统开销，提高系统效率
3. 什么样的数据可以使用缓存？
   1. 经常查询并且不经常改变的数据 【可以使用缓存】

## MyBatis 缓存

- MyBatis 包含一个非常强大的查询缓存特性，它可以非常方便的定制和配置缓存，缓存可以极大的提高查询效率。
- MyBatis 系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
  - 默认情况下，只有一级缓存开启（SqlSession 级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于 namespace 级别的缓存。
  - 为了提高可扩展性，MyBatis 定义了缓存接口 Cache。我们可以通过实现 Cache 接口来定义二级缓存。

## 一级缓存

- 一级缓存也叫本地缓存：SqlSession
  - 与数据库同一次会话期间查询到的数据会放在本地缓存中
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库

测试步骤：

1. 开启日志

2. 测试在一个 Session 中查询两次记录

   ```java
   @Test
   public void test1() {
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           User user = mapper.getUserById(1);
           System.out.println(user);

           System.out.println("=====================================");

           User user2 =  mapper.getUserById(1);
           System.out.println(user2 == user);
       }
   ```

3. 查看日志输出

   ![](img/article/JAVA-MyBatis-20210503/20200623165129955.png)

**缓存失效的情况：**

1. 查询不同的东西
2. 增删改操作，可能会改变原来的数据，所以必定会刷新缓存
3. 查询不同的 Mapper.xml
4. 手动清理缓存

```java
sqlSession.clearCache();
```

## 二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
- 基于 namespace 级别的缓存，一个名称空间，对应一个二级缓存
- 工作机制
  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中
  - 如果会话关闭了，这个会员对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中
  - 新的会话查询信息，就可以从二级缓存中获取内容
  - 不同的 mapper 查询出的数据会放在自己对应的缓存（map）中

一级缓存开启（SqlSession 级别的缓存，也称为本地缓存）

- 二级缓存需要手动开启和配置，他是基于 namespace 级别的缓存。
- 为了提高可扩展性，MyBatis 定义了缓存接口 Cache。我们可以通过实现 Cache 接口来定义二级缓存。
-

步骤：

1. 开启全局缓存

   ```xml
   <!--显示的开启全局缓存-->
   <setting name="cacheEnabled" value="true"/>
   ```

2. 在 Mapper.xml 中使用缓存

   ```xml
   <!--在当前Mapper.xml中使用二级缓存-->
   <cache
          eviction="FIFO"  //first input first output
          flushInterval="60000"
          size="512"
          readOnly="true"/>
   ```

3. 测试

   1. 问题：我们需要将实体类序列化，否则就会报错

**小结：**

- 只要开启了二级缓存，在同一个 Mapper 下就有效
- 所有的数据都会放在一级缓存中
- 只有当前会话提交，或者关闭的时候，才会提交到二级缓存中

## 缓存原理

![](img/article/JAVA-MyBatis-20210503/20200623165404113.png)

**注意：**

- 只有查询才有缓存，根据数据是否需要缓存（修改是否频繁选择是否开启）useCache=“true”

```xml
    <select id="getUserById" resultType="user" useCache="true">
        select * from user where id = #{id}
    </select>
```

## 自定义缓存-ehcache

Ehcache 是一种广泛使用的开源 Java 分布式缓存。主要面向通用缓存

1. 导包

   ```xml
   <dependency>
       <groupId>org.mybatis.caches</groupId>
       <artifactId>mybatis-ehcache</artifactId>
       <version>1.2.1</version>
   </dependency>
   ```

2. 在 mapper 中指定使用我们的 ehcache 缓存实现

   ```xml
   <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
   ```

3. resource/ehcache.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--
       diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
       user.home – 用户主目录
       user.dir  – 用户当前工作目录
       java.io.tmpdir – 默认临时文件路径
     -->
    <diskStore path="java.io.tmpdir/Tmp_EhCache"/>
    <!--
       defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
     -->
    <!--
      name:缓存名称。
      maxElementsInMemory:缓存最大数目
      maxElementsOnDisk：硬盘最大缓存个数。
      eternal:对象是否永久有效，一但设置了，timeout将不起作用。
      overflowToDisk:是否保存到磁盘，当系统当机时
      timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
      timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
      diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
      diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
      diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
      memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
      clearOnFlush：内存数量最大时是否清除。
      memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
      FIFO，first in first out，这个是大家最熟的，先进先出。
      LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
      LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
   -->
    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>

    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>
</ehcache>
```

4. MyCache.java 重写 其中方法

```java
package com.shan.utils;

import org.apache.ibatis.cache.Cache;

import java.util.concurrent.locks.ReadWriteLock;

public class MyCache implements Cache {
    public String getId() {
        return null;
    }

    public void putObject(Object o, Object o1) {

    }

    public Object getObject(Object o) {
        return null;
    }

    public Object removeObject(Object o) {
        return null;
    }

    public void clear() {

    }

    public int getSize() {
        return 0;
    }

    public ReadWriteLock getReadWriteLock() {
        return null;
    }
}

```

现在一般用 Redis 做缓存！ <K, V>
