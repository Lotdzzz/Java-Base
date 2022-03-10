@[TOC](目录)
# 1：SpringMVC框架的认识
1：SpringMVC是spring的一个模块框架，专门做web开发的
2：SpringMVC开发底层是servlet，作用是servlet的功能增强
3：SpringMVC也可以把对象放到容器中管理，@Controller对象
## 1.1：中央调度器（DispatcherServlet）
> 中央调度器DispatcherServlet是Servlet对象
> 作用：
> 1：负责接收用户请求
> 2：扫描Controller对象
> 3：把请求转发给Controller对象，然后Controller对象处理请求
> 4：把返回结果发送到结果页面

**流程图：**
用户请求（index.jsp）——中央调度器（DispatcherServlet）——Controller对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/414b0f85248749df8b3a8c6adb0b4a83.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**中央调度器代码实现：**
**1：创建web项目**
![在这里插入图片描述](https://img-blog.csdnimg.cn/9249f940e13a4c1aa892acfee8c5bfb0.png)
**2：pom.xml加入依赖**
```html
    <dependency><!--servlet依赖-->
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>

    <dependency><!--springMVC的依赖-->
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.12</version>
    </dependency>
```
**3：配置web.xml（声明中央调度器对象）**
```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--声明注册SpringMVC的DispatcherServlet的对象
    执行过程：
    在tomcat启动后，会创建DispatcherServlet中央调度器对象
    然后这个中央调度器对象会读取<servlet-name>标签的springmvc文件（SpringMVC的配置文件）
    对象创建后会执行DispatcherServlet的init()方法底层原理:
        init(){
            //创建SpringMVC的容器
            WebApplicationContext ctx = new ClassPathXmlApplicationContext("springmvc.xml");
            //把容器对象放入到全局作用域中,这样容器只需要被创建一次则能被所有控制器使用
            getServletContext().setAttribute(key,ctx);
        }
    -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!--指定SpringMVC的配置文件位置-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        
        <!--load-on-startup表示在tomcat服务器启动后创建DispatcherServlet对象
        数值是一个整数：比0大的正整数
        数值越小表示创建Servlet对象的速度越快，假如有两个Servlet第一个数值1第二个数值2则会先创建第一个Servlet
        有顺序的
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    
        <!--声明springmvc的mapping-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--
        url-pattern语法：*.扩展名
        常用的：*.do , *.action , *.mvc
        比如*.do的作用是：扩展名为do的请求将会交给springmvc来调度
        比如http://localhost:8080/myWeb/some.do
        -->
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
</web-app>
```
**4：在resources文件夹下创建springmvc配置文件**
![在这里插入图片描述](https://img-blog.csdnimg.cn/e85fd958b386463e850cd518c3f0330b.png)
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!--声明组件扫描器，用来扫描Controller对象并放到容器中-->
    <context:component-scan base-package="com.javaWeb.controller"/>
</beans>
```
**5：创建控制器类**
配置tomcat：
![在这里插入图片描述](https://img-blog.csdnimg.cn/693ce23a2d5a453aa60a310ef0fe2c70.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
配置index.jsp页面：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<!--
some.do路径是http://localhost:8080/myWeb/
/some.do路径是http://localhost:8080
如果使用/some.do需要在前面加${pageContext.request.contextPath}
${pageContext.request.contextPath}/some.do
-->
<p><a href="some.do">发起some.do请求</a></p>
</body>
</html>
```
创建控制器类（用来处理请求）：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8f3e15e948ab4d759e527f4acd0cdc09.png)
```java
package com.javaWeb.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

/*
创建Controller对象，他会被springmvc配置文件扫描并放到容器中
 */
 
@Controller
public class MyController {

    /*
    准备用doSome方法来处理用户的请求：
    需要用到@RequestMapping注解：请求映射，作用是把一个地址和一个方法绑定在一起，用方法处理请求

    属性value：是请求地址url地址比如some.do，必须是唯一值
    RequestMapping修饰的方法叫做处理器方法
    是用来处理用户发起的请求，类似Servlet的doGet或doPost

    返回值：ModelAndView，作用是请求处理完成后，要显示的结果页面forward()
     */
     
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        /*
            处理请求
         */
        ModelAndView mv = new ModelAndView();
        //将结果添加到返回值中，底层是setAttribute
        //addObject(key,数据);
        mv.addObject("msg","执行doSome方法");
        //设置识图（结果页面）
        mv.setViewName("/result.jsp");
        return mv;//返回之后才会做setAttribute方法或forward方法等
    }
}
```
创建识图页面（结果页面）：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h3>msg数据:${msg}</h3>
</body>
</html>
```
**测试：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/a30c6a8d8194496cac2d564c6b2e26d9.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/be9b990807fe467b996625b595085996.png)
## 1.2：配置识图解析器
为了防止用户跳过index.jsp直接访问some.do请求的result.jsp需要将结果页面result.jsp切换文件夹
将结果页面result.jsp放入WEB-INF文件夹下，资源保护防止直接访问result.jsp
![在这里插入图片描述](https://img-blog.csdnimg.cn/c26b9729b37e48d7826cc1958592fbbe.png)
**在springmvc的配置文件中声明视图解析器：**
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.javaWeb.controller"/>

    <!--声明视图解析器，作用是简写被资源保护文件的路径
    比如原路径：/WEB-INF/view/result.jsp现在可以直接写成result
    -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--表示需要配置识图解析器的路径-->
        <property name="prefix" value="/WEB-INF/view/"/>
        <!--表示文件的后缀名-->
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```
**修改控制器的处理结果页面：**
```java
@Controller
public class MyController {
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","执行doSome方法");
        //在把文件资源保护后原路径：/WEB-INF/view/result.jsp
        //在添加资源解析器后可以直接写成result
        mv.setViewName("result");
        return mv;
    }
}
```
## 1.3：转发forward/重定向redirect
使用ModelAndView的转发forward和重定向redirect来跳转页面
作用：
当文件资源不在识图解析器配置的路径内，可以使用转发和重定向到达页面（必须写完整路径）
重定向redirect：无法访问WEB-INF下的资源
转发forward：可以访问WEB-INF的资源
```java
@Controller
public class MyController {
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("forward:/result.jsp");
        return mv;
    }
}
```
```java
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("redirect:/result.jsp");
        return mv;
    }
```
**使用redirect的时候发送一条请求实际处理两条请求，所以是两个request所以结果页面接收参数需要使用EL表达式：${param.参数名} 来接收参数**
# 2：SpringMVC注解式
## 2.1：@RequestMapping第二种用法
@RequestMapping不但可以放在方法上面，还可以放在类上面：
```java
@Controller
//放在类上面相当于处理请求的地址变成/user/some.do
@RequestMapping(value = "/user")
public class MyController {
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","执行doSome方法");
        mv.setViewName("result");
        return mv;
    }
}
```
**发起请求的主页面（发送请求使用user/some.do）：**
```html
<html>
<head>
    <title>Title</title>
</head>
<body>
<p><a href="user/some.do">发起some.do请求</a></p>
</body>
</html>
```
## 2.2：请求方式Method属性
@RequestMapping还有Method属性，是用来指定使用get请求处理还是post处理
```java
@Controller
@RequestMapping(value = "/user")
public class MyController {
    /*
    method属性是个枚举值，默认不加method属性情况下get和post都可以处理
 
    GET,
    HEAD,
    POST,
    PUT,
    PATCH,
    DELETE,
    OPTIONS,
    TRACE;
     */
    @RequestMapping(value = "/some.do",method = RequestMethod.GET)
    public ModelAndView doSome(){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","执行doSome方法");
        mv.setViewName("result");
        return mv;
    }
}
```
### 2.2.1：Request接收参数

> Controller控制器接收参数的方式：
>  HttpServletRequest 
>  HttpServletResponse
> HttpSession

**需要在处理请求的方法参数上加上这三个参数：**
```java
@Controller
@RequestMapping(value = "/user")
public class MyController {
    @RequestMapping(value = "/some.do",method = RequestMethod.GET)
    //加入接收请求参数的参数：HttpServletRequest request, HttpServletResponse response, HttpSession session
    //接收参数的方法和Servlet一样：request.getParameter("参数");
    public ModelAndView doSome(HttpServletRequest request, HttpServletResponse response, HttpSession session){
        String name = request.getParameter("name");
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","执行doSome方法");
        mv.addObject("name",name);
        mv.setViewName("result");
        return mv;
    }
}
```
### 2.2.2：接收用户提交的参数
**逐个接收：**

请求页面（传入的参数名name）：
```html
<html>
<head>
    <title>Title</title>
</head>
<body>
<form action="user/some.do">
    姓名<input type="text" name="name">
    <input type="submit">
</form>
</body>
</html>
```
处理页面：
```java
@Controller
@RequestMapping(value = "/user")
public class MyController {
    @RequestMapping(value = "/some.do",method = RequestMethod.GET)
    /*
    处理请求的方法的参数必须和请求中的参数名一样
    同名的请求参数赋给同名的控制器方法的参数
     */
    public ModelAndView doSome(String name){//加入参数

        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","执行doSome方法");
        mv.addObject("name",name);
        mv.setViewName("result");
        return mv;
    }
}
```
显示结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/18afb546e28e4829aa67f55ee925daeb.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc7f5ad77ee6455799f04b0583aff673.png)
**对象接收：**
```java
@Controller
@RequestMapping(value = "/user")
public class MyController {
    @RequestMapping(value = "/some.do",method = RequestMethod.GET)
    /*
    处理方法参数是个对象，框架会自动调用构造创建对象，请求参数name会自动调用setName方法传入name数据值
     */
    public ModelAndView doSome(Student student){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","执行doSome方法");
        mv.addObject("name",student.getName());//使用对象的get方法获取数据
        mv.setViewName("result");
        return mv;
    }
}
```
### 2.2.3：配置过滤器设置页面编码
**在web.xml文件中声明过滤器**
```html
    <!--声明过滤器
    作用：将指定页面在访问时设置页面编码，主要是解决post请求乱码问题
    -->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--设置编码-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <!--强制请求对象（HttpServletRequest）使用encoding编码的值-->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!--强制响应对象（HttpServletResponse）使用encoding编码的值-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <!--声明characterEncodingFilter的Mapping指定哪些文件需要过滤器-->
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <!--设置所有页面都需要过滤器-->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
### 2.2.4：@RequestParam注解
**请求中的参数名和处理方法的参数名不一样时，需要使用@RequestParam注解**
假如请求页面的参数是Rname：
```html
姓名<input type="text" name="Rname">
```
```java
@Controller
@RequestMapping(value = "/user")
public class MyController {
    @RequestMapping(value = "/some.do",method = RequestMethod.GET)
    /*
    在接收参数前使用@RequestParam注解可以让请求参数和处理参数不同也可是使用
    属性value表示请求参数的名字
    required是个布尔类型，表示这个参数是否可以为空值，默认是true
     */
    public ModelAndView doSome(@RequestParam(value = "Rname",required = false) String name){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","执行doSome方法");
        mv.addObject("name",name);
        mv.setViewName("result");
        return mv;
    }
}
```
## 2.3：处理请求方法的返回值
**返回值ModelAndView**
返回值是ModelAndView是同时处理数据（接收参数）和视图（页面跳转）的

**返回值String**
如果返回值只是一个跳转页面操作只需要用字符串就可以
```java
@Controller
@RequestMapping(value = "/user")
public class MyController {
    @RequestMapping(value = "/some.do",method = RequestMethod.GET)
    public String doSome(){
        //内部会执行forward方法
        return "result";
    }
}
```
**返回值void**
返回值void表示不做数据处理也不做视图页面跳转
主要作用是在ajax从控制器打印数据时，接收print而已
```java
@Controller
@RequestMapping(value = "/user")
public class MyController {
    @RequestMapping(value = "/some.do",method = RequestMethod.GET)
    public void doSome(HttpServletRequest request, HttpServletResponse response, HttpSession session) throws IOException {
        PrintWriter pw = response.getWriter();
        //假设数据处理完成后返回ajax的json格式的数据，这样要用到void返回值，不需要做任何处理
        pw.println("ajax的json数据格式");
        pw.flush();
        pw.close();
    }
}
```
**返回值Object**
返回值Object表示返回只是数据，和视图无关
数据可以是Integer，String等基础数据类型还有合法类型等
### 2.3.1：用处理器的返回值实现ajax的数据接收
> 步骤：
> 1：加入依赖：加入处理json工具库的依赖，springmvc默认使用的是jackson杰克逊工具
> ****
> 2：在springmvc的配置文件加入\<mvc:annotation-driven>注解驱动
> 底层原理：
> 注解驱动的作用是完成java对象转换成jason，xml，text，二进制等数据格式的转换
> 它有一个HttpMessageConverter接口，这个接口的实现类就是用来做数据格式转换的
> ****
> 3：在处理器方法上加入@ResponseBody注解

**实现过程：**
1：加入依赖：
```html
	<dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.12.3</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.12.3</version>
    </dependency>
