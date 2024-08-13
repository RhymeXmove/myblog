---
title: JAVA_javaweb
date: 2021-04-30 23:48:28
tags:
  - java基础
  - 狂神说
categories:
  - java
cover:
  - https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/JAVA.png
---

# 基本概念

<!--more-->

## 前言

web 开发：

- web，网页的意思，www.baidu.com
- 静态 web
  - html,css
  - 提供给所有人看的数据始终不会发生变化！
  - 技术栈：servlet / JSP，ASP，PHP

在 java 中，动态 web 资源开发的技术统称为 JavaWeb

## web 应用程序

web 应用程序：可以提供浏览器访问的程序；

- a.html、b.html ..... 多个 web 资源，这些 web 资源可以被外界访问，对外界提供服务；
- 你们能访问到的任何一个页面或者资源，都存在于这个世界上的某一角落的计算机上。
- URL
- 这个统一的 web 资源会被放在同一个文件夹下，web 应用程序-->Tomcat: 服务器
- 一个 web 应用由多部分组成（静态 web，动态 web）
  - html，css，js
  - jsp，servlet
  - java 程序
  - jar 包
  - 配置文件（properties）

web 应用程序编写完毕后，若想提供给外界访问：需要一个服务器来统一管理；

## 静态 web

- _.html_ 这些都是网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取。通络；

![](img/article/JAVA-javaweb-20210430/20210428131402.png)

- 静态 web 存在的缺点
  - web 页面无法动态更新，所有用户看到都是同一个页面
    - 轮播图，点击特效；伪动态
    - javaScript （实际开发中，它用的最多）
    - VBScript
  - 它无法和数据库交互（数据无法持久化，用户无法交互）

## 动态 web

页面会动态展示：“web 的页面展示的效果因人而异”；

![](img/article/JAVA-javaweb-20210430/20210428131746.png)

缺点：

- 加入服务器的动态 web 资源出现了错误，我们需要重新编写我们的后台程序，重新发布；
  - 停机维护

优点：

- Web 页面可以动态刷新，所有用户看到的都不是同一个页面
- 他可以与数据库交互（数据持久化：注册，商品信息，用户信息........）

![](img/article/JAVA-javaweb-20210430/20210428132115.png)

# Web 服务器

ASP:

- 微软：国内最早流行的就是 ASP；
- 在 HTML 中嵌入了 VB 的脚本，ASP + COM；
- 在 ASP 开发中，基本一个页面都有几千行的代码业务，页面极其混乱
- 维护成本高！
- C#
- LLS

```asp
<h1>
    <h1><h1>
        <h1>
            <h1>
                <h1>
        <h1>
            <%
            System.out.println("hello")
                %>
            <h1>
                <h1>
    <h1><h1>
<h1>
```

php :

- php 开发速度很快，功能很强大，跨平台，代码简单（70%，WordPress）
- 无法承载大访问量的情况（局限性）

JSP/Servlet

B/S: 浏览和服务器

C/S: 客户端和服务器

- sun 公司主推的 B/S 架构
- 基于 java 语言的（所有的大公司，或者一些开源的组件，都是用 java 写的）
- 可以承载三高问题带来的影响；
- 语法像 ASP， ASP--->JSP，加强市场强度；

......

## web 服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些影响信息；

### IIS

微软的；ASP..., windows 中自带的

### Tomcat

面向百度编程；

Tomcat 是 Apache 软件基金会（Apache Software Foundation） 的 Jakarta 项目中的一个核心项目，最新的 Servlet 和 JSP 规范总是能在 Tomcat 中的到体现，因为 Tomcat 技术先进，性能稳定，而且免费，因而深受 java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的 web 应用服务器。

Tomcat 服务器是一个免费的开放源代码 web 应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试 jsp 程序的首选。对于一个 java 初学 web 的人来说，它是最佳的选择。

Tomcat 实际上运行 JSP 页面和 servlet。Tomcat 最新版本为 9.0。

# Tomcat

## Tomcat 启动和配置

文件夹作用：

