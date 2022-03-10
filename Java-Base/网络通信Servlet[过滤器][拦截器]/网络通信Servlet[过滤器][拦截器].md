@[TOC](目录)
# 网络通信流程
> 现在大部分javaWeb开发都是BS结构
> BS——浏览器与服务器之间
> CS——客户端与服务器之间
> 用前端Html/css/js语言写的浏览器页面属于静态页面
> 需要通过获取服务器的数据而打到动态展示的效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/50352c1c1d4a478aa7df11c9121ebaa3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
***网络协议包：***
> 网络中传递的信息都是以二进制的形式存在
> 在浏览器接收到服务器的数据后都要将二进制信息编译为相应的图片，文字，视频等
> 网络协议包是一组有规律的二进制数据(在这组数据存在固定的空间后)，每一个空间专门存放特定信息

***Http网络协议包***
> 在基于BS结构下，所有传递的信息都是保存在Http网络协议包中
> 分类：
> **Http请求协议包(发送东西的时候)：**
> 在浏览器准备发送请求时，负责创建一个Http请求协议包
> 浏览器将请求信息以二进制的形式保存在Http请求协议包的各个空间
> 由浏览器负责将Http请求协议包发送到指定服务端计算机
> ****
> **Http响应协议包(接收东西的时候)：**
> Http服务器在定位到被访问的资源文件后
> 负责创建一个Http响应协议包
> Http服务器将定位文件内容或命令等以二进制形式写入到Http响应协议包各个空间中
> 由Http服务器负责将Http响应协议包发送回浏览器上

