@[TOC](目录)
# Dubbo框架介绍
> Dubbo框架是一个高性能的RPC框架，解决了分布式中的调用问题
> 服务器调用另一个服务器的业务服务
> 因Dubbo使用的Serializable接口序列化，使用的是二进制流（效率最高）
> ****
> 三大核心能力：
> 1：面向接口的远程方法调用
> 2：智能容错和负载均衡
> 3：服务自动注册和发现
> 官网：[Dubbo](https://dubbo.apache.org/zh/)

***基本架构***
![在这里插入图片描述](https://img-blog.csdnimg.cn/a89f92808d5841ad8f913f03ee1bf4c5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

> Dubbo步骤：
> 1：创建服务提供者（Provider）- （Container）spring容器
> 2：注册到注册中心（Register）
> 3：消费者(Consumer)去注册中心订阅服务
> 4：调用服务（invoke）
> 5：监控中心（Monitor）监控运行状态

> dubbo支持的协议：
> dubbo，hessian，rmi，http，webservice，thrift，memcached，redis
> dubbo协议默认端口：20880
# Dubbo使用——直连方式
## 1：创建服务提供者

***创建maven项目***
![在这里插入图片描述](https://img-blog.csdnimg.cn/ca848264e3ba40f29faa0eb2d206a52d.png)
***加入依赖***
```html
  <dependencies>
	<!--测试单元-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
	<!--spring依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.12</version>
    </dependency>
	<!--springmvc依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.12</version>
    </dependency>
	<!--dubbo依赖-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dubbo</artifactId>
      <version>2.6.2</version>
    </dependency>

  </dependencies>
```
***创建服务***
![在这里插入图片描述](https://img-blog.csdnimg.cn/33043187c16546f8ac28f189dd6749d0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
User类
注：需要序列化，实现接口Serializable
```java
package com.dubbo.domain;

public class User implements Serializable{
    private Integer id;
    private String name;
    public User() {}
    public Integer getId() {return id;}
    public void setId(Integer id) {this.id = id;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
}
```
UserService接口
```java
package com.dubbo.service;

import com.dubbo.domain.User;

public interface UserService {
    User queryUserById(Integer id);
}
```
实现类
```java
package com.dubbo.service.impl;

import com.dubbo.domain.User;
import com.dubbo.service.UserService;

public class UserServiceImpl implements UserService {
    @Override
    public User queryUserById(Integer id) {
        User user = new User();
        user.setId(id);
        user.setName("张三");
        return user;
    }
}
```
## 2：将服务注册到注册中心
![在这里插入图片描述](https://img-blog.csdnimg.cn/664a8143efea48099c3e16557e559efe.png)
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!--服务器提供者声明名称，名称需要保证唯一性-->
    <dubbo:application name="provider"/>

    <!--访问服务协议的名称端口号，Dubbo默认推荐20880
    name:指定协议的名称
    port:指定协议的端口号
    -->
    <dubbo:protocol name="dubbo" port="20880" />

    <!--暴露服务
    interface:服务接口的全限定名
    ref:服务接口的实现类
    registry:N/A使用直连方式
    -->
    <dubbo:service interface="com.dubbo.service.UserService" ref="userServiceImpl" registry="N/A"/>

    <!--将实现类加载到spring容器中-->
    <bean id="userServiceImpl" class="com.dubbo.service.impl.UserServiceImpl"/>

</beans>
```
***配置监听器***
```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-dubbo.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    
</web-app>
```
## 3：创建消费者订阅注册中心的服务
***创建消费者的maven项目***
> 先将提供服务者的maven项目打包部署到本地仓库
> 记得要将pom.xml文件内修改
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/58e1bbcc308c48c588f61d59c848d2ff.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/100b1294a213455f983a8829e84d33c5.png)
> 打包完修改回
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0ac16843e0414649826bb68dadf0c15b.png)
> 
***加入消费者项目的依赖***
```html
 <dependencies>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dubbo</artifactId>
      <version>2.6.2</version>
    </dependency>
	<!--提供服务者的依赖也需要加入-->
    <dependency>
      <groupId>com.dubbo</groupId>
      <artifactId>java-dubbo-provider</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
    
  </dependencies>
```
***创建控制器Controller***
![在这里插入图片描述](https://img-blog.csdnimg.cn/32e864a7327e4828a0c61d2689db9154.png)
***消费者订阅注册中心的服务***
![在这里插入图片描述](https://img-blog.csdnimg.cn/1f4ff55316c74fedb5f8d9bd4a00fba4.png)
***spring-dubbo***
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!--声明服务消费者的名称，名称也需要唯一性-->
    <dubbo:application name="consumer"/>

    <!--
    引用远程服务接口
    id为远程服务接口的对象名称
    interface调用远程接口的全限定名
    url访问服务接口的地址
    registry为直接连接方式
    -->
    <dubbo:reference id="userServiceImpl" interface="com.dubbo.service.UserService" url="dubbo://localhost:20880" registry="N/A"/>
</beans>
```
***spring-mvc***
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--扫描组件-->
    <context:component-scan base-package="com.dubbo.web"/>

    <!--配置注解的驱动-->
    <mvc:annotation-driven/>

<!--    识图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    
</beans>
```
## 4：调用服务方法
```java
@Controller
public class UserController {

    @Resource
    private UserService userService;

    @RequestMapping(value = "/user.do")
    public String user(Model model,Integer id){
        model.addAttribute("user",userService.queryUserById(id));
        return "user";
    }
}
```
***设置跳转页面***
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>用户详情</h1>
<div>用户标识:${user.id}</div>
<div>用户名称:${user.name}</div>
</body>
</html>
```

> 启动两个网站的tomcat实现：消费者服务器调用提供服务者服务器的service服务
![在这里插入图片描述](https://img-blog.csdnimg.cn/dcf96e517c0b4fbe9db49beb23d2455e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
# Dubbo使用——注册中心
> zookeeper——注册中心
> 作用：对提供服务者的服务进行管理
> 通过版本号对一个接口服务对应多个实现类的管理
## 1：注册中心安装zookeeper
> 安装管网：[zookeeper](https://downloads.apache.org/zookeeper/)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/6a886f808e684c8abd59fc962e29a1c4.png)
> 在conf目录下复制粘贴一份zoo_sample.cfg
> 将新的zoo_sample.cfg改名为zoo.cfg
> 进入zoo文件内在clientPort=2181下一行添加admin.serverPort=8888
> 启动服务：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/b2233fbab141461e9872f437aeea8700.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/de84419b869c4a989accfb1acccffcb2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 2：创建接口（interface）项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/7e94fb7cf2954049b32611c25ef50074.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_17,color_FFFFFF,t_70,g_se,x_16)
***User***
```java
package com.dubbo.domain;
import java.io.Serializable;
public class User implements Serializable {
    private Integer id;
    private String name;
    public User() {}
    public Integer getId() {return id;}
    public void setId(Integer id) {this.id = id;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
}
```
***UserService***
```java
package com.dubbo.service;
import com.dubbo.domain.User;
public interface UserService {
    User queryUserById(Integer id);
}
```
## 3：创建提供服务者
![在这里插入图片描述](https://img-blog.csdnimg.cn/c369a94f51a14b95b8e4661882c227b5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
***UserServiceImpl***
```java
package com.dubbo.service.impl;
import com.dubbo.domain.User;
import com.dubbo.service.UserService;
public class UserServiceImpl implements UserService {
    @Override
    public User queryUserById(Integer id) {
        User user = new User();
        user.setId(id);
        user.setName("张三");
        return user;
    }
}
```
***UserServiceImpl2***
```java
package com.dubbo.service.impl;
import com.dubbo.domain.User;
import com.dubbo.service.UserService;
public class UserServiceImpl2 implements UserService {
    @Override
    public User queryUserById(Integer id) {
        User user = new User();
        user.setId(id);
        user.setName("李四");
        return user;
    }
}
```
***依赖***
```html
<dependencies>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dubbo</artifactId>
      <version>2.6.2</version>
    </dependency>

    <dependency>
      <groupId>com.dubbo</groupId>
      <artifactId>java-dubbo-interface</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
	<!--zookeeper的依赖-->
	<dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-framework</artifactId>
      <version>4.1.0</version>
    </dependency>

  </dependencies>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa87650fe3444df8b1538de87d77b459.png)
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="provider"/>

    <dubbo:protocol name="dubbo" port="20880" />

    <!--使用zookeeper注册中心
    指定注册中心的地址和端口号
    -->
    <dubbo:registry address="zookeeper://localhost:2181" />

    <!--一个接口有多个接口实现类需要加version版本号区分功能-->
    <dubbo:service interface="com.dubbo.service.UserService" ref="userServiceImpl" version="1.0.0"/>
    <dubbo:service interface="com.dubbo.service.UserService" ref="userServiceImpl2" version="2.0.0"/>

    <bean id="userServiceImpl" class="com.dubbo.service.impl.UserServiceImpl"/>
    <bean id="userServiceImpl2" class="com.dubbo.service.impl.UserServiceImpl2"/>

</beans>
```
***注册监听器web.xml***
```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-dubbo.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    
</web-app>
```
## 4：创建消费者
![在这里插入图片描述](https://img-blog.csdnimg.cn/d37394642606422bb531b9fc209c2bc2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
***依赖***
```html
<dependencies>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dubbo</artifactId>
      <version>2.6.2</version>
    </dependency>

    <dependency>
      <groupId>com.dubbo</groupId>
      <artifactId>java-dubbo-provider</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>

    <dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-framework</artifactId>
      <version>4.1.0</version>
    </dependency>

  </dependencies>
```
***spring-dubbo***
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="consumer"/>

    <!--指定注册中心-->
    <dubbo:registry address="zookeeper://localhost:2181" />

    <dubbo:reference id="userServiceImpl" interface="com.dubbo.service.UserService" version="1.0.0"/>

    <dubbo:reference id="userServiceImpl2" interface="com.dubbo.service.UserService" version="2.0.0"/>

</beans>
```
***spring-mvc***
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--扫描组件-->
    <context:component-scan base-package="com.dubbo.web"/>

    <!--配置注解的驱动-->
    <mvc:annotation-driven/>

<!--    识图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    
</beans>
```
***调用业务服务方法***
```java
package com.dubbo.web;

import com.dubbo.service.UserService;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.annotation.Resource;

@Controller
public class UserController {

    @Resource
    private UserService userServiceImpl;

    @Resource
    private UserService userServiceImpl2;

    @RequestMapping(value = "/user.do")
    public String user(Model model,Integer id){
        model.addAttribute("user",userServiceImpl.queryUserById(id));
        model.addAttribute("user2",userServiceImpl2.queryUserById(id));
        return "user";
    }
}
```
***展示页面***
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>用户详情</h1>
<div>用户标识:${user.id}</div>
<div>用户名称:${user.name}</div>

<h1>用户详情2</h1>
<div>用户标识:${user2.id}</div>
<div>用户名称:${user2.name}</div>
</body>
</html>
```
> 启动步骤：
> 开启zookeeper服务然后陆续开启：interface——provider——consumer

***结果***
![在这里插入图片描述](https://img-blog.csdnimg.cn/00fa2bb62697498bb7cc36ac20405adf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

