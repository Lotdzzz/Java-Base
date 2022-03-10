@[TOC](目录)
# Spring框架

> Spring框架使用的jar都比较小，体积轻量，运行效率高，不依赖其他jar
> Sprng框架提供了解耦合方法，让对象之间的操作更灵活，**ioc控制反转**
> Spring提供了**AOP切面编程功能**
> Spring提供了**集成**功能，可以和其他的框架一起使用
> ****
> **数据访问模块（连接数据库等操作）：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/a03a260a894b426eacbefe33750c4815.png)
> **web开发模块（SpringMVC）：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/088c3367926e4fae9c572ca5e3e5feee.png)
> **AOP面向切面编程模块：**
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1ffb387e7c5a47f9af7539a86b76f696.png)
> **集成模块：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/3e8f37e766d74ebf955d66fbb2e14072.png)
> **核心容器模块（存放java对象创建java对象的）：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/7f6f295f31864724a9d5dd419ba98ce5.png)
> **测试模块**
![在这里插入图片描述](https://img-blog.csdnimg.cn/95cae937a1ac4412a7142ff84387a616.png)
## ioc功能
> ioc控制反转功能，可以把对象的创建，赋值管理工作都交给容器完成，可以让其他资源创建对象并赋值 
> 通过ioc功能可以省去Object obj= new Object();等这样的创建对象的功能
> 只需要通过ioc的容器获取现成的对象就可以
### di依赖注入
通过ioc的di技术只需要提供类的名字就可以通过di在容器中创建对象，id技术底层使用了反射机制

**ioc功能的使用：**
**创建普通javaSE项目：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/43db5fde7e094b8e92c873952e565378.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_15,color_FFFFFF,t_70,g_se,x_16)
**加入Spring依赖：**
maven中央仓库spring依赖地址：[maven中央仓库](https://mvnrepository.com/artifact/org.springframework/spring-context/5.3.12)
```html
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.12</version>
</dependency>
```
**创建带接口的类（service包）：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/9effa13f01ac4263a10e22e38c642e9e.png)
**SomeService：**
```java
package com.javase.service;

public interface SomeService {
    void doSome();
}
```
**SomeServiceImpl：**
```java
package com.javase.service.impl;

import com.javase.service.SomeService;

public class SomeServiceImpl implements SomeService {

    private String name;
    private String password;
	private School school;//引用类型
	
    @Override
    public void doSome() {
        System.out.println("doSome is begin");
    }

    @Override
    public String toString() {
        return "SomeServiceImpl{" +
                "name='" + name + '\'' +
                ", password='" + password + '\'' +
                ", school=" + school +
                '}';
    }
    
    public SomeServiceImpl() {}
    
public SomeServiceImpl(String name, String password,School school) {
        this.name = name;
        this.password = password;
        this.school = school;
    }

    public School getSchool() {return school;}
    public void setSchool(School school) {this.school = school;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public String getPassword() {return password;}
    public void setPassword(String password) {this.password = password;}
}
```
**创建Spring的配置文件：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/0370a498d14e41c68166c7d685ec603b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/956aebdcaf8b41a882705b9ebd3f86bc.png)
**创建对象方法：**
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!--
    创建对象：
    使用bean标签：id是创建后的对象名字(唯一值)，class是类的全限定名知道要创建对象的类在哪
    这样spring就完成SomeService someService = new SomeServiceImpl();
    创建完后spring会把创建好的对象放进内置的map中
    一个标签只能声明一个对象
    
    spring创建的对象还可以是java中的类（非自定义的类）比如String，Date等
    -->
    <bean id="someService" class="com.javase.service.impl.SomeServiceImpl">
    </bean>
    
    <!--多个bean标签表示创建多个对象-->
    <bean id="someServiceTwo" class="com.javase.service.impl.SomeServiceImpl">
    </bean>
  
</beans>
```
**使用对象方法：**
```java
    @Test
    public void testApp() {
        //指定spring配置文件的名称
        String config = "applicationContext.xml";
        /*
        创建spring容器对象，ApplicationContext是个接口创建它的实现类ClassPathXMLApplicationContext表示从类路径(classes)的文件中加载配置文件
        执行完这段代码之后spring的容器内就已经调用对象的无参构造方法创建完对象了
         */
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(config);
        //调用对象的getBean方法获取创建好的对象，参数根据bean标签的id值，方法返回Object需要强转
        SomeService service = (SomeService) applicationContext.getBean("someService");
        //使用方法
        service.doSome();
        //获取容器中定义的对象的名称：getBeanDefinitionNames方法返回String数组
        //getBeanDefinitionCount获取对象的个数
        int count = applicationContext.getBeanDefinitionCount();
        String[] names = applicationContext.getBeanDefinitionNames();
        for (int i = 0; i < names.length; i++) {
            System.out.println(names[i]);
        }//结果是
        //someService
        //someServiceTwo
    }
```
### 设值注入
设值注入就是给对象的属性赋值
可以通过配置文件给属性赋值
也可以通过注解给属性赋值
**Set注入：**
```html
    <!--
    set注入语法：<property 属性名="" 属性值="">
    一个property标签只能表示一个属性
    就是实现了类的setName和setPassword方法
    
    注：类里必须写set方法，
    只要通过<property>标签设置name和value后spring就会通过反射去调用对应名称的set方法，但是不关心你这个类到底有没有这条属性，而是关心有没有这个名称的set方法

	顺序：容器内通过无参方法创建对象然后set方法设置属性值
    -->
    <bean id="someService" class="com.javase.service.impl.SomeServiceImpl">
        <property name="name" value="张三"/><!--setName("张三")-->
        <property name="password" value="123456"/><!--setPassword("123456")-->
    </bean>