```
2：创建ajax页面（index.jsp）：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="res/jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn").on("click",function (){
                var StuName = $("#StudentName").val();
                $.ajax({
                    data:{
                        name:StuName
                    },
                    dataType:"json",
                    success:function (resp){
                        window.alert(resp.name);
                    },
                    url:"some.do"
                })
            })
        })
    </script>
</head>
<body>
<div>
    姓名<input type="text" id="StudentName">
    <input type="button" id="btn" value="提交">
</div>
</body>
</html>
```
5：在springmvc配置页面注册驱动：

```html
<mvc:annotation-driven></mvc:annotation-driven>
```
4：配置处理器页面：
```java
@Controller
public class MyController {

    @RequestMapping(value = "/some.do")
    @ResponseBody//这个注解的作用是要把控制器处理完的对象转为json对象通过响应体HttpServletResponse输出给浏览器
    public Student doSome(String name) throws IOException {
        Student student = new Student(name);
        return student;
    }
}
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/53e22c841aeb4d3e85b2b8aee205e945.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 2.4：url-pattern（静态资源问题）
**项目中的index.jsp（默认主页面）还有静态资源：jpg或html等都归tomcat处理（跟框架无关）**
**项目中的控制器处理的请求，比如\*.do，\*.xml，\*.action等都是归springmvc框架处理的**

在tomcat服务器中的web.xml配置文件中有一项配置：
```html
    <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```
名为default的servlet
它是处理静态资源的配置，还可以处理未映射的请求（没有在web.xml映射Mapping的servlet是未映射）

如果想用springmvc处理静态资源的话需要将tomcat的default覆盖：
在自己项目的web.xml中修改springmvc的Mapping：
```html
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>

        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--
        使用/表示静态资源为springmvc处理的
        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```
第一种方式处理静态资源：然后在springmvc的配置文件加入标签：default-servlet-handle
```html
<!--因为default-servlet-handle和@RequestMapping有冲突，需要加入注解驱动-->
<mvc:annotation-driven></mvc:annotation-driven>
<!--加入标签把静态资源交给tomcat的default处理-->
<mvc:default-servlet-handler></mvc:default-servlet-handler>
```
第二种方式处理静态资源：在springmvc的配置文件加入标签resources：
```html
    <!--这个标签不依赖tomcat服务器，框架自己处理静态资源
    mapping访问的url地址，可以使用通配符**
    location静态资源位置
    -->
    <mvc:resources mapping="**" location="/res"/>