![](img/article/JAVA-javaweb-20210430/20210428133849.png)

![](img/article/JAVA-javaweb-20210430/20210428133859.png)

可以配置启动的端口号

- tomcat 的默认端口号为：8080
- mysql: 3306
- http: 80
- https: 443

```xml
<Connector URIEncoding="UTF-8" connectionTimeout="20000"
           port="8080" protocol="HTTP/1.1" redirectPort="8443" useBodyEncodingForURI="true"/>
```

可以配置主机的名称

- 默认的主机名为：localhost->127.0.0.1
- 默认网站应用存放的位置为：webapps

```xml
<Host appBase="webapps" autoDeploy="true" name="localhost" unpackWARs="true">
```

**高难度面试题**：

请你谈谈网站是如何进行访问的！

1. 输入一个域名: 回车

2. 检查本机的 C:\Windows\System32\drivers\etc\hosts 配置文件下有没有这个域名映射：

   1. 有：直接返回对应的 IP 地址，这个地址中，有我们需要访问的 web 程序，可以直接访问

      ```powershell
      192.30.255.112 github.com
      ```

   2. 没有：去 DNS 服务器找，找到的话就返回，找不到就返回找不到：

   ![](img/article/JAVA-javaweb-20210430/20210428135406.png)

## 发布一个网站

将自己写的网站，放到服务器（Tomcat）中指定的 Web 应用的文件夹（Wevapps）

网站该有的结构

```
--webapps：Tomcat服务器的web目录
	-ROOT
    -kuangstudy：网站的目录名
    	-WEB-INF
    		-classes：java程序
    		-lib：web应用所依赖的jar包
    		-web.xml：网站配置文件
    	-index.html 默认的首页
    	-static
    		-css
    			-style.css
    		-js
    		-img
    	-......
```

# HTTP

## 什么是 HTTP

HTTP（超文本传输协议）是一个简单的请求-响应协议，他通常运行在 TCP 之上。

- 文本：html，字符串，~......
- 超文本：图片，音乐，视频，定位，地图..........
- 默认端口：80

HTTPS：安全的

- 默认端口：443

## 两个时代

- http1.0
  - HTTP/1.0：客户端可以与 web 服务器连接后，只能获得一个 web 资源，断开连接
- http2.0
  - HTTP/1.1：客户端可以与 web 服务器连接后，可以获得多个 web 资源

## HTTP 请求

- 客户端---发送请求（Request）--- 服务器

百度：

```java
Request URL:https://www.baidu.com/ 请求地址
Request Method:GET     get方法/post方法
Status Code:200 OK     状态码：200
Remote （远程） Address：14.215.177.39:443
```

```java
Accpet:text/html
Accept-Encoding:gzip, deflate, br
Accept-Language:zh-CN, zh; q=0.9 语言
Cache-Control:max-age=0
Connection:keep-alive
```

### 请求行

- 请求行中的请求方式：get

- 请求方式：Get，Post，HEAD，DELETE，PUT，TRACT......
  - get: 请求能够携带的参数比较少，大小有限制，会在浏览器的 URL 地址栏显示数据内容，不安全，但高效；
  - post:请求能够携带的参数没有限制，大小没有限制，不会在浏览器的 URL 地址栏显示数据内容，安全，但不高效；

### 消息头

```java
Accpet: //告诉浏览器，它所支持的数据类型
Accept-Encoding: //支持哪种编码格式 GBK UTF-8 GB-2312 ISO8859-1
Accept-Language: //告诉浏览器，他的语言环境
Cache-Control:  //缓存控制
Connection: //告诉浏览器，请求完成是断开还是保持连接
HOST: 主机....../.
```

## HTTP 响应

- 服务器---响应---客户端

百度

```xml
Cache-control:private  缓存控制
Connection：Keep-Alive  连接
Content-Encoding: gzip  编码
Content-Type:text/html  类型
```

### 响应体