***Http请求协议包内部空间***
```bash
按照自上而下划分为四个空间：
=====================================================
请求行：
URL:请求地址比如：http://localhost:8080/index.html
method：请求方式[post/get]
=====================================================
请求头：
请求参数信息[GET]
=====================================================
空白行：
没有实际作用，隔离数据
=====================================================
请求体：
如果浏览器以post方式发送请求的话，存放请求参数信息
=====================================================
```
***Http响应协议包内部空间***
```bash
=====================================================
状态行：
Http状态码
=====================================================
响应头：
content-type:指定浏览器采用对应编译器对响应体数据解析
比如[text/html;charset=UTF-8]text文本信息html语言
=====================================================
空白行
=====================================================
响应体：
被访问的静态资源
被访问的动态资源结果
=====================================================
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/890f71aef9994a56b0d426365eb2780c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
# Http服务器分类：tomcat服务器安装
> tomcat下载官网：[tomcat官网](https://tomcat.apache.org/download-90.cgi)

![瞎子啊](https://img-blog.csdnimg.cn/d46030e6cd524ba884bcc329b98122dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
***tomcat服务器启动***
![在这里插入图片描述](https://img-blog.csdnimg.cn/a9acf4997ea54a3f9397320405f2f976.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
***访问服务器***
> 地址栏输入http://localhost:8080

![在这里插入图片描述](https://img-blog.csdnimg.cn/79b9a1543f71483babab5a476d5f829b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
***关闭服务器***
![在这里插入图片描述](https://img-blog.csdnimg.cn/38e6d56bdc45419692e690e17c3ffc03.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
***IDEA配置tomcat***
![在这里插入图片描述](https://img-blog.csdnimg.cn/44459eae09ca401495174078468a2c93.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_13,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0f182614d494449884291c9cbefeea69.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/bafe3990b73c46bebe07ab66648dcd6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/554b5e685ecf41aea2a68e0edcf97007.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/851bb77c8c6d41e7affe4383a05b9ad1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd5f98f17a2848949bc2180566417465.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
# 创建网站
![在这里插入图片描述](https://img-blog.csdnimg.cn/4a75b2d86e3b4c3eaabed48b1b8864cc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a95a0901b6e34a219b70dc21041f1d2a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/08ccc2a07dce49ff95ef9523ee22bead.png)
> web文件夹存放静态资源文件
> src文件夹存放动态资源的java文件
> WEB-INF存放依赖的jar包核心配置文件
> lib文件夹存放jar包
> web.xml核心配置文件和服务器互通

***导入jar包***
![在这里插入图片描述](https://img-blog.csdnimg.cn/f76355ea50234410822e0633ed3aa5bb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b0250a317f7c46018380cc00029422cc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_18,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/509abd05ec7243ec9abfd96e5ce4a78c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
# Servlet
***Servlet规范介绍***

> 1：在Servlet规范中，指定[动态资源文件]开发步骤
> 2：在Servlet规范中，指定Http服务器调用动态资源文件规则
> 3：在Servlet规范中，指定Http服务器管理动态资源文件实例对象规则
## Servlet接口实现类开发步骤
> Servlet接口（只能Servlet的实现类才可以调用动态资源文件）
> 使用：
> java类继承HttpServlet实现类
> 重写HttpServlet的方法:doGet()/doPost()
> 源码：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b6ba425f65c54386b07b593afa51b513.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d2b52446b3ba4b38ac4104502bf6013d.png)
> 继承HttpServlet而不是Servlet之后降低开发难度

***HttpServlet***
```java
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getMethod();//作用 获取请求方式get或post
        long lastModified;
        if (method.equals("GET")) {//如果以get方式请求
            lastModified = this.getLastModified(req);
            if (lastModified == -1L) {
                this.doGet(req, resp);//this可以是这个父类对象，也可以是继承HttpServlet的类对象
                //确认请求之后 tomcat会调用servlet new出新的servlet对象并且调用service方法
            } else {
                long ifModifiedSince;
                try {
                    ifModifiedSince = req.getDateHeader("If-Modified-Since");
                } catch (IllegalArgumentException var9) {
                    ifModifiedSince = -1L;
                }

                if (ifModifiedSince < lastModified / 1000L * 1000L) {
                    this.maybeSetLastModified(resp, lastModified);
                    this.doGet(req, resp);
                } else {
                    resp.setStatus(304);
                }
            }
        } else if (method.equals("HEAD")) {
            lastModified = this.getLastModified(req);
            this.maybeSetLastModified(resp, lastModified);
            this.doHead(req, resp);
        } else if (method.equals("POST")) {//doPost请求方法是被tomcat的new的servlet调用的service调用的doPost
            this.doPost(req, resp);
        } else if (method.equals("PUT")) {
            this.doPut(req, resp);
        } else if (method.equals("DELETE")) {
            this.doDelete(req, resp);
        } else if (method.equals("OPTIONS")) {
            this.doOptions(req, resp);
        } else if (method.equals("TRACE")) {
            this.doTrace(req, resp);
        } else {
            String errMsg = lStrings.getString("http.method_not_implemented");
            Object[] errArgs = new Object[]{method};
            errMsg = MessageFormat.format(errMsg, errArgs);
            resp.sendError(501, errMsg);
        }

    }
```
***Servlet创建***
```java
package com.javaweb.controller;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```
***将Servlet接口实现类类路径地址交给tomcat***
```html
    <servlet>
<!--        声明一个变量储存servlet接口实现类类路径-->
        <servlet-name>loginServlet</servlet-name>
        <servlet-class>类路径的路径</servlet-class>
    </servlet>

    <!--设置访问servlet接口的简称 降低访问难度-->
    <servlet-mapping>
        <servlet-name>loginServlet</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>
```
***web.xml文件***
```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
   <servlet>
       <servlet-name>NewController</servlet-name>
       <servlet-class>com.javaweb.controller.NewController</servlet-class>
   </servlet>
    <servlet-mapping>
        <servlet-name>NewController</servlet-name>
        <url-pattern></url-pattern>
    </servlet-mapping>
</web-app>
```
***默认主页面***
```html
通过在web.xml内配置以下信息设置默认主页面也可以自定义
	<welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
```
## Servlet对象生命周期
> 网站中所有的servlet接口实现类的实例对象，只能由Http服务器负责创建
> 在默认情况下Http服务器接收到对于当前servlet接口实现类第一次请求时
> 自动创建这个servlet接口实现类的实例对象

***手动配置Http服务器启动时自动创建servlet接口实现类***
```html
   <servlet>
       <servlet-name>NewController</servlet-name>
       <servlet-class>com.javaweb.controller.NewController</servlet-class>
       <load-on-startup>1</load-on-startup>
   </servlet>
    <servlet-mapping>
        <servlet-name>NewController</servlet-name>
        <url-pattern></url-pattern>
    </servlet-mapping>