```
## 2.5：动态获取项目地址
只针对一个页面文件有效\<base>标签：

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    String basePath = request.getScheme() + "://" +
        request.getServerName() + ":" + request.getServerPort() +
        request.getContextPath() + "/";
%>
<html>
<head>
    <title>Title</title>
    <base href="<%=basePath%>"/>
</head>
<body>
<a href="some.do"></a>
</body>
</html>
```
标签作用：让页面的所有href可以省去地址路径，只需要写资源名或请求就可以
# 3：SSM三大框架整合开发（Spring SpringMVC Mybatis）
SSM三大框架整合：spring集成Mybatis再配置springmvc
步骤：
## 3.1：创建项目结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/91a7a59e20c345d296ed18885793b42c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_15,color_FFFFFF,t_70,g_se,x_16)
## 3.2：加入spring，mybatis，springmvc依赖
```html
	<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.3.13</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.7</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.6</version>
    </dependency>

    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.27</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>5.3.13</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.2</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.12</version>
    </dependency>
```
加入读取配置文件的插件：
```html
    <resources>
      
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>

      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
      
    </resources>
```
## 3.3：创建主配置文件applicationContext，spring，springmvc，mybatis，jdbc，web.xml等配置文件
mybatis配置文件：
```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <mappers>
        <package name="com.javaWeb.dao"/>
    </mappers>

</configuration>
```
jdbc配置文件：
```html
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/ccxg
name=root
password=123456
```
spring配置文件：
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:property-placeholder location="classpath:conf/jdbc.properties"></context:property-placeholder>
    <bean id="DataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <property name="driverClassName" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${name}"/>
        <property name="password" value="${password}"/>
    </bean>
    
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="DataSource"></property>
        <property name="configLocation" value="classpath:conf/mybatis.xml"></property>
    </bean>
    
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
        <property name="basePackage" value="com.javaWeb.dao"></property>
    </bean>
    
	<context:component-scan base-package="com.javaWeb.service"></context:component-scan>