```
**引用类型的赋值**
创建School类并设置有无参构造，并设置set与get方法
配置文件语法：
```html
    <bean id="someService" class="com.javase.service.impl.SomeServiceImpl">
        <property name="name" value="张三"/>
        <property name="password" value="123456"/>
        <!--引用类型的赋值语法:<property name="" ref="bean的id（对象的id值）">-->
        <property name="school" ref="school"/>
    </bean>
    <!--声明school对象为someService的school属性设值-->
    <bean id="school" class="com.javase.service.impl.School">
        <property name="schoolName" value="某某科技大学"/>
    </bean>
```
**构造注入：**
容器通过调用类的有参构造方法创建对象
注：构造方式创建对象时，设置属性必须和类里的属性个数一致，没有默认赋值
```html
    <bean id="someService" class="com.javase.service.impl.SomeServiceImpl">
        <!--构造方法创建对象语法：
        第一种写法：
        <constructor-arg name=属性名 value或ref=属性值>
        第二种：
        <constructor-arg index=属性位置从左0开始 value或ref=属性值>
        第三种：
        <constructor-arg value或ref=属性值>
        -->
        <constructor-arg name="name" value="张三"/>
        <constructor-arg name="password" value="123456"/>
        <constructor-arg name="school" ref="school"/>
    </bean>
    <bean id="school" class="com.javase.service.impl.School">
        <property name="schoolName" value="某某科技大学"/>
    </bean>
```
**引用类型的自动注入byName**
```html
    <!--
    自动注入只能针对引用类型的
    autowire常用的：byName，byType
    byName：
    引用类型属性的名字一定要和bean的id一样才可以赋值
    比如someService的School属性的名字叫school而bean的id也叫school才可以给school属性赋值
    -->
    <bean id="someService" class="com.javase.service.impl.SomeServiceImpl" autowire="byName">
        <property name="name" value="张三"/>
        <property name="password" value="123456"/>
    </bean>
    <bean id="school" class="com.javase.service.impl.School">
        <property name="schoolName" value="某某科技大学"/>
    </bean>
```
**引用类型的自动注入byType**
```java
    <!--
    自动注入只能针对引用类型的
    autowire常用的：byName，byType
    byType：
    这是通过寻找类的引用类型属性的数据类型来和其他bean标签的class的数据类型是否一样
    必须是同类型的数据才可以赋值
    比如someService类的school属性的数据类型是School而标签id是school的class也是School
    这样就可以赋值

    注：数据类型一样的条件
    1：数据类型和class的类型一样的
    2：数据类型和class的类型是父子关系的
    3：数据类型和class的类型是接口和实现类关系的
    -->
    <bean id="someService" class="com.javase.service.impl.SomeServiceImpl" autowire="byType">
        <property name="name" value="张三"/>
        <property name="password" value="123456"/>
    </bean>
    <bean id="school" class="com.javase.service.impl.School">
        <property name="schoolName" value="某某科技大学"/>
    </bean>
```
### 多配置文件的方式
**多配置文件可以创建一个主配置文件来管理**
**创建SomeService配置文件：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/72da017b77cf48e28ebddf1485160a6e.png)
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="someService" class="com.javase.service.impl.SomeServiceImpl" autowire="byType">
        <property name="name" value="张三"/>
        <property name="password" value="123456"/>
    </bean>
    <bean id="school" class="com.javase.service.impl.School">
        <property name="schoolName" value="某某科技大学"/>
    </bean>
</beans>
```
**创建主配置文件：**
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--主配置文件导入其他配置文件
    导入之后，测试使用的导入配置文件直接写主配置文件即可
    注：主配置文件不能导入自己
    -->
    <import resource="classpath:spring-SomeService.xml"/>
</beans>
```
### 设值注入（注解方式）
#### 使用方式/功能介绍
**以下几种注解使用方式一致**
> @Repository（在持久层上使用的注解）：
> 放在dao层上的注解
> ****
> @Service（在业务层上使用的注解）：
> 放在Service层上的注解
> ****
> @Controller（用在控制器上的注解）：
> 比如servlet等可以使用这个注解
> ****
> @Component（除以上三种方式之外的情况的类对象用此注解）
> ****
> @Repository，@Service，@Controller三种注解是给项目分层用的（功能不一样）

**介绍通过注解方式创建对象方式：**
**在类中：**
```java
/*
注解方式在类的上面使用注解@Component()
内的value表示创建后对象的名字，创建后对象会被放进容器
value可以省略写成：@Component("someService")
 */
@Component(value = "someService")
public class SomeServiceImpl implements SomeService {
	//普通属性的赋值：@Value("")
	//需要使用@Value注解()内是属性的值
	@Value("张三")
    private String name;
    @Value("123456")
    private String password;
	@Value("张三")//注解也可以写到set方法上也可以完成属性赋值
	public void setName(){}

	/*
	引用类型赋值需要用到@Autowired注解
	@Autowired的参数默认是(required = true)
	required的true是默认必须有值，false是默认是null值
	而Autowired默认是byType的自动注入方式找引用对象的,默认找到在容器中类型为School类型的对象

	如果需要byName就加入@Qualifier注解把value设置为容器中对象的名字
	*/
	@Autowired
	@Qualifier(value = "school")//byName的方式
	private School school;//引用类型

	/*
	第二种方式赋值
	@Resource是jdk自带的不是spring的，spring只是提供了使用支持
	Resource是默认先使用byName自动注入，如果byName没找到则使用byType方式
	Resource指定只使用byName方式写法：@Resource(name = "容器中对象的名字")
	*/
	@Resource
	private School school;
	......
}

@Component("school")
public class School {
    @Value("某某科技大学")
    private String SchoolName;
    ......
}
```
**配置文件声明组件扫描器：**
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!--声明组件扫描器component-scan
	扫描的东西就是java对象上的注解@Component
	base-package就是要扫描的包名
	conponent-scan是扫描包下的类中的注解并创建对象和属性赋值操作
	-->
    <context:component-scan base-package="com.javase.service.impl"></context:component-scan>
    


    <!--使用多个包的写法：-->
    <!--第一种-->
	<context:component-scan base-package="包路径;包路径"></context:component-scan>
	<!--第二种-->
	<context:component-scan base-package="包路径"></context:component-scan>
	<context:component-scan base-package="包路径"></context:component-scan>
	<!--第三种指定父包比如com.javase.service可以写成com.javase-->
	<context:component-scan base-package="com.javase"></context:component-scan>
	
</beans>
```
## AOP面向切面编程
AOP是aspectj框架实现的技术
AOP的作用是给已经创建好的类增加方法的额外功能（在保证不改变源代码的情况下）
AOP功能底层是用jdk动态代理和cglib动态代理技术实现的