```

> 在Http服务器运行时，一个servlet接口实现类只能被创建出一个实例对象
> Http服务器关闭时，会自动将实例对象销毁
## HttpServletResponse接口
> HttpServletResponse接口负责将doGet/doPost方法执行的结果写入到响应体中交给浏览器
> 可以设置响应头的content-type属性控制浏览器使用对应的编译器转二进制为数据
> 可以设置响应头的location属性，将一个请求地址赋值给location控制浏览器向指定服务器发送请求
```java
package com.javaweb.controller;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //创建执行结果
        String result = "执行结果";
        //设置响应头的content-type属性
        resp.setContentType("text/html;charset=UTF-8");
        //通过response响应对象将结果写到响应体
        PrintWriter pw = resp.getWriter();
        //写入结果
        pw.print(result);
    }
}
```
***web.xml***
```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
   <servlet>
       <servlet-name>NewController</servlet-name>
       <servlet-class>com.javaweb.controller.NewController</servlet-class>
   </servlet>
    <servlet-mapping>
        <servlet-name>NewController</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>
</web-app>
```
***结果：***
![在这里插入图片描述](https://img-blog.csdnimg.cn/62e868567df84dfbbe22b795b3186836.png)
***设置响应头的location属性***
```java
package com.javaweb.controller;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String result = "http://www.baidu.com";
        //通过响应对象，将地址赋值给响应头的location的属性
        resp.sendRedirect(result);
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ee048738e314f21b80d18db1bc19dc9.png)
## HttpServletRequest接口
> HttpServletRequest接口实现类由Http服务器负责提供和使用
> HttpServletRequest接口负责在doGet/doPost方法运行的时候，读取Http请求协议包中的信息
> 作用：
> 可以读取Http请求协议包中请求行的信息
> 可以读取保存在Http请求协议包中请求头或请求体的参数信息
> 可以代替浏览器向Http服务器申请资源文件的调用

***HttpServletRequest使用***
```java
package com.javaweb.controller;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //通过请求对象request，读取请求行中的url信息
        String url = req.getRequestURI().toString();
        System.out.println(url);
        //读取请求行中的method信息
        String method = req.getMethod();
        System.out.println(method);
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/5de8797ba39f4221aafca490e896c22a.png)
***HttpServletRequest接收参数***
> 添加index.html写超链接传id参数
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <a href="/web/login?id=1">超链接</a>
</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ce43f313ea6f4b1ebb2feb33b62f82d5.png)
```java
package com.javaweb.controller;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;

public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //接受所有参数，将参数保存在枚举中
        Enumeration values = req.getParameterNames();
        //遍历枚举
        while (values.hasMoreElements()){
            String value = (String)values.nextElement();
            //通过request的getParameter方法传入一个参数名称获取参数的值
            System.out.println(req.getParameter(value));
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d5ae8413826b4925a6e48830ce869fae.png)

> 从请求体和请求头中读取的方法是一样的req.getParameter("参数名");
> 保存在请求头和请求体的数据到达Http服务器后会进行解码
> tomcat负责get解码：[utf-8]
> 请求对象request负责post解码：[ISO-8859-1]
> 如果以post方式发送请求，在请求体中接收数据会造成乱码现象
> 在post请求下，在读取数据之前通知请求对象request使用utf-8字符集：req.setCharacterEncoding("urf-8");
## Request/Response生命周期
> 在Http服务器接收到浏览器发送的Http请求协议包之后
> Http服务器就会创建一个请求协议包request和响应协议包response并作为参数
> 根据请求的方式/doGet(request,response)/doPost(request,response)
> 当doGet/doPost运行完毕时，销毁掉请求对象request和响应对象response，然后将数据返回到浏览器页面
## Http状态码
> 状态码表示服务器资源的完整性
> Http服务器在推送响应包之前，根据本次请求处理情况
> 将Http状态码写入到响应包状态行中