</beans>
```
springmvc配置文件：
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.javaWeb.controller"></context:component-scan>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
    <mvc:annotation-driven></mvc:annotation-driven>
</beans>
```
主配置文件applicationContext：
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="classpath:conf/spring-web.xml"/>
    <import resource="classpath:conf/spring-mvc.xml"/>
</beans>
```
web.xml配置文件：
```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>dispatcherservlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:conf/applicationContext.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcherservlet</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
</web-app>

    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
	<context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:conf/applicationContext.xml</param-value>
    </context-param>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
```
配置tomcat：
![在这里插入图片描述](https://img-blog.csdnimg.cn/872e7f2f5a5043faa811b841bfdc128e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 3.4：创建dao,entity,controller,service层的对象
**entity：**
```java
package com.javaWeb.entity;

public class User {
    private Integer id;
    private String username;
    private String password;
    private String sex;
    private String email;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", sex='" + sex + '\'' +
                ", email='" + email + '\'' +
                '}';
    }

    public User() {
    }

    public User(Integer id, String username, String password, String sex, String email) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.sex = sex;
        this.email = email;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```
**dao：**
```java
package com.javaWeb.dao;

import com.javaWeb.entity.User;
import java.util.List;

public interface UserDao {
    List<User> SelectUsers();
}
```
```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.javaWeb.dao.UserDao">
    <select id="SelectUsers" resultType="com.javaWeb.entity.User">
        select id,username,password,sex,email from usertable
    </select>