```java
Accpet: //告诉浏览器，它所支持的数据类型
Accept-Encoding: //支持哪种编码格式 GBK UTF-8 GB-2312 ISO8859-1
Accept-Language: //告诉浏览器，他的语言环境
Cache-Control:  //缓存控制
Connection: //告诉浏览器，请求完成是断开还是保持连接
HOST: 主机....../.
Refresh: 告诉客户端，多久刷新一次；
Location: 让网页重新定位；
```

### 响应状态码

200：请求响应成功

3xx：请求重定向

4xx：找不到资源

​ 资源不存在；

5xx：服务器代码错误 500 502：网关错误

常见面试题：

当你的浏览器中地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么?

1. 域名解析
2. 发起 TCP 的三次握手
3. 建立起 TCP 连接后发起 http 请求
4. 服务器响应 http 请求，浏览器得到 html 代码
5. 浏览器解析 html 代码，并请求 html 代码中的资源（css JavaScript 图片）
6. 浏览器对页面进行渲染呈现
7. 详细请参考:https://www.cnblogs.com/wupeixuan/p/8747918.html

# Maven

为什么学习这个技术?

1. 在 javaweb 开发中, 需要使用大量的 jar 包,我们手动去导入;

2. 如何能让一个东西自动帮我导入和配置这个 jar 包。

   由此,maven 诞生了。

## maven--项目架构管理工具

我们目前用来就是方便导入 jar 包的！

Maven 的核心思想，**约定大于配置**

- 有约束，不要去违反。

Maven 会规定好你该如何去编写我们的 java 代码，必须要按照这个规范来。

## 下载，安装，配置环境变量，配置国内镜像，配置本地仓库--略

## 在 IDEA 中使用 Maven

创建 maven 项目

![](img/article/JAVA-javaweb-20210430/20210428162308.png)

![](img/article/JAVA-javaweb-20210430/20210428162631.png)

![](img/article/JAVA-javaweb-20210430/20210428162806.png)

## 创建一个普通的 maven 项目

不选模板创建

一个干净的 maven 项目

![](img/article/JAVA-javaweb-20210430/20210428170220.png)

webapp 文件夹只有在 web 应用下才会有！

## 标记文件夹目录

![](img/article/JAVA-javaweb-20210430/20210428170647.png)

方法二

![](img/article/JAVA-javaweb-20210430/20210428171043.png)

![](img/article/JAVA-javaweb-20210430/20210428171331.png)

![](img/article/JAVA-javaweb-20210430/20210428172011.png)

## pom 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.kuang</groupId>
  <artifactId>javaweb-01-maven</artifactId>
  <version>1.0-SNAPSHOT</version>
  <!-- package 项目的打包方式
   jar: java应用
   war: javaWeb应用
   -->
  <packaging>war</packaging>

  <name>javaweb-01-maven Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <!-- 配置 -->
  <properties>
    <!-- 项目的默认构建编码 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!-- 编码版本 -->
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <!-- 项目依赖 -->
  <dependencies>
    <!-- 具体依赖的jar包配置文件 -->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
    </dependency>
  </dependencies>

  <!-- 项目构建用的东西 -->
  <build>
    <finalName>javaweb-01-maven</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.kuang</groupId>
    <artifactId>javaweb-01-maven02</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- maven的高级之处在于，他会帮你导入这个jar包所依赖的其他jar包 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
    </dependencies>
</project>
```

## maven 无法导出或者生效的问题

maven 由于他的约定大于配置，我们之后可能会遇到我们写的配置文件，无法被导出或者生效的问题，解决方案：

在 build 中配置 resources 节点，来防止我们资源导出失败的问题

```xml
<build>
    .......
      <resources>
        <resource>
            <directory>src/main/resources</directory>
            <!--打包时该路径下过滤掉的-->
            <excludes>
                <exclude>**/*.properties</exclude>
                <exclude>**/*.xml</exclude>
             </excludes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <!--打包时该路径下留下的，其他过滤掉-->
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
    ......
</build>
```

## maven 默认 web 项目中的 web.xml 版本问题

去 tomcat\webapps\ROOT\WEB-INF\web.xml 中复制，这里的是 tomcat 9.0 版本中的

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">


</web-app>
```

# Serverlet