**面向切面编程：**
> 分析方法的功能，找出切面（需要增加额外功能的位置）
> 安排切面的执行时间（在方法前，还是方法后）
> 安排方法的执行位置（在哪个类，哪个方法实现功能增强）

**术语：**
> Aspect：切面，表示增强的功能，非业务功能，常见的功能：日志，事务等
> ****
> JoinPoint：连接点，连接需要增强功能的类对象方法位置
> ****
> Pointcut：切入点，指多个连接点方法的集合
> ****
> Advice：通知，表示切面功能的执行时间

**aspectj实现aop两种方式：配置xml文件，使用注解**
**aspectj使用的通知注解：**
> @Before
> ****
> @AfterReturing
> ****
> @Around
> ****
> @AfterThrowing
> ****
> @After

**Aspectj的切入点表达式（需要增强功能的方法位置表示）：**
>**execution(访问权限类型(可省略) 返回值类型 包名类名(可省略)方法名(方法参数) 抛出异常类型(可省略))**
>**比如：execution(public void com.javase.service.SomeService.doSome(String,Integer))**
>符号：
![在这里插入图片描述](https://img-blog.csdnimg.cn/fe2c3e50424a43d7838af7f544cf1dec.png)
```java
execution(public void com.javase.service.SomeService.doSome(String,Integer))
以上代码可以简写成：execution(* *..Service.SomeService.doSome(..))

方法的访问权限已经省略，
第一个*表示方法的返回值可以是任何类型的，
第二个*..表示从所有Service包的上多级目录寻找Service包的SomeService类的doSome方法,
如：
com.javase.Service
cn.crm.Service
java.Service
以上的*..是查找Service之前的所有级别的父包中查找Service包
而(..)的..是表示任何参数
```
**aop技术实现步骤：**
```java
1：加入依赖：aspectj依赖
2：创建目标类（接口和实现类）
3：创建切面类，使用aspectj的@Aspect注解表示这是一个切面类
并实现目标类的方法功能增强指定切入点表达式execution
5：创建spring配置文件，将对象交给容器管理
```
### AOP功能实现：
**加入aspectj依赖：**
```html
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.3.13</version>
</dependency>
```
**创建接口和目标实现类：**
```java
接口：
package com.javase.service;

public interface SomeService {
    void doSome(String name,Integer id);
    Student doStudent(String name,String password);
}


目标类：
package com.javase.service.impl;

import com.javase.service.SomeService;
/*
创建目标类，使用aop技术给这个实现类的doSome方法增加额外的功能
 */
public class SomeServiceImpl implements SomeService {
    @Override
    public void doSome(String name,Integer id) {
        System.out.println("doSon is running");
    }
    @Override
    public Student doStudent(String name,String password){
        return new Student(name,password);
    }
}
```
**注：**
**如果只创建普通类没有接口的话底层使用的是cglib动态代理
如果有接口和实现类底层使用的是jdk的动态代理**

**创建切面类（实现增加额外功能的切面技术）（例子全部注解）：**
#### @Before前置通知
```java
package com.javase.service;

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

/*
@Aspect是aspectj框架中的注解
作用是把这个类当做切面类来给业务的方法增加额外的功能
*/
@Aspect
public class MyAspectj {
    /*
    定义方法实现切面类增加额外功能
    定义要求：
    1：必须是公共方法public
    2：方法没有返回值
    3：方法名称自定义
    4：方法可以有无参数，参数是数据类型不用写名
     */
     
    //添加前置注解@Before表示在方法执行之前增强功能
    //value是切入点表达式execution（指定方法的位置的表达式）
    //第一种写法：
    @Before(value = "execution(public void com.javase.service.impl.SomeServiceImpl.doSome(String,Integer))")
    //JoinPoint是一个连接点参数，作用是获取方法执行的所有信息，列如方法名，方法的参数，方法的返回值等
    //JoinPoint是框架赋予的必须是第一位参数
    public void Before(JoinPoint jp){
    
        //给目标类的方法前增加方法功能
        System.out.println("代码前置功能执行");
        
        //使用JoinPoint方法获取目标方法的信息
        System.out.println("方法的定义："+jp.getSignature());//结果：方法的定义：void com.javase.service.SomeService.doSome(String,Integer)
        
        //获取方法名称
        System.out.println("方法的名称"+jp.getSignature().getName());//结果：方法的名称doSome
        
        //获取方法的参数信息（是传入的参数不是参数名字）返回的是Object数组
        Object[] args = jp.getArgs();
        for (int i = 0; i < args.length; i++) {
            System.out.println(args[i]);
        }
        /*
        结果：
        张三
        1
         */
    }
    //第二种:
    @Before(value = "execution(* doSome(..))")
    public void Before1(){
        //给目标类的方法前增加方法功能
        System.out.println("代码前置功能执行");
    }
}
```
**创建spring配置文件：**
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--把对象交给spring容器管理目标类SomeServiceImpl和切面类MyAspectj-->
    <bean id="someService" class="com.javase.service.impl.SomeServiceImpl">
    </bean>
    <bean id="MyAspectj" class="com.javase.service.MyAspectj">
    </bean>

    <!--
    声明自动代理生成器，表示使用aop面向切面功能
    使用aop后，动态代理创建的对象全都在内存中完成改变
    改变后spring容器中的目标对象都变为动态代理对象

    工作顺序：先从容器中扫描类：
    SomeServiceImpl发现没有@Aspect注解继续扫描MyAspectj类有@Aspect注解
    就把这个类里的所有切入点表达式execution中出现的类视为目标类并实现动态代理

	如果有接口和实现类之后还想用cglib动态代理就在后面加参数：poxy-target-class="true"
	<aop:aspectj-autoproxy poxy-target-class="true">
    -->
    <aop:aspectj-autoproxy/>
    
</beans>
```
#### @AfterReturning后置通知
**切面类实现@AfterReturning：**
```java
    /*
    @AfterReturning后置注解
    属性value是切入点表达式
    属性returning是目标方法的返回值，也就是@AfterReturning后置注解的方法的参数，它的参数就是目标类方法的返回值，参数名要和returning属性的名字一样
    注解作用：
    可以获取目标方法的返回值，执行原理：
    Object result = doStudent(参数，参数);
    @AfterReturning注解的后置方法AfterReturning(result)
    获取返回值后可以修改这个参数，可是这个参数只是目标方法执行后返回的数据，主方法内的返回值是基础类型不会改变，引用类型是会被改变的
     */
    @AfterReturning(value = "execution(* com.javase.service.impl.SomeServiceImpl.doStudent(String,String))",returning = "result")
    public void AfterReturning(Object result){
        //方法结束时的返回值
        System.out.println("方法的返回值:"+result);

        //把返回值从Object类型强转为返回值的类型
        Student res = (Student) result;
        //更改返回值的属性
        res.setName("李四");
    }
```
**测试：**
```java
    @Test
    public void testApp() {
        String config = "applicationContext.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(config);
        SomeService service = (SomeService) applicationContext.getBean("someService");
        System.out.println(service.doStudent("张三","123456").toString());
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6aed410b563c46f68960b8372230928a.png)
#### @Around环绕通知
**环绕通知既能改前置也能改后置和结果（一般是做事务用的）**
```java
    /*
    @Around：环绕通知
    属性value：切入点表达式
    作用：
    在目标方法前和后能都做功能增强
    也可以修改目标方法执行的结果

    参数：ProceedingJoinPoint 相当于jdk动态代理的Method调用目标方法的作用
    ProceedingJoinPoint是JoinPoint子类也可以实现获取目标方法信息的功能
     */
    @Around(value = "execution(* com.javase.service.impl.SomeServiceImpl.doStudent(..))")
    public Object Around(ProceedingJoinPoint pjp) throws Throwable {
        Object res = null;
        //设置方法前置功能增强
        System.out.println("前置功能启动");

        //实现目标方法的调用
        res = pjp.proceed();//相当于method.invoke();
        Student stu = (Student) res;
        System.out.println("目标方法执行"+stu.toString());

        //设置方法后置功能增强
        System.out.println("后置功能启动");

        //处理目标类的返回结果
        stu.setName("李四");
        return res;
    }
```
测试：
```java
    @Test
    public void testApp() {
        String config = "applicationContext.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(config);
        SomeService service = (SomeService) applicationContext.getBean("someService");
        System.out.println(service.doStudent("张三","123465"));
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd9aa67c72d044b3b24a30da3c047f91.png)
#### @AfterThrowing异常通知
```java
    /*
    @AfterThrowing：异常通知
    属性value：切入点表达式
    属性throwing：目标方法抛出的异常对象，要和方法的参数名一致
    方法参数：Exception，也可以加入JoinPoint
    作用：当目标方法出现异常的时候这个异常通知会执行，报告错误等信息
     */
    @AfterThrowing(value = "execution(* com.javase.service.impl.SomeServiceImpl.doSome(..))",throwing = "ex")
    public void AfterThrowing(Exception ex){
        //报告异常信息
        System.out.println("方法异常："+ex.getMessage());
    }
```
#### @After最终通知
```java
    /*
    @After：最终通知
    属性value：切入点表达式
    作用：
    在目标方法执行之后总是会执行
     */
    @After(value = "execution(* com.javase.service.impl.SomeServiceImpl.doSome(..))")
    public void After(){
        //一般都是做清除等操作
        System.out.println("内存已释放");
    }
```
#### @Pointcut辅助注解
定义和管理切入点的
作用是将切面类中可复用的代码片段实现复用可以重复利用的功能
实现：
```java
    /*
    比如这个切入点表达式的方法名称：* com.javase.service.impl.SomeServiceImpl.doSome(..)
    每次的通知都需要写那可以用@Pointcut把这段方法名称重复利用
     */
    @Before(value = "execution(* com.javase.service.impl.SomeServiceImpl.doSome(..))")
    public void Before(){
        System.out.println("代码前置功能执行");
    }

    /*
    @Pointcut：辅助注解，无需实现代码
    属性value：将需要重复利用的方法名称写进value属性中
     */
    @Pointcut(value = "execution(* com.javase.service.impl.SomeServiceImpl.doSome(..))")
    public void cut(){
        //无需写代码
    }
    /*
    使用cut
     */
    @Before(value = "cut()")
    public void Before(){
        System.out.println("代码前置功能执行");
    }
```
## Spring框架集成Mybatis
Spring框架使用ioc技术去集成Mybatis
ioc主要是把Mybatis的类都使用ioc去创建对象

**步骤：**
加入依赖：spring，Mybatis，mysql，spring的事务依赖，Mybatis和spring集成的依赖
创建实体类，dao接口和sql映射文件mapper
创建Mybatis主配置文件
创建spring配置文件
将Mybatis的类在spring配置文件声明：
mysql数据库连接信息（数据源）
SqlSessionFactory
Dao对象
service
**加入依赖：**
```html
  <dependencies>

    <dependency><!--spring事务依赖-->
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.3.12</version>
    </dependency>

    <dependency><!--spring事务依赖-->
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.3.13</version>
    </dependency>

    <dependency><!--mybatis依赖-->
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.7</version>
    </dependency>

 	<dependency><!--spring集成mybatis的依赖-->
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.6</version>
    </dependency>

    <dependency><!--mysql依赖-->
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.27</version>
    </dependency>

    <dependency><!--spring的aop依赖-->
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>5.3.13</version>
    </dependency>

    <dependency><!--spring依赖-->
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.12</version>
    </dependency>

    <!--阿里公司的德鲁伊数据库连接池依赖-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>

    <dependency><!--单元测试依赖-->
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

  </dependencies>
```
**创建Mybatis框架：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/3c9ee7c571254768ae197447d8a66faa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_15,color_FFFFFF,t_70,g_se,x_16)
**Mybatis主配置文件的连接池在spring中使用方式：**
需要阿里的德鲁伊连接池（使用方式）：[github官网：druid](https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE)
**创建Spring配置文件将类加载到容器中**
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
    使用bean标签的方式实现连接数据库信息
    id:自定义的，表示这个标签的名字
    class:使用德鲁伊的DruidDataSource
    init-method：表示开始时使用的方法init
    destroy-method:表示关闭时使用的方法close
    -->
    它的作用是提供连接信息
    它的作用是提供连接信息
    它的作用是提供连接信息
    <!--读取jdbc配置文件-->
    <context:property-placeholder location="classpath:Jdbc.properties"/>
    
    <bean id="DataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!--
        url,username,password都是DruidDataSource类中的属性value就是连接数据库的信息
        -->
        <property name="driverClassName" value="${driver}"/>
        <property name="url" value="${url}" />
        <property name="username" value="${name}" />
        <property name="password" value="${password}" />
    </bean>
	它的作用是连接数据库，读取Mybatis文件里面一些配置信息
	它的作用是连接数据库，读取Mybatis文件里面一些配置信息
	它的作用是连接数据库，读取Mybatis文件里面一些配置信息
    <!--创建SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--表示把数据库的连接池标签dataSource属性赋给了SqlSessionFactoryBean
        它就知道你的数据库是什么了
        -->
        <property name="dataSource" ref="DataSource"/>
        <!--configLocation是Resource类型，他是读取配置文件的value是配置文件路径
        -->
        <property name="configLocation" value="classpath:Mybatis.xml"/>
    </bean>

    <!--
    创建dao对象使用类：MapperScannerConfigurer：它在内部调用了getMapper()生成dao接口的代理对象
    需要提供SqlSession的getMapper(接口.class)
    -->
    它的作用是通过SqlSessionFactory获取SqlSession并调用方法getMapper把包中的所有接口都动态代理提供可以使用的操作数据库的方法，然后把所有的动态代理的对象放入容器中
    它的作用是通过SqlSessionFactory获取SqlSession并调用方法getMapper把包中的所有接口都动态代理提供可以使用的操作数据库的方法，然后把所有的动态代理的对象放入容器中
    它的作用是通过SqlSessionFactory获取SqlSession并调用方法getMapper把包中的所有接口都动态代理提供可以使用的操作数据库的方法，然后把所有的动态代理的对象放入容器中
    比如Sqlsession s = SqlSessionFactory.openSession
    Student student = (Student)s.getMapper(Student.class)
    spring就把student放入到容器中
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--需要提供SqlSessionFactory对象这个对象就是容器中创建的SqlSessionFactory，然后它内部使用SqlSessionFactory创建SqlSession对象-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--指定包名，MapperScannerConfigurer会扫描这个包中的每个接口
        扫描到的接口都执行getMapper()得到dao对象
        -->
        <property name="basePackage" value="com.javase.dao"/>
    </bean>
    <!--声明service层的对象-->
     <bean id="ServiceStudent" class="com.javase.service.impl.ServiceStudentImpl">
        <property name="studentDao" ref="studentDao"/>
    </bean>
</beans>
```
测试：
```java
    @Test
    public void TestApp() {
        //读取spring配置文件获取容器中的对象
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        ServiceStudentImpl dao = (ServiceStudentImpl) ctx.getBean("ServiceStudent");
        //获取的是service业务层的对象
        List<Student> stuList = dao.SelectStudentsService();
        for (Student student:stuList) {
            System.out.println(student.toString());
        }
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/9db7f732f006451d89e56132d2037340.png)
### 集成Mybatis异常错误处理
如果出现以下异常多半是mapper文件未找到或者driver的属性写错了
![在这里插入图片描述](https://img-blog.csdnimg.cn/2150287b8bc84bcdb580bd29c09b6c88.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
driver的属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/fadfa88b9cb242dfbd3c7269ef9886ec.png)
**只需要将项目中的mapper文件复制到类目录下就可以：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c9133dad327840fdb173dc7183219c44.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_18,color_FFFFFF,t_70,g_se,x_16)
## spring框架控制事务
### 事务的介绍
使用事务的方式：jdbc事务，Mybatis事务，hibernate事务
通过spring提供的技术来使用这些事务
> 管理事务的是事务管理和实现类
> 指定要使用的事务管理器实现类\<bean>
> 指定哪些类和方法需要加入事务功能
> 指定方法需要的隔离级别，传播行为，超时

![在这里插入图片描述](https://img-blog.csdnimg.cn/85587bc6670b4e6fa039cb321bc60481.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
spring处理事务需要提供给spring使用事务的信息
spring的PlatformTransactionManger接口内部实现事务提交（commit）事务回滚（rollback）
实现类：
Mybatis的spring创建的事务类：DataSourceTransactionManger
hibernate的spring创建的事务类：HibernateTransactionManger
使用事务类只需要在spring配置文件声明bean就可以了

事务控制的接口（事务隔离级别，事务传播，事务默认超时等）：TransactionDefinition

**spring处理事务的隔离级别**
![在这里插入图片描述](https://img-blog.csdnimg.cn/3e342544d415469eac1324e4f3ad6b47.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

> DEFAULT：默认的事务隔离级别，
> mysql默认（3级）：ISOLATION_REPEATABLE_READ
> oracle默认（2级）：ISOLATION_READ_COMMITTED

> 超时时间（默认-1：无限时间）：TIMEOUT_DEFAULT
> 
**事务传播行为**
表示方法在调用时，表示方法之间有没有事务或怎么使用事务
传播行为：
> PROPAGATION_REQUIRED
> 表示方法必须有事务，如果没有事务则会新创建事务
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/aee9124f5d244897bed72a4fd2078313.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ****
> PROPAGATION_REQUIRES_NEW
> 方法支持事务，如果方法没有事务也不影响
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/3043e6e52b4c41c881d350e7fe5d4e3e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ****
> PROPAGATION_SUPPORTS
> 在执行方法时每次都新建事务，如果方法有事务则会挂起，直到新事务执行完毕
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0eeef140ae1d4f2da661a6947768a477.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

事务提交和事务回滚的时机：
当方法执行成功，没有异常则会在执行完毕后提交事务
当方法抛出运行时异常时，会执行回滚事务
当方法抛出受查异常，受检异常（编译时异常）时，则会提交事务
### 事务的使用
**模拟商品购物：**
创建mysql数据库的表：
商品表goods：
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b1f24678d4c49b692daa55cbfe73016.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a286be69cbf34e5ba38783546f537fcb.png)
**购买记录表sale：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/161f25fe43ec4b1dbe61bf07573323da.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**创建项目（spring和mybat整合基础）：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/35cc5f0a529446a88417b9aef31cb007.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_15,color_FFFFFF,t_70,g_se,x_16)
**创建实体类（文章字数太多用省略号表示get和set方法tostring方法有无参构造等）：**
sale：
```java
public class sale {
    private Integer id;
    private Integer gid;
    private Integer nums;
    ......
}
```
goods:
```java
package com.javase.entity;

import java.math.BigDecimal;

public class goods {
    private Integer id;
    private String name;
    private Integer amount;
    private BigDecimal price;
    ......
}
```
**创建DAO**
![在这里插入图片描述](https://img-blog.csdnimg.cn/0548c545253a4e29bf664f49873dd7d6.png)
```java
package com.javase.dao;

import com.javase.entity.Sale;

public interface SaleDao {
    //作用是添加购买记录
    int InsertSale(Sale sale);
}
```
```java
package com.javase.dao;

import com.javase.entity.Goods;

public interface GoodsDao {
    //作用是更新数据库的商品信息
    int UpdateGoods(Goods goods);
    //作用是查询这个商品的信息是否符合条件
    Goods SelectGoods(Goods goods);
}
```
```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.javase.dao.GoodsDao">
    <update id="UpdateGoods">
        <!--更新数据库，如果事务成功，则把该商品的数量减少购买次数-->
        update goods set amount = amount - #{amount} where id = #{id}
    </update>
    <select id="SelectGoods" resultType="com.javase.entity.Goods">
        select * from goods where id = #{id}
    </select>
</mapper>
```
```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.javase.dao.SaleDao">
    <insert id="InsertSale">
        insert into sale(gid,nums) values(#{gid},#{nums})
    </insert>
</mapper>
```
**创建Service业务层**
![在这里插入图片描述](https://img-blog.csdnimg.cn/6df6975dcf724790a2be550c4edd013a.png)
```java
package com.javase.service;

public interface BuyService {
    //实现购买业务，参数id是要购买的商品id，nums是需要购买的个数
    void BuyGoods(Integer id,Integer nums);
}
```
```java
package com.javase.service.impl;

import com.javase.dao.GoodsDao;
import com.javase.dao.SaleDao;
import com.javase.entity.Goods;
import com.javase.entity.Sale;
import com.javase.service.BuyService;
import org.springframework.transaction.annotation.Isolation;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

public class BuyServiceImpl implements BuyService {
    //创建dao对象并实现set方法为了使用ioc技术，spring集合Mybatis的技术创建dao对象
    private GoodsDao goodsDao;
    private SaleDao saleDao;

    public void setGoodsDao(GoodsDao goodsDao) {
        this.goodsDao = goodsDao;
    }

    public void setSaleDao(SaleDao saleDao) {
        this.saleDao = saleDao;
    }

    /*
    加入事务注解：
     */
    @Transactional(
            propagation = Propagation.REQUIRED,//传播行为：REQUIRED：方法必须有事务，没有则会新建事务
            isolation = Isolation.DEFAULT,//事务的隔离级别：默认级别，会根据数据库选择mysql默认3级
            readOnly = false,//true表示对数据库的操作只读，false代表可操作读写
            rollbackFor = {//表示发生这个异常时，一定使用事务回滚
                    RuntimeException.class
            }
    )
    @Override
    public void BuyGoods(Integer id,Integer nums) {
        //加入购买记录，根据参数商品id和购买次数nums：购买了id商品nums次
        Sale sale = new Sale();
        sale.setGid(id);
        sale.setNums(nums);
        saleDao.InsertSale(sale);
        //查询这个商品是否合格能够购买，判断商品不能为空，判断商品数量不能小于购买数量，如果不合格则抛出运行时异常
        Goods goods = new Goods(id,null,null,null);
        Goods goods1 = goodsDao.SelectGoods(goods);
        if(goods1 == null){
            throw new RuntimeException();
        }else if (goods1.getAmount() < nums){
            throw new RuntimeException();
        }
        //如果是合格商品没有抛出异常，则执行数据库的更新goods商品表
        goodsDao.UpdateGoods(new Goods(id,null,nums,null));
    }
}
```
**spring配置文件**
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <context:property-placeholder location="classpath:Jdbc.properties"/>
    <bean id="DataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${driver}"/>
        <property name="url" value="${url}" />
        <property name="username" value="${name}" />
        <property name="password" value="${password}" />
    </bean>

    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="DataSource"/>
        <property name="configLocation" value="classpath:mybatis.xml"/>
    </bean>

    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <property name="basePackage" value="com.javase.dao"/>
    </bean>

    <!--创建Service的实现类交给spring容器管理，并从容器中获取动态代理的dao对象传入Service参数-->
    <bean id="buyService" class="com.javase.service.impl.BuyServiceImpl">
        <property name="saleDao" ref="saleDao"/>
        <property name="goodsDao" ref="goodsDao"/>
    </bean>
    <!--
    使用spring的事务
    声明事务管理器
    -->
    <bean id="transactionManger" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--指定连接数据库的数据源-->
        <property name="dataSource" ref="DataSource"/>
    </bean>
    <!--开启事务注解驱动，使用注解来管理事务，创建代理对象-->
    <tx:annotation-driven transaction-manager="transactionManger"/>
</beans>
```
**测试：**
```java
    @Test
    public void AppTest(){
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BuyService dao = (BuyService) ctx.getBean("buyService");
        dao.BuyGoods(1002,10);
    }
```
**添加成功后结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/0f0645c33b3344c188892ef339f8ae0c.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/93f5036f209548268485375114d15495.png)
**添加失败后结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/676fbdfde2f642e3a85ae17303707221.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/7967bc76f13b4ef2834ef9f5115ce2f7.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d6321ea8d8934253b4a5d172953d2c0e.png)
### 事务的另一种使用方式（aop）
**使用Aspect框架实现业务方法和事务配置完全分离的操作**
修改spring配置文件：
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <context:property-placeholder location="classpath:Jdbc.properties"/>
    <bean id="DataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${driver}"/>
        <property name="url" value="${url}" />
        <property name="username" value="${name}" />
        <property name="password" value="${password}" />
    </bean>

    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="DataSource"/>
        <property name="configLocation" value="classpath:mybatis.xml"/>
    </bean>

    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <property name="basePackage" value="com.javase.dao"/>
    </bean>

    <bean id="buyService" class="com.javase.service.impl.BuyServiceImpl">
        <property name="saleDao" ref="saleDao"/>
        <property name="goodsDao" ref="goodsDao"/>
    </bean>

    <bean id="transactionManger" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="DataSource"/>
    </bean>
    <!--
    声明业务方法它的事务属性（隔离级别，传播行为，超时时间等）
    标签属性id：自定义的名字，表示advice标签的
    transaction-manager是事务管理器对象的id
    -->
    <tx:advice id="advice" transaction-manager="transactionManger">
        <!--attributes是配置事务属性-->
        <tx:attributes>
            <!--method给具体的方法设置属性
            name是方法名称不带包和类，可以使用通配符*,比如addSale(),addGoods()就是用add*就可以表示这俩
            propagation是传播行为
            isolation隔离级别
            rollback-for异常类名，全限定类名
            -->
            <tx:method name="buy" propagation="REQUIRED" isolation="DEFAULT" rollback-for="java.lang.RuntimeException"/>
        </tx:attributes>
    </tx:advice>

    <!--配置aop：
    表示哪个包哪些类需要事务
    需要标签<aop:config>
    -->
    <aop:config>
        <!--
        id：切入点表达式，唯一值
        expression：切入点表达式，表示哪些类需要使用事务，Aspect会创建代理对象
        execution(* *..service..*.*(..))表示所有service包中的所有类所有方法都需要使用事务
        -->
        <aop:pointcut id="serviceAop" expression="execution(* *..service..*.*(..))"/>

        <!--配置增强器
        作用：关联advice标签和config的pointcut标签的关系
        advice-ref表示advice标签的id表示哪些需要事务的方法名字
        pointcut-ref切入点表达式的id，需要事务的位置
        -->
        <aop:advisor advice-ref="advice" pointcut-ref="serviceAop"/>
    </aop:config>