![在这里插入图片描述](https://img-blog.csdnimg.cn/cbd772b1af18484fb73e26bf24a2fab1.png)
***Http状态码分类***

> 数字100到599之间每一个数字都是一种状态码
> 分为五个类：
> 1XX：
> 状态码：100（本次返回的资源文件不是独立的资源文件，需要浏览器接收到响应包之后继续向Http服务器索要资源）
> 2XX：
> 状态码：200（表示本次返回的资源文件是一个完整的资源文件，不需要做其他操作）
> 3XX：
> 状态码：302（本次返回的不是一个资源文件的内容，而是一个资源文件的地址，需要浏览器根据地址来自动发起请求来索要资源文件）
> 4XX：
> 状态码：404（本次请求没有定位到被访问的资源文件，无法提供相应的数据）
> 状态码：405（在服务端中已经定位到被访问的资源文件(servlet)，但是这个servlet对于浏览器采用的请求方式不能处理）
> 5XX：
> 状态码：500（已经定位到被访问的文件(servlet)，这个servlet可以接收浏览器请求方式，但是servlet在处理请求时，由于java异常导致失败）
## 多个Servlet之间调用规则
> 浏览器发起一次请求访问，通过Servlet调用Servlet
> 两个方案：
> 重定向：浏览器第一次通过手动方式发送请求，解决完第一个Servlet返回数据，然后浏览器自动发起请求调用第二个Servlet返回数据，然后自动请求第三个Servlet返回最终数据
> 命令：response.sendRedirect("请求地址");
> 特点：可以把网站内部的资源文件地址发送给浏览器(/网站名/资源文件名)
> 也可以把其他网站资源文件地址发送给浏览器(http://ip地址:端口号/网站名/资源文件名)
> 浏览器至少发送2次请求，浏览器自动发送的请求是get方式
> 缺点：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/716f0815d5bd4a4284a6d6a1e618f5d5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)转发：
> 通过当前请求对象生成资源文件申请报告
> 命令：request.getRequestDispatcher("/资源文件名");
> 将对象发送给tomcat：直接写成request.getRequestDispatcher("/资源文件名").forward(请求对象,响应对象);
> 优点：无论本次请求涉及多少个Servlet，只需要手动通过浏览器发送一次请求即可
> 节省服务器与浏览器之间的往返次数
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/11e5f5d731294880910e8441b7b89c26.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 多个Servlet之间数据共享实现方案
> 数据共享：将一个Servlet处理完的数据放在一个内存区域中，供所有Servlet使用
> Servlet规范中提供四种数据共享方案：
> 1：ServletContext接口
> 2：Cookie类
> 3：HttpSession接口
> 4：HttpServletRequest接口
### ServletContext接口
> 如果多个Servlet需要共享同一个数据，彼此之间通过网站的ServletContext实例对象实现数据共享
> ServletContext——全局作用域对象
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/123ee48175864dbe922c1ff1cb0b9f34.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

***ServletContext生命周期***
> 在Http服务器启动时，自动为当前网站在内存中创建一个全局作用域对象
> 在Http服务器运行期间，一个网站只有一个全局作用域对象并且一直处于存活状态
> 在Http服务器准备关闭的时候，会自动销毁当前网站的全局作用域对象

***ServletContext使用***
```java
package com.javaweb.controller;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //通过请求对象向tomcat获取全局作用域对象 命名一般为application也可以自定义
        ServletContext application = req.getServletContext();
        //将数据添加到全局作用域对象中
        application.setAttribute("key","value");
    }
}
```
***取出全局作用域对象的数据***
```java
package com.javaweb.controller;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //通过请求对象向tomcat获取全局作用域对象 命名一般为application也可以自定义
        ServletContext application = req.getServletContext();
        //获取数据 返回Object类型 需要强转为需要的类型
        String value = (String) application.getAttribute("key");
    }
}
```
### Cookie

> 如果两个Servlet需要为同一个浏览器提供服务，此时需要Cookie对象进行数据共享
> Cookie存放当前用户的私人数据，在共享数据的过程中提高服务质量
> 用户通过浏览器第一次向网站发送请求处理数据，Servlet在运行时会创建一个Cookie存储与当前用户相关的数据
> 在Servlet工作完毕时，将Cookie写入到响应头中返回浏览器，浏览器在接收到响应包之后，会将Cookie存储在浏览器的缓存中
> 当浏览器再次向服务器发起请求时，浏览器会把缓存中的Cookie也同时发送到服务器
> 在另一个Servlet运行时，可以通过请求头中的Cookie信息，得到第一个Servlet处理的数据
> ****
> 命令：Cookie cookie = new Cookie("key","value");
> Cookie相当于一个Map集合，存数据使用键值对的形式
![在这里插入图片描述](https://img-blog.csdnimg.cn/eae7df0be3dd475c9a99e271f7744018.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    	//存入Cookie数据
        Cookie cookie = new Cookie("key","value");
        //将Cookie写入到响应头发送浏览器
        resp.addCookie(cookie);
    }
}
```
```java
public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //读取请求头的Cookie
        Cookie[] cookies = req.getCookies();
        //得到Cookie中的数据
        for (Cookie cookie:cookies) {
            String key = cookie.getName();
            String value = cookie.getValue();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6b71c19318847ab82719f8ab8841d78.png)