## servlet 简介

- serverlet 就是 sun 公司开发动态 web 的一门技术；
- sun 公司在这些 API 中提供一个接口叫做：Servlet，如果你想开发一个 servlet 程序，只需要完成两个小步骤：
  - 编写一个类，实现 servlet 接口。
  - 把开发好的 java 类部署到 web 服务器中。

**把实现了 Servlet 接口的 java 程序叫做，servlet**

## HelloServlet

Servlet 接口在 sun 公司有两个默认的实现类：HttpServlet GenericServlet

1. 构建一个普通 maven 项目，删掉其中的 src 目录，以后我们的学习就在这个项目里面建立 Moudel；这个空的工程就是 Maven 主工程；

2. 关于 maven 父子工程的理解：

   - 父项目中会有

   ```xml
   <modules>
           <module>servlet-01</module>
   </modules>
   ```

   - 子项目会有

   ```xml
   <parent>
           <artifactId>javaweb-servlet</artifactId>
           <groupId>com.kuang</groupId>
           <version>1.0-SNAPSHOT</version>
   </parent>
   ```

   父项目中的 java 子项目可以直接使用

   ```java
   son extends father
   ```

3. Maven 环境优化

   1. 修改`web.xml`为最新的
   2. 将 maven 的结构搭建完整

4. 编写一个 Servlet 程序

   1. 编写一个普通类
   2. 实现 Servlet 接口
   3. 由于 get 或者 post 只是请求实现的不同方式，可以相互调用，业务逻辑都一样；

   ```java
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.PrintWriter;

   public class HelloServlet extends HttpServlet {
       //由于get或者post只是请求实现的不同方式，可以相互调用，业务逻辑都一样；
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           PrintWriter writer = resp.getWriter(); //响应流
           writer.print("hello servlet");
       }
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           super.doPost(req, resp);
       }
   }
   ```

5. 编写 servlet 的映射

   为什么需要映射，我们写的是 java 程序，但是要通过浏览器访问，而浏览器需要连接 Web 服务器，所以我们需要在 Web 服务器中注册我们写的 servlet，还需要给他一个浏览器能够访问到的路径；

6. 配置 tomcat

7. 启动测试

## Servlet 原理

![](img/article/JAVA-javaweb-20210430/20210428201314.png)

## Mapping 问题

1. 一个 servlet 可以指定一个映射路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
   </servlet-mapping>
   ```

2. 一个 servlet 可以指定对个映射路径

   ```xml
   <!--注册servlet-->
       <servlet>
           <servlet-name>hello</servlet-name>
           <servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
       </servlet>

       <!--servlet的请求路径-->
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello1</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello2</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello3</url-pattern>
       </servlet-mapping>
   ```

3. 一个 servlet 可以指定通用映射路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello/*</url-pattern>
   </servlet-mapping>
   ```