</mapper>
```
**service：**
```java
package com.javaWeb.service;

import com.javaWeb.entity.User;

import java.util.List;

public interface UserService {
    List<User> SelectUsers();
}
```
```java
package com.javaWeb.service.impl;

import com.javaWeb.dao.UserDao;
import com.javaWeb.entity.User;
import com.javaWeb.service.UserService;
import org.springframework.stereotype.Service;
import javax.annotation.Resource;
import java.util.List;

@Service
public class UserServiceImpl implements UserService {
    @Resource
    private UserDao dao;

    @Override
    public List<User> SelectUsers() {
        return dao.SelectUsers();
    }
}
```
**controller：**
```java
package com.javaWeb.controller;

import com.javaWeb.entity.User;
import com.javaWeb.service.UserService;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import javax.annotation.Resource;
import java.util.List;

@Controller
@RequestMapping(value = "/user")
public class UserController {
    @Resource
    private UserService service;

    @RequestMapping(value = "/selectUsers.do")
    @ResponseBody
    public List<User> SelectUsers(User user){
        return service.SelectUsers();
    }
}
```
## 3.5：创建页面并测试
index.jsp：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="js/jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn").on("click",function (){
                $.ajax({
                    url:"user/selectUser.do",
                    data:{},
                    dataType:"json",
                    success:function (resp){
                        $("#data").empty();
                        $.each(resp,function (i,e){
                            $("#data").append("<tr>")
                            .append("<td>"+e.id+"</td>")
                            .append("<td>"+e.username+"</td>")
                            .append("<td>"+e.password+"</td>")
                            .append("<td>"+e.sex+"</td>")
                            .append("<td>"+e.email+"</td>")
                            .append("</tr>")
                        })
                    }
                })
            })
        })
    </script>
</head>
<body>
<div align="center">
    <table>
        <thead>
        <tr>
            <td>编号</td>
            <td>姓名</td>
            <td>密码</td>
            <td>性别</td>
            <td>邮箱</td>
        </tr>
        </thead>
        <tbody id="data">

        </tbody>
    </table>
    <input type="button" id="btn" value="查询">
</div>
</body>
</html>
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ae7ba4fb82a472ebc0c842fb9069ca7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/674d2ca023aa4fa58cfdc6c9ea6f940f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
# 4：集中统一处理异常
springmvc框架采用全局统一的异常处理
把controller的所有异常都集中到一个地方处理，底层采用aop技术
需要使用的注解：
@ExceptionHandler
@ControllerAdvice
步骤：
## 4.1：创建自定义的异常类
![在这里插入图片描述](https://img-blog.csdnimg.cn/284320923d0e4fb1b515296a8bd78ebb.png)
```java
package com.javaWeb.exception;