</beans>
```
测试：
```java
    @Test
    public void AppTest(){
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BuyService dao = (BuyService) ctx.getBean("buyService");
        dao.BuyGoods(1002,10);
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/f4625740ac184407b95303ab7b935525.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0f849df135834bb888fb6a1e53ecd20f.png)
## spring框架web开发
创建web项目：
![在这里插入图片描述](https://img-blog.csdnimg.cn/d4bccbcb93e7441393c9de7a98e6fef6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d94db74ea364ad4bd985744b683f310.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_19,color_FFFFFF,t_70,g_se,x_16)
**其中的java和resources都是自己添加**

**加入依赖（servlet，jsp）：**

```html
<dependency><!--servlet依赖-->
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>

<dependency><!--jsp依赖-->
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2</version>
    <scope>provided</scope>
</dependency>
```
**配置spring集成Mybatis**
![在这里插入图片描述](https://img-blog.csdnimg.cn/a4d3178809df4f62b40b3ef943583d38.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/45fbac54c3264b989264ae555b7a2c21.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

**配置jsp：**
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <p>注册用户</p>
    <form action="" method="get">
        <table>
            <tr>
                <td>id</td>
                <td><input type="text" name="id"></td>
            </tr>
            <tr>
                <td>username</td>
                <td><input type="text" name="username"></td>
            </tr>
            <tr>
                <td>password</td>
                <td><input type="text" name="password"></td>
            </tr>
            <tr>
                <td>sex</td>
                <td><input type="checkbox" name="sex" value="man" checked>男
                    <input type="checkbox" name="sex" value="woman">女
                </td>
            </tr>
            <tr>
                <td>email</td>
                <td><input type="text" name="email"></td>
            </tr>
        </table>
    </form>
</body>
</html>
```
**maven生成的web.xml文件版本太低需要改：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f393fb8be6064031a8f777709ad70ff8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c1e0ef7525b04ca78bd29e1000aed8d1.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7603f25a8fc048389e8e78af826ff22f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7c996f3827da41c8913957b7847fe45d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ba29008b8085429496ccc5a407bc9b4b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**创建注册用户的servlet：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b9c466803e34fcb86d666db9f9eb646.png)
```java
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Register extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        StudentService service = (StudentService) ctx.getBean("studentService");
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String sex = req.getParameter("sex");
        String email = req.getParameter("email");
        Student student = new Student(null,username,password,sex,email);
        service.RegisterStudent(student);
        req.getRequestDispatcher("/result.jsp");
    }
}
```
**创建结果页面（result.jsp）：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/cdddcb71c7394678a6b4361f83f118d5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**配置tomcat：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/7aa82fe73bbb4bed94eb19f9e157b080.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/21450cc6156d4c1dbc584daecd8f5bc5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/64fc750a56b14712bdbfaae3b9831ced.png)
**配置监听器**
spring的容器对象只要方法调用就会创建一次，而这个容器只需要项目启动时只需要创建一次就够了
所以把容器配置到监听器全局作用域ServletContext中

作用：不用创建容器对象多次，增加程序运行速度

步骤：
加入依赖：
```html
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.3.13</version>
</dependency>
```
在web.xml中注册监听器：
```html
    <!--设置监听器需要读到的spring配置文件的路径
    contextConfigLocation表示要读取文件路径
    -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <!--
    注册监听器ContextLoaderListener
    监听器创建后需要读取spring配置文件路径
    -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