4. 默认请求路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/*</url-pattern>
   </servlet-mapping>
   ```

5. 指定一些后缀或者前缀等等......

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>*.do</url-pattern>
   </servlet-mapping>
   可以自定义
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>*.shan</url-pattern>
   </servlet-mapping>
   ```

6. 优先级问题

   指定了固定的映射路径优先级最高，如果找不到就会走默认的处理请求

   ```xml
       <servlet>
           <servlet-name>errorServlet</servlet-name>
           <servlet-class>com.kuang.servlet.ErrorServlet</servlet-class>
       </servlet>

       <servlet-mapping>
           <servlet-name>errorServlet</servlet-name>
           <url-pattern>/*</url-pattern>
       </servlet-mapping>
   ```

一般都是一对一

## ServletContext

web 容器在启动的时候，它会为每个 web 程序都创建一个对应的 ServletContext 对象，他代表了当前的 Web 应用：

- 共享容器

  我在这个 servlet 中保存的数据，可以在另外一个 servlet 中拿到；（context.setAttribute(); context.getAttribute(); ）

  ![](img/article/JAVA-javaweb-20210430/20210428230921.png)

  ```java
  @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          ServletContext context = this.getServletContext();
          //this.getInitParameter();  //初始化参数
          //this.getServletConfig();  //servlet配置
          //this.getServletContext(); //servlet上下文
          resp.setContentType("text/html;charset=utf-8");
          String username = "秦疆";
          context.setAttribute("username", username);
      }
  ```

  ```java
  @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

          ServletContext context = this.getServletContext();
          String username = (String) context.getAttribute("username");
          resp.setContentType("text/html;charset=utf-8");
          resp.getWriter().print("名字"+username);
      }
  ```

  ```xml
  <servlet>
          <servlet-name>hello</servlet-name>
          <servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>hello</servlet-name>
          <url-pattern>/hello</url-pattern>
      </servlet-mapping>

      <servlet>
          <servlet-name>getc</servlet-name>
          <servlet-class>com.kuang.servlet.GetServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>getc</servlet-name>
          <url-pattern>/getc</url-pattern>
  </servlet-mapping>
  ```

## 请求转发

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        System.out.println("servlet");
       // RequestDispatcher requestDispatcher = context.getRequestDispatcher("/servlet03"); //请求转发的路径
      //  requestDispatcher.forward(req,resp); //调用foward实现请求转发

    context.getRequestDispatcher("/servlet03").forward(req,resp); //同上两行，调用foward实现请求转发
    }
```

## 读取资源文件

Properties

- 在 java 目录下新建 properties
- 在 resource 目录下新建 properties

发现：都被打包到了同一个路径下：classes，我们俗称这个路径为 classpath

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=utf-8");
        InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");
        Properties prop = new Properties();
        prop.load(is);
        String username = prop.getProperty("username");
        String password = prop.getProperty("password");

        resp.getWriter().print(username+":"+password);
    }
```

访问测试都可 OK；

## HttpServletResponse

web 服务器接收到客户端的 HTTP 请求，针对这个请求，分别创建一个代表请求的 HttpServletRequest 对象。代表相应的一个 HttpServletResponse；

- 我们如果要获取客户端请求过来的参数；找 HttpServletRequest；
- 如果要给客户响应一些信息；找 HttpServletResponse；

### 简单分类

#### 负责向浏览器发送数据的办法

```java
ServletOutputStream getOutputStream() throw IOException;
PrintWriter getWriter() throws IOException;
```

#### 负责向浏览器发送响应头的方法

```java
(ServletResponse)
void setCharacterEncoding(String charset);
void setContentLength(int len);
void setContentLengthLong(long len);
void setContentType(String type);
(HttpServletResponse)
void setDateHeader(String name, long date);
void addDateHeader(String name, long date);
void setHeader(String name, String value);
void addHeader(String name, String value);
void setIntHeader(String name, int value);
void addIntHeader(String name, int value);
```

#### 响应的状态码

404.....................

#### 常见应用

1. 向浏览器输出消息（前面内容）
2. 下载文件
   1. 要获取下载文件的路径
   2. 下载的文件名是什么
   3. 设置想办法让浏览器能够支持下载我们需要的东西
   4. 获取下载文件的输入流
   5. 创建缓冲区
   6. 获取 OutputStream 对象
   7. 将 FileOutputStream 流写入到 buffer 缓冲区
   8. 使用 OutputStream 将缓冲区中的数据输出到客户端！

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1. 要获取下载文件的路径
        String realPath = "D:\\workspace\\IDEA-workspace\\kuangshen_java\\javaweb-servlet\\response\\src\\main\\resources\\1.png";
        System.out.println("下载文件的路径："+realPath);
        //2. 下载的文件名是什么
        String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
        //3. 设置想办法让浏览器能够支持下载我们需要的东西
        resp.setHeader("Content-Disposition","attachment;filename="+URLEncoder.encode(fileName, "utf-8"));
        //4. 获取下载文件的输入流
        FileInputStream fileInputStream = new FileInputStream(realPath);
        //5. 创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];
        //6. 获取OutputStream对象

        ServletOutputStream outputStream = resp.getOutputStream();
        //7. 将FileOutputStream流写入到buffer缓冲区
        //8. 使用OutputStream将缓冲区中的数据输出到客户端！
        while ((len=fileInputStream.read(buffer))>0){
            outputStream.write(buffer,0,len);
        }
        fileInputStream.close();
        outputStream.close();
    }
```

#### 验证码

验证码怎么来的？

- 前端实现
- 后端实现、需要用到 java 的图片类，生产一个图片

```java
public class ImageServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //如何让浏览器5秒自动刷新一次
        resp.setHeader("refresh","3");

        //在内存中创建一个图片
        BufferedImage image = new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
        //的到图片
        Graphics2D g = (Graphics2D) image.getGraphics();
        //设置图片的背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);
        //给图片写数据
        g.setColor(Color.BLUE);
        g.setFont(new Font(null,Font.BOLD,20));
        g.drawString(makeNum(),0,20);
        //告诉浏览器这个请求用图片的方式打开
        resp.setContentType("image/jpeg");
        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");
        //把图片写给浏览器
        ImageIO.write(image,"jpg", resp.getOutputStream());
    }

    private String makeNum() {
        Random random = new Random();
        String num = random.nextInt(9999999)+"";
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < 7-num.length(); i++) {
            sb.append("0");
        }
        num = sb.toString() + num;
        return num;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```

#### 实现重定向

B 一个 Web 资源收到客户端 A 请求后，B 会通知 A 客户端去访问另外一个 Web 资源 C，这个过程叫做重定向；

常见场景：

- 用户登录

```java
resp.sendRedirect("/index.jsp");
```

```java
resp.setHeader("Location","/r/img");
        resp.setStatus(302);
```

面试题：

重定向和转发的区别？

相同点

- 页面都会实现跳转

不同点

- 请求转发的时候，url 不会产生变化 307
- 重定向的时候，URL 地址会发生变化 302

## HttpServletRequest

```java
@Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbys = req.getParameterValues("hobby");

        System.out.println(username);
        System.out.println(password);
        System.out.println(Arrays.toString(hobbys));

        //通过请求转发
        req.getRequestDispatcher("./success.jsp").forward(req, resp);
    }
```

# Cookie、Session

## 会话

**概念**：用户打开一个浏览器，点击了很多超链接，访问了很多 Web 资源，关闭浏览器，这个过程称之为会话；

**有状态会话**：

一个网站，怎么证明你来过？

客户端 服务端

1. 服务端给客户端一个信件，客户端下次访问服务端带上信件就可以了；cookies
2. 服务端登记你来过了，下次你来的时候我来匹配你；session

## 保存会话的两种技术

cookie

- 客户端技术（响应，请求）

session

- 服务端技术，利用这个技术，可以保存用户的会话信息？我们可以把信息或者数据放在 session 中！

常见场景：网站登陆之后，下次不用登陆了，第二次访问直接就登上了！

## Cookie

1. 从请求中拿到 cookie 信息。
2. 服务器响应给客户端 cookie。

各种常用方法：

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //服务器，告诉你，你来的时间，把这个时间封装成一个信件，你下次带来，我就知道你来了
        //解决中文乱码
        resp.setContentType("text/html");
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        PrintWriter out = resp.getWriter();

        //Cookie，服务器端从客户端获取时
        Cookie[] cookies = req.getCookies();  //这里返回数组，说明cookies可能存在多个
        //判断Cookies是否存在
        if (cookies!=null) {
            //如果cookies存在
            out.write("你上一次访问的时间是：");

            for (int i = 0; i < cookies.length; i++) {
                Cookie cookie = cookies[i];
                //获取cookie的值 cookie.getValue();
                if (cookie.getName().equals("LastLoginTime")) {
                    //获取cookie的名字
                    long l = Long.parseLong(cookie.getValue());
                    Date date = new Date(l);
                    out.print(date.toLocaleString());
                }

            }
        }else {
            out.write("这是你第一次访问本站");
        }
        // 服务器给客户端响应一个cookie
        Cookie cookie = new Cookie("LastLoginTime", System.currentTimeMillis()+"");

        //设置cookie有效期，关闭浏览器不会删除cookie
        cookie.setMaxAge(24*60*60);
        //响应给客户端一个cookie，可以多个
        /*
        resp.addCookie(cookie);
        resp.addCookie(cookie);
        resp.addCookie(cookie);
        */
        resp.addCookie(cookie);
    }