***Cookie的生命周期***
> 默认情况下，Cookie会存储在浏览器的缓存中
> 当浏览器关闭时，Cookie会自动销毁
> 可以手动设置浏览器接收的Cookie存放在计算机的硬盘上，指定存活时间
> cookie对象.setMaxAge(时间);秒制
### HttpSession接口
> 【会话作用域对象】HttpSession
> ****
> **HttpSession与Cookie区别：**
> 1：存储位置：
> Cookie是存储在浏览器的缓存中在客户端计算机
> HttpSession存储在服务器端的计算机内存中
> 2：数据类型：
> Cookie数据类型只能是String
> HttpSession可以存储任意类型的共享数据Object
> 3：数据数量：
> 一个Cookie对象只能存储一个数据
> HttpSession使用Map集合存储数据，可以存储任意数量的数据
> ****
> 命令：
> HttpSession session = req.getSession();
> session.setAttribute("key","value");
> req.getSession().getAttribute("key");
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/40c46a79c6b24672968c33ef72c3fe54.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取服务器的HttpSession
        HttpSession session = req.getSession();
        session.setAttribute("key","value");
    }
}
```
```java
public class OldController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取服务器中的会话作用域对象
        String value = (String) req.getSession().getAttribute("key");
        System.out.println(value);
    }
}
```
***getSession()与getSession(false)***
> ***getSession()***
> 如果当前用户在服务端已经拥有了自己的HttpSession
> 要求tomcat将这个HttpSession返回
> 如果当前服务器没有这个用户的HttpSession，那tomcat会为当前用户创建一个全新的HttpSession
> ***getSession(false)***
> 如果当前用户在服务器有自己的HttpSession
> 要求tomcat将这个HttpSession返回数据
> 如果用户在服务端没有自己的HttpSession
> 此时tomcat返回null

***HttpSession生命周期***
> 默认情况下存储在服务器内存中
> 当浏览器关闭时，那用户与自己的HttpSession的联系被切断
> tomcat无法检测浏览器何时关闭，因此在浏览器关闭时不会导致tomcat将浏览器关联的HttpSession销毁掉，为解决这个问题，tomcat为每个HttpSession设置一个空闲时间
> 默认空闲时间为30分钟，如果当前HttpSession对象空闲到达30分钟，那tomcat会认为用户已经放弃HttpSession，此时tomcat会销毁这个HttpSession

***HttpSession空闲时间手动设置：***
***web.xml***
```html
	<session-config>
        <session-timeout>5</session-timeout><!--5表示5分钟-->
    </session-config>