```
这样注册完之后spring底层会使用setAttribute把容器对象添加到全局作用域中
更新servlet文件：
```java
package com.javaWeb.controller;

import com.javaWeb.entity.Student;
import com.javaWeb.service.StudentService;
import org.springframework.web.context.WebApplicationContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Register extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //创建容器对象，准备把从全局作用域中的容器放到这里
        WebApplicationContext ctx = null;
        //获取容器对象的key值：WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE
        String key = WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE;
        //获取全局作用域中的容器对象
        ctx = (WebApplicationContext) getServletContext().getAttribute(key);

        StudentService service = (StudentService) ctx.getBean("studentService");
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String sex = req.getParameter("sex");
        String email = req.getParameter("email");
        Student student = new Student(null,username,password,sex,email);
        service.RegisterStudent(student);
        req.getRequestDispatcher("/result.jsp").forward(req,resp);
    }
}
```
**使用框架的工具类获取从全局作用域容器对象：**
```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        WebApplicationContext ctx = null;
        ServletContext sc = getServletContext();
        ctx = WebApplicationContextUtils.getRequiredWebApplicationContext(sc);



        StudentService service = (StudentService) ctx.getBean("studentService");
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String sex = req.getParameter("sex");
        String email = req.getParameter("email");
        Student student = new Student(null,username,password,sex,email);
        service.RegisterStudent(student);
        req.getRequestDispatcher("/result.jsp").forward(req,resp);
    }