```

cookie：一般会保存在本地的用户目录下 appdata:

一个网站的 cookie 是否存在上限！**细节问题**

- 一个 cookie 只能保存一个信息；
- 一个 web 站点/域名可以给浏览器发送多个 cookie，最多存放 20 个；
- 所有 cookie 总大小不超出 4kb;
- 浏览器总共能储存 300 个 cookie；
- 具体情况根据浏览器不同会有不同限制；

删除 cookie：

- 不设置有效期，关闭浏览器，自动失效；
- 设置有效期时间为 0；

PS：解码的一个方法

```java
URLDecoder.decode(String s, "UTF-8");
```

## Session (重点)

什么是 Session：

- 服务器会给每一个用户（浏览器）创建一个 session 对象；
- 一个 session 独占一个浏览器，只要浏览器没有关闭，这个 session 就存在；
- 用户登录之后，整个网站他都可以访问！-->保存用户信息，保存购物车的信息......

Session 和 cookie 的区别：

- Cookie 是把用户的数据写给用户的浏览器，浏览器保存（可以保存）；
- Session 把用户的数据写到用户独占 session 中，服务器端保存；（减少服务器资源的浪费）
- Session 对象由服务创建；

使用场景：

- 保存一个登录用户的信息；
- 购物车信息；
- 在整个网站中经常会使用的数据，我们将它保存在 session 中；

使用 Session:

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决中文乱码
        resp.setContentType("text/html；charset=utf-8");
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        //得到session
        HttpSession session = req.getSession();
        //给session中存东西
        session.setAttribute("name", new Person("秦疆", 1));
        //获取sessionID
        String id = session.getId();
        //判断session是否是新创建的
        if (session.isNew()) {
            resp.getWriter().write("session创建成功，ID"+id);
        } else {
            resp.getWriter().write("session已经在服务器中存在，ID"+id);
        }
    }
```