```
### HttpServletRequest接口（实现数据共享）

> 两个Servlet之间通过请求转发的方式forward进行调用
> 此时两个Servlet是用同一个请求对象，可以利用这个请求对象实现数据共享
> HttpServletRequest【请求作用域对象】
> 命令：request.setAttribute("key","value");
> request.getRequestDispatcher("/资源地址").forward(request,response);
```java
public class NewController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置共享数据
        req.setAttribute("key","value");
        //设置请求转发
        req.getRequestDispatcher("/old").forward(req,resp);
    }
}
```
```java
public class OldController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取请求作用域对象
        String value = (String) req.getAttribute("key");
    }
}
```
# 监听器接口
> 监听器接口用于监控【作用域对象生命周期变化】以及【作用域对象共享数据变化】

> Servlet规范下的作用域对象：
> ServletContext：全局作用域对象
> HttpSession：会话作用域对象
> HttpServletRequest：请求作用域对象

> 监听器接口实现类开发规范（一般开发步骤都在三步及三步内，三步以上为拖慢开发速度）：
> 1：根据监听的实际情况，选择对应监听器接口进行实现
> 2：重写监听器接口生命【监听事件处理方法】
> 3：在web.xml文件将监听器接口实现类注册到Http服务器上

> ServletContextListener接口：
> 作用：
> 通过这个接口合法的检测全局作用域对象被初始化时刻以及被销毁时刻
> 监听事件处理方法：
> contextInitialized();-------在全局作用域对象被初始化的时候被调用
> contextDestroyed();-------在全局作用域对象被销毁的时候被调用

> ServletContextAttributeListener接口：
> 作用：
> 通过这个接口会对全局作用域共享的数据变化时刻
> 监听事件方法：
> attributeAdded();------在全局作用域对象添加共享数据的时候
> attributeReplaced();------在全局作用域对象更新共享数据的时候
> attributeRemoved();------在全局作用域对象删除共享数据的时候

***监听器实现***
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d48521dcb7d44e191fec2b4f123ae2e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c0a8ca62bb31486f8e5779b3420e4154.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
```java
public class Listener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("监听器开始");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("监听器结束");
    }
}
```
```java
public class AttributeListener implements ServletContextAttributeListener {
    @Override
    public void attributeAdded(ServletContextAttributeEvent scae) {
        System.out.println("数据添加");
    }

    @Override
    public void attributeRemoved(ServletContextAttributeEvent scae) {
        System.out.println("数据删除");
    }

    @Override
    public void attributeReplaced(ServletContextAttributeEvent scae) {
        System.out.println("数据更新");
    }
}
```
***web.xml***
```html
	<listener>
        <listener-class>com.javaweb.listener.Listener</listener-class>
    </listener>

	<listener>
        <listener-class>com.javaweb.listener.AttributeListener</listener-class>
    </listener>
```
# 过滤器接口
> Filter接口在Http服务器调用资源文件之前，对Http服务器进行拦截

> Filter接口实现类开发步骤：三步
> 1：创建一个java类实现Filter接口
> 2：重写Filter接口的doFilter方法
> 3：在web.xml将过滤器接口实现类注册到Http服务器
> ****
> doFilter方法内servletRequest参数和servletResponse都是被拦截的请求的请求体和响应体
> 如果请求合法则使用filterChain参数进行页面跳转：filterChain.doFilter(servletRequest,servletResponse);
>
![在这里插入图片描述](https://img-blog.csdnimg.cn/60c0e6dcf21a443a8cecceaa5dd004ac.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
```java
public class OneFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("拦截器执行");
        filterChain.doFilter(servletRequest,servletResponse);
    }
}
```
```html
<!--将拦截器交给tomcat-->
    <filter>
        <filter-name>OneFilter</filter-name>
        <filter-class>com.javaweb.filter.OneFilter</filter-class>
    </filter>
    <!--声明拦截器-->
    <filter-mapping>
        <filter-name>OneFilter</filter-name>
        <!--将需要拦截的资源告诉拦截器
        静态资源与动态资源都可以
        如果是目录下的所有资源则使用通配符：*
        所有请求：*.do或者*
        -->
        <url-pattern>/old</url-pattern>
    </filter-mapping>
```