/*
创建异常类，创建子类继承，实现抛出异常的功能
 */
public class UserException extends Exception{
    public UserException() {
        super();
    }

    public UserException(String message) {
        super(message);
    }
}
```
```java
package com.javaWeb.exception;

/*
抛出异常对象的类
 */
public class UserMessageException extends UserException{
    public UserMessageException() {
        super();
    }

    public UserMessageException(String message) {
        super(message);
    }
}
```
在controller层抛出异常：
```java
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome() throws UserException {//抛出异常建议要在声明处抛出异常的父类
        ModelAndView mv = new ModelAndView();
        throw new UserMessageException("信息异常已抛出");
    }
```
## 4.2：创建普通类作为全局异常处理类
![在这里插入图片描述](https://img-blog.csdnimg.cn/39d155d2e8584e899260ec88e0f9858f.png)
```java
package com.javaWeb.handler;

import com.javaWeb.exception.UserMessageException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.ModelAndView;

/*
@ControllerAdvice控制器增强：目前作用是增加异常处理功能
需要在mvc配置文件声明
 */
@ControllerAdvice
public class UserExceptionHandler {

    /*
    处理异常，方法声明跟Controller的方法定义一样
    方法形参Exception是抛出的异常对象，通过参数可以获取异常的信息
    需要加注解：@ExceptionHandler(异常.class)
    异常.class当发生这个异常时，就用这个方法来处理
    UserMessageException.class表示当controller层发生这个异常时，就用这个方法处理
     */
    @ExceptionHandler(value = UserMessageException.class)
    public ModelAndView UserMessageException(Exception ex){
        /*
        异常发生时需要做的事：
        记录异常发生的时间
        记录异常是哪个方法发出的
        发送异常通知
        
        能力有限目前只做异常提示
         */
        ModelAndView mv = new ModelAndView();
        mv.setViewName("result");
        return mv;
    }
    