```java
//得到session
HttpSession session = req.getSession();
//给session中存东西
Person person = (Person) session.getAttribute("name");
resp.getWriter().write(person.toString());
```

```java
//移除session
HttpSession session = req.getSession();
session.removeAttribute("name");
```

```java
  <!--设置session失效时间-->
  <session-config>
    <!--15分钟后失效，timeout以分钟为单位-->
    <session-timeout>15</session-timeout>
  </session-config>
```

# javaBean

实体类

javaBean 有特定的写法：

- 必须要有一个无参构造；
- 属性必须私有化；
- 必须有对应的 get/set 方法；

一般用来和数据库的字段做映射 ORM；

ORM 对象关系映射

- 表->类
- 字段->属性
- 行记录->对象

**people 表**

| id  | name      | age | address |
| --- | --------- | --- | ------- |
| 1   | 秦疆 1 号 | 3   | 西安    |
| 2   | 秦疆 2 号 | 18  | 西安    |
| 3   | 秦疆 3 号 | 100 | 西安    |

# MVC 三层架构

什么是 MVC：model，view，controller 模型，视图，控制器

![](img/article/JAVA-javaweb-20210430/20210430181656.png)

用户直接访问控制层，控制层就可以直接操作数据库：

```java
servlet --CURD --->数据库
弊端：程序十分臃肿，不利于维护
servlet的代码中；处理请求、响应、视图跳转、处理JDBC，处理业务代码，处理逻辑代码

架构：没有什么是加了一层解决不了的......
```

Model

- 业务处理：业务逻辑（service）
- 数据持久层：CURD （Dao）

View

- 展示数据
- 提供链接发起 servlet 请求（a，form，img.......）

Controller (Servlet)

- 接收用户的请求：（req：请求参数、Session 信息......）
- 交给业务层处理对应的代码
- 控制视图的跳转