```
# JavaConfig和xml配置文件
## JavaConfig
> JavaConfig可以代替xml文件的bean标签，是配置spring容器的纯java方法，这个java类可以创建java对象，把对象放到容器内
> 使用：
> @Configuration：在一个类上使用，表示这个类是作为配置文件使用的
> @Bean：声明对象，把对象注入到容器中

***创建配置文件***
![在这里插入图片描述](https://img-blog.csdnimg.cn/83178c2481794f6c9298d075c193bc17.png)
***创建普通类***
```java
package com.java.domain;

public class Student {
    private String name;
    private String id;
    public Student() {}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public String getId() {return id;}
    public void setId(String id) {this.id = id;}
    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", id='" + id + '\'' +
                '}';
    }
}
```
***创建JavaConfig类***
```java
package com.java.config;

import com.java.domain.Student;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class JavaConfig {
    //    创建方法，方法的返回值是个对象，在方法上加入@Bean注解
    //而这个@Bean可以有参数，参数就是name = "名字"
    //这个name参数就是Bean标签的id，指定对象的名称
    @Bean
    public Student createStudent(){
        Student student = new Student();
        student.setName("张三");
        student.setId("1");
        return student;
    }
}
```
测试：
```java
public class TestApp {
    @Test
    public void Test(){
        //如果是注解创建，使用的实现类是：AnnotationConfigApplicationContext
        //需要的参数是JavaConfig类
        //getBean直接获取类内的方法就可以
        ApplicationContext ctx = new AnnotationConfigApplicationContext(JavaConfig.class);
        Student student = (Student) ctx.getBean("createStudent");
        System.out.println(student.toString());
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2942a5f608e041e8bab4aa872cebbccb.png)
## @ImportResource

> 作用：导入其他的xml的配置文件，等同于xml文件中使用import resources="配置文件"的标签
```java
@Configuration
@ImportResource(value = "classpath:spring.xml")//通过这个注解和参数可以将spring配置文件中的bean也导入到容器中
public class JavaConfig {
    @Bean
    public Student createStudent(){
        Student student = new Student();
        student.setName("张三");
        student.setId("1");
        return student;
    }
}
```
## @PropertyResource
> 作用：读取properties配置文件，使用配置文件实现外部化配置，在程序之外提供数据
> 使用：
> 在resources目录下，创建properties配置文件
> 在PropertyResource指定配置文件位置
> 使用(value = "${key}")

![在这里插入图片描述](https://img-blog.csdnimg.cn/e4c9ed33f32f4b8993674606bf051add.png)
```java
stu.name=王五
stu.id=2
```
***在普通类设置配置文件***
```java
package com.java.domain;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("stu")
public class Student {
    @Value("${stu.name}")
    private String name;
    @Value("${stu.id}")
    private String id;
    public Student() {}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public String getId() {return id;}
    public void setId(String id) {this.id = id;}
    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", id='" + id + '\'' +
                '}';
    }
}
```
***将创建好的对象放到容器内***
```java
package com.java.config;

import com.java.domain.Student;
import org.springframework.context.annotation.*;

@Configuration
@ImportResource(value = "classpath:spring.xml")
@PropertySource(value = "classpath:config.properties")//声明配置文件
@ComponentScan(basePackages = "com.java.domain")//组件扫描器扫描使用配置文件创造的对象的类
public class JavaConfig {
    @Bean
    public Student createStudent(){
        Student student = new Student();
        student.setName("张三");
        student.setId("1");
        return student;
    }
}
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ee4480555cd4a33917b1764e09ba391.png)