    /*
    @ExceptionHandler无属性写法，表示默认其他异常用这个方法处理
    除UserMessageException异常以外的异常都用这个方法处理
     */
    @ExceptionHandler
    public ModelAndView OtherException(Exception ex){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("result");
        return mv;
    }
}
```
## 4.3：组件扫描器，注解驱动
```html
<!--扫描handler处理异常的类所在的包-->
<context:component-scan base-package="com.javaWeb.handler"/>
<!--声明注解驱动-->
<mvc:annotation-driven/>
```
# 5：拦截器
拦截器作用是判断用户请求的权限，合法性等
![在这里插入图片描述](https://img-blog.csdnimg.cn/832a70297f5749f0ad8953972e5c36f8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 5.1：创建拦截器类
![在这里插入图片描述](https://img-blog.csdnimg.cn/967b9e15eec040d4ad5250cd602bf415.png)
```java
package com.javaWeb.handler;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class Interceptor implements HandlerInterceptor {
    /*
    预处理方法
    参数Object handler是被拦截的控制器对象controller对象
    方法返回值是true的话表示请求通过
    false表示请求拦截

    这个方法的主要作用是验证用户请求的合法性
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return true;
    }

    /*
    后处理方法
    在控制器方法之后执行的，能获取处理器的ModelAndView返回值
    主要作用做第二次修正
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        modelAndView.setViewName("result");
    }

    /*
    最后执行的方法
    在请求处理完成后执行的（视图处理完成后就认为请求处理完成）
    主要作用是做资源回收
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    }
}
```
## 5.2：声明拦截器
```html
    <!--声明拦截器
    拦截器可以有很多个
    -->
    <mvc:interceptors>
        <!--声明第一个拦截器
        path表示需要拦截器拦截请求的
        bean表示拦截器类的目录
        -->
        <mvc:interceptor>
            <mvc:mapping path="/user/**/"/>
            <bean class="com.javaWeb.handler.Interceptor"/>
        </mvc:interceptor>
		<!--如果声明多个拦截器那么请求会按照拦截器的声明顺序来进入拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/user/**/"/>
            <bean class="com.javaWeb.handler.Interceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```
拦截器的声明底层是用AarrayList集合存储的
![在这里插入图片描述](https://img-blog.csdnimg.cn/438f0bd4829c483c90925df7b737e3e4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 5.3拦截器和过滤器的区别

> 1：过滤器是servlet中的对象，拦截器是框架中的对象
> 2：过滤器实现Filter接口的对象，拦截器是HandlerInterceptor
> 3：过滤器是用来设置request，response的参数属性的，侧重数据过滤
> 拦截器用来验证请求合法性，能截断请求
> 4：过滤器是在拦截器之前执行的
> 5：过滤器是tomcat服务器创建的对象，拦截器是springmvc容器中的对象
> 6：过滤器是一个执行时间点，拦截器是三个执行时间点
> 7：过滤器可以处理jsp，js，html等，拦截器是对Controller对象进行拦截，如果请求不会被中央调度器接收，那么拦截器也收不到
> 8：拦截器拦截普通类方法执行（Controller控制器的方法），过滤器过滤servlet请求响应
#  6：springmvc的执行流程（映射器的介绍）
![在这里插入图片描述](https://img-blog.csdnimg.cn/0e3cc30df1894edc85e4d54a5cedda35.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**springmvc处理流程：**
> 1：用户发起请求：some.do
> ****
> 2：DispatcherServlet中央调度器接收请求some.do，把请求转交给处理器映射器
> 处理器映射器作用：springmvc框架中的一个对象，框架把实现了HandlerMapping接口的类叫做映射器（有很多个），框架把处理器对象放到一个叫做处理器执行链（HandlerExecutionChain）的类里保存
> HandlerExecutionChain的类里保存着：处理器对象（Controller对象），拦截器（List\<HandlerInterceptor>）
> ****
> 3：中央调度器（DispatcherServlet）把处理器映射器的HandlerExecutionChain中的处理器对象交给了处理器适配器对象（有很多个）
> 处理器适配器：springmvc框架的对象，实现了HandlerAdapter接口的对象
> 适配器作用：执行处理器方法（执行Controller对象的方法得到返回值ModelAndView）
> ****
> 4：中央调度器（DispatcherServlet）把处理器适配器中获取的返回值ModelAndView交给视图解析器对象
> 视图解析器：实现了ViewResoler接口的对象（有多个）
> 视图解析器作用：使用前缀后缀组成视图完整路径，然后创建view对象
> View是一个接口表示视图的，在框架中的jsp，html不是String表示，而是使用View和他的实现类表示视图
> 比如：mv.setViewName("result");执行完后这个result会变成路径：/WEB-INF/jsp/result.jsp
> 会被转成View对象：相当于mc.setView(new ForwardView("/WEB-INF/jsp/result.jsp"));转发
InternalResourceView：视图类，表示jsp文件，视图解析器会创建InternalResourceView对象
这对象里有个属性叫url=/WEB-INF/jsp/result.jsp
> ****
>5：中央调度器（DispatcherServlet）把视图解析器中创建的View对象获取到，调用View类的方法
>把返回值Model数据放入request作用域中，执行对象视图的forward，请求结束。