```java
登录 ---接受用户的请求 --- 处理用户的请求（获取用户登录的参数，username，password） --- 交给业务层处理登录业务（判断用户名密码是否正确：事务）--- Dao层查询用户名和密码是否正确 --- 数据库
```

# Filter

FIlter：过滤器，用来过滤网站数据；

- 处理中文乱码
- 登录验证

![](img/article/JAVA-javaweb-20210430/20210430200702.png)

## Filter 开发步骤

1. 导包

2. 编写过滤器

   ```java
   package com.kuang.filter;

   import javax.servlet.*;
   import java.io.IOException;

   public class CharacterEncodingFilter implements Filter {
       //初始化
       public void init(FilterConfig filterConfig) throws ServletException {
           System.out.println("CharacterEncodingFilter初始化");
       }

       //  chain（链）
       /*
       过滤中的所有代码，在过滤特定请求的时候都会执行
       必须要让过滤器继续通行  chain.doFilter(request,response);
        */
       public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
           request.setCharacterEncoding("utf-8");
           response.setCharacterEncoding("utf-8");
           response.setContentType("text/html;charset=utf-8");
           System.out.println("CharacterEncodingFilter执行前......");
           chain.doFilter(request,response);//让请求继续走，如果不写，程序会被这里拦截
           System.out.println("CharacterEncodingFilter执行后......");
       }

       //销毁
       public void destroy() {
           System.out.println("CharacterEncodingFilter销毁");
       }
   }

   ```

3. 在 web.xml 中配置 Filter

   ```java
       <filter>
           <filter-name>characterEncodingFilter</filter-name>
           <filter-class>com.kuang.filter.CharacterEncodingFilter</filter-class>
       </filter>

       <filter-mapping>
           <filter-name>characterEncodingFilter</filter-name>
           <!--此访问路径（不是项目路径）下的任何请求，都会经过这个过滤器-->
           <url-pattern>/servlet/*</url-pattern>
       </filter-mapping>
   ```

4. **Filter 链**

   ​ 在一个 web 应用中，可以开发编写多个 Filter，这些 Filter 组合起来称为一个 Filter 链。

   ​ web 服务器根据 Filter 在 web.xml 中的注册顺序，决定先调用哪个 Filter，当第一个 Filter 的 doFilter 方法被调用时，web 服务器会创建一个代表 Filter 链的 FilterChain 对象传递给该方法，在 doFilter 方法中，开发人员如果调用了 FilterChain 对象的 doFilter 方法，则 web 服务器会检查 FilterChain 对象中是否还有 filter，如果有，则调用第二个 filter，如果没有，则调用目标资源。

# 监听器

监听登录人数

```java
package com.kuang.listener;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class OnlineCountListener implements HttpSessionListener {
    //创建session监听，看你的一举一动
    //一旦创建session就会触发一次这个事践
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext ctx = se.getSession().getServletContext();
        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");
        if (onlineCount==null) {
            onlineCount = 1;
        }else {
            int count = onlineCount;
            onlineCount = count + 1;
        }
        ctx.setAttribute("onlineCount", onlineCount);
    }
    //销毁session监听
    public void sessionDestroyed(HttpSessionEvent se) {

    }
}

```

```java
<listener>
    <listener-class>com.kuang.listener.OnlineCountListener</listener-class>
</listener>
<session-config>
    <session-timeout>1</session-timeout>
</session-config>
```

# 过滤器监听器常见应用

用户登录后才能进入主页！用户注销后就不能进入主页了！

1. 用户登录之后，向 session 中放入用户的数据；
2. 进入主页的时候要判断用户是否已经登录；下面：在过滤器中实现；

```java
package com.kuang.filter;

import com.kuang.utils.Constant;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class SysFilter implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {
    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {

        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) resp;

        if (request.getSession().getAttribute(Constant.USER_SESSION)==null){
            response.sendRedirect("/sys/Error.jsp");
        }

        chain.doFilter(req,resp);
    }
    public void destroy() {
    }
}
```

# Junit 测试

`@Test`注解

只能在单元测试代码中使用
