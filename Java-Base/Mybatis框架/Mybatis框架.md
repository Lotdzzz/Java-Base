@[TOC](目录)
# 三层架构

> 1：界面层——controller包（servlet） 
> 接收用户的请求参数，处理请求结果（jsp，html，servlet）
> 2：业务逻辑层——service包（service类） 
> 接收了界面层传递的数据，计算逻辑，调用数据库，获取数据
> 3：数据访问层——dao包（Dao类） 
> 就是访问数据库，执行对数据的增删查看（CRUD）等

**三层中类的交互：
用户使用界面层——>业务逻辑层——>数据访问层（持久层）——>数据库（mysql）**
> 三层对应的处理框架： 
> 界面层：——servlet——springmvc（框架） 
> 业务逻辑层：——service——spring（框架）
> 数据访问层：——dao类——mybatis（框架）
# Mybatis
## Mybatis作用
**Mybatis类似于增强版的JDBC**
> 1：sql mapper：sql映射 
> 可以把数据库中的一行数据 映射为一个java对象
> 一行数据可以看做是一个java对象，操作这个对象，就相当于操作表中的数据 
> 2：Data Access Objects（DAOs）
> 数据访问，对数据库执行增删改查（CRUD）

**提供的功能**
> 1：提供了创建Connection，Statement，ResultSet的能力
> 2：提供了执行sql语句的能力
> 3：提供了循环sql，把sql的结果转为java对象，集合的能力
> 4：提供了关闭资源（Connection，Statement，ResultSet）的能力
## Mybatis的安装
Mybatis下载地址：[Github官网下载地址](https://github.com/mybatis/mybatis-3/releases)
![在这里插入图片描述](https://img-blog.csdnimg.cn/160c5b4dcdde45e0a1a92070285aef74.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**Mybatis中文帮助官网：**[mybatis中文网](https://mybatis.net.cn/)


## Mybatis的传统使用
> 1：加入Mybatis坐标，mysql的驱动
> 2：创建sql映射的配置文件
> 在dao目录中和接口同级，文件名和接口保持一致
> sql映射文件，写sql语句，一个表对应一个sql映射文件，后缀.xml
> 3：创建mybatis的主配置文件
> 一个项目只有一个主配置文件
> 主配置文件提供了数据库的连接信息和sql映射文件的位置信息
> 4：使用SqlSession调用操作数据库的操作 
>
创建Maven的普通javaSE的项目：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6a7197f69162422eaf988d335decd726.png)
**准备dao目录，mysql表，普通的类（类的数据和表中的数据类型保持一致）**
![在这里插入图片描述](https://img-blog.csdnimg.cn/515646dea1364c24bb98083501a9534d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
```java
StudentDao文件的内容：

package com.javase.dao;

import com.javase.entity.student;
import java.util.List;

public interface StudentDao {
    List<student> SelectStudents();
}
```
创建普通的类
![在这里插入图片描述](https://img-blog.csdnimg.cn/15e419b4aed34777be1e0b650019652c.png)
**打开pom.xml加入依赖**
加入依赖：mybatis，mysql
打开mybatis中文网
![在这里插入图片描述](https://img-blog.csdnimg.cn/4e35e5dd7c484c21bd759cdc5c7429d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
```html
<dependencies>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.7</version>
    </dependency>
    
     <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.27</version>
    </dependency>
    
</dependencies>

 <build>

    <resources>
      <resource>
        <!--directory填写需要拷贝额外配置文件的目录-->
        <directory>src/main/java</directory>
        <includes>
          <!--配置文件的类型：properties和xml类型的文件-->
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>

      <resource>
        <!--directory填写需要拷贝额外配置文件的目录-->
        <directory>src/main/resources</directory>
        <includes>
          <!--配置文件的类型：properties和xml类型的文件-->
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
    </resources>
</build>
```
**创建映射sql的配置文件**

在dao目录下创建文件file：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6e35f4336efb4dcda37bb0456b2e8d0e.png)
**配置文件复制mybatis中文网的内容：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/eae4eb4de84e4f7bbc972c3b5594f2e1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
```html
<?xml version="1.0" encoding="UTF-8" ?>
<!--
指定约束文件的
mybatis-3-mapper.dtd是约束文件的名称，扩展名dtd的
约束文件的作用：
限制，检查在当前文件中出现的标签，属性必须符合mybatis的要求
语法规则必须按照mybatis规定的写法
-->
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">


<!--
mapper是当前文件的根标签
namespace是命名空间，是唯一值不能重名，可以自定义
要求使用dao接口的全限定名称
-->
<mapper namespace="com.javase.dao.StudentDao">
<!--
<select>：表示查询语句，select语句
<update>：表示更新数据库的操作
<insert>：表示插入数据
<delete>：表示删除数据
-->
  <select id="SelectStudents" resultType="com.javase.entity.student">
  <!--select表示查询操作，id是执行sql语句的唯一标识，mybatis会通过这个id查找这个语句
	resultType表示查询完成后结果转换为的结果对象类型
	-->
    select * from usertable
  </select>
</mapper>
```
**创建主配置文件mybatis**
mybatis的中文网
![在这里插入图片描述](https://img-blog.csdnimg.cn/be21c4137bcc46d6b47defd559c631e5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**在resources配置文件内创建文件：mybatis.xml**
![在这里插入图片描述](https://img-blog.csdnimg.cn/5eee31d709a647118885f2dbbadb7d08.png)
```html
<?xml version="1.0" encoding="UTF-8" ?>
<!--
根标签，约数文件
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!--环境配置-->
	
	<!--日志功能-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <environments default="development">
		<!--id是唯一值，表示环境的名字-->
        <environment id="development">

			<!--<transactionManager type="JDBC"/>
			type表示事务的处理方式
			JDBC：表示底层调用jdbc的Connection对象的commit和rollback
			MANAGED：把mybatis的事务委托给其他容器比如服务器软件，框架
			-->
            <transactionManager type="JDBC"/>
			
			<!--
			dataSource是一个连接数据库是否创建连接池和连接服务器的一些信息driver url之类的
			参数
			POOLED：表示mybatis连接数据库时需要创建一个连接池PooledDataSource类
			UPOOLED：表示不使用连接池，每次执行sql语句先创建连接执行sql，再关闭连接
			这会创建一个UnPooledDataSource来管理Connection对象
			-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/ccxg"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>

        </environment>
    </environments>

    <mappers>
    <!--从类路径开始的信息，target/classes(类路径)-->
        <mapper resource="com/javase/dao/StudentDao.xml"/>
    </mappers>

</configuration>
```
## SqlSessionFactory和SqlSession
主要的类的介绍：

> Resources类： 
> 负责读取Mybatis的主配置文件 
> ****
> SqlSessionFactoryBuilder：
> 创建SqlSessionFactory对象：
> SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
> builder.build(Resources类读取主配置文件);
> ****
> SqlSessionFactory：
> 重量级对象，整个项目中有一个就够用了
> 这个对象本身是个接口，里面有两个openSession方法：带参数/无参数
> 带布尔类型参数的表示是否自动提交sql事务
> 不带参数默认不自动提交事务
> 获取SqlSession对象需要调用
> SqlSessionFactory对象调用openSession方法返回值是SqlSession
> ****
>SqlSession：
>SqlSession是个接口，提供了操作数据库的各种功能
>要求：
>SqlSession不是线程安全的，在sql语句执行之前需要openSession获取SqlSession
>执行完sql语句之后，需要关闭它，SqlSession.close();
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed5bba9d2a174d1c8cc43cb391e9fe5e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/23d1aaa416e749b39da6e78c979d0035.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

**代码实现：**
```java
public class App {
    public static void main(String[] args) throws IOException {
        //主配置文件名
        String config = "mybatis.xml";
        //获取主配置文件
        InputStream in = Resources.getResourceAsStream(config);
        //创建builder对象准备获取SqlSessionFactory对象
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        //获取SqlSessionFactory对象准备获取SqlSession
        SqlSessionFactory factory = builder.build(in);
        //调用openSession方法（自动提交事务）获取SqlSession对象
        SqlSession SqlSession = factory.openSession(true);
        //指定要执行的sql语句标识，sql映射文件中的namespace和sql语句标签的id中间加个.
        String sqlId = "com.javase.dao.StudentDao" + "." + "selectStudent";
        //调用查询方法接收返回值
        List<student> students = SqlSession.selectList(sqlId);
        //输出结果
        students.forEach( student -> System.out.println(student.toString()));
        //关闭对象
        SqlSession.close();
    }
}
```
**SqlSession的封装**
**创建util包和工具类**
![在这里插入图片描述](https://img-blog.csdnimg.cn/edfd6b6ff0404dfba855850175e67b2f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
```java
package com.javase.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import java.io.IOException;
import java.io.InputStream;

public class MyBatisUtil {
    //创建SqlSessionFactory对象，因为项目只需要一个所以写在静态块中
    private static SqlSessionFactory factory = null;
    static {
        try {
            //获取主配置文件
            InputStream in = Resources.getResourceAsStream("mybatis.xml");
            //创建SqlSessionFactory对象
            factory = new SqlSessionFactoryBuilder().build(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //获取SqlSession方法
    public static SqlSession getSqlSession(){
        SqlSession sqlSession = null;
        if(factory!=null){
            sqlSession = factory.openSession();
        }
        return sqlSession;
    }
}
```
**创建接口实现类**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f53ae764899b494bac8cfa7ec8dbd07c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_15,color_FFFFFF,t_70,g_se,x_16)
```java
package com.javase.dao.impl;

import com.javase.dao.StudentDao;
import com.javase.entity.student;
import com.javase.util.MyBatisUtil;
import org.apache.ibatis.session.SqlSession;
import java.util.List;

public class StudentDaoImpl implements StudentDao {
    @Override
    public List<student> SelectStudents() {
        //获取SqlSession对象
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        //创建根据sql映射文件的标识
        String sqlId = "com.javase.dao.StudentDao.SelectStudents";
        //执行sql语句，获取返回结果
        List<student> studentList = sqlSession.selectList(sqlId);
        //关闭SqlSession
        sqlSession.close();
        return studentList;
    }
}
```
**测试代码结果**
创建测试类
![在这里插入图片描述](https://img-blog.csdnimg.cn/5fee07e59d0848feaabe6fd83e5f1213.png)
```java
package com.javase;


import com.javase.dao.StudentDao;
import com.javase.dao.impl.StudentDaoImpl;
import com.javase.entity.student;
import org.junit.Test;
import java.io.IOException;
import java.util.List;

public class TestMybatis {
    @Test
    public void testSelect() throws IOException {
        //创建dao实体类
        StudentDao studentDao = new StudentDaoImpl();
        //得到查询结果
        List<student> studentsList = studentDao.SelectStudents();
        //显示查询结果
        for (student stu:studentsList) {
            System.out.println(stu.toString());
        }
    }
}
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/f35cd2ba691640ad9d44f589ea1ea2c5.png)
## 动态代理getMapper
动态代理的作用：
更方便的调用dao接口的方法来操作数据库
SqlSession对象执行操作数据库的方法时，sql映射文件的namespace是和接口的全限定名称一样
如com.javase.dao.StudentDao
而sql语句标签的id是和接口的方法名一样
**通过接口的全限定名称和实体类的方法的调用就可以知道sql映射文件内的sql语句标签的类型**
![在这里插入图片描述](https://img-blog.csdnimg.cn/94b14b44c93247828e130e8cfadefeac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**动态代理可以通过全限定名和方法的名称获取到sql映射文件的标签类型，通过标签确定来调用SqlSession的哪个操作数据库的方法**

如果标签是select返回值是集合那调用的是SqlSession.selectList()
如果标签是insert返回值是Int那调用的是SqlSession.insert();

mybatis的动态代理：mybatis根据接口实体类方法的调用，获取执行sql语句的信息
mybatis动态代理是通过jdk反射机制内部创建接口的实现类，完成SqlSession调用方法，访问数据库

**使用动态代理优化项目：**
删除实体类impl
![在这里插入图片描述](https://img-blog.csdnimg.cn/1e5f9adb878444b4a039c87f68dd9a7b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_15,color_FFFFFF,t_70,g_se,x_16)
**在测试中使用Mybatis动态代理：**
```java
package com.javase;

import com.javase.dao.StudentDao;
import com.javase.entity.student;
import com.javase.util.MyBatisUtil;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import java.io.IOException;
import java.util.List;

public class TestMybatis {
    @Test
    public void testSelect() throws IOException {
        //使用动态代理机制的方法，SqlSession.getMapper(接口)
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        /*
        调用dao接口的方法，调用方法，反射机制内部已经创建实体类对象，然后反射机制通过接口的全限定名称StudenDao.class和内部创建对象的方法名
        来提供对SqlSession需要调用的操作数据库的方法
         */
        List<student> studentList = dao.SelectStudents();
        //输出结果
        for (student stu:studentList) {
            System.out.println(stu.toString());
        }
    }
}
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/71763e9aabcf4c659cbe36f5d7dd2dab.png)
## Mybatis深入理解参数
执行对sql映射文件的参数传递，比如JDBC的sql语句条件where = ?
需要进行传参，而在Mybatis中是where = #{字符串}

**parameterType（不写也可以）：**
根据传入参数的数据类型来表示，比如传入的编号参数是Integer那就可以写成Mybatis规定表示的int
```html
    <select id="SelectStudentById" parameterType="int" resultType="com.javase.entity.student">
        select * from usertable where id = #{id}
    </select>
```
**Mybatis对数据类型全限定名称对比表：左边是Mybatis规定名称，右边是数据类型名称**
![在这里插入图片描述](https://img-blog.csdnimg.cn/8f604c92e78c4a718389942f88a8ee15.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
### 一个简单类型的传参
简单类型：基本数据类型（包括包装类）和String字符串
在sql映射文件中简单类型的参数是用#{字符串}来表示
```html
    <select id="SelectStudentById" resultType="com.javase.entity.student">
        select * from usertable where id = #{id}
    </select>
```
测试：
```java
    @Test
    public void testSelectStudentById(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        student stu = dao.SelectStudentById(69);
        System.out.println(stu.toString());
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/622215b8cbbc4e99acfa91d2fedcca54.png)
原理：
```java
在使用#{}之后，Mybatis执行JBDC中的PreparedStatement对象
String sql = "select * from usertable where id = ?"
PreparStatement ps = con.preparedStatement(sql);
ps.setInt(0,69);
执行完毕后封装为resultType="com.javase.entity.student"
ResultSet rs = ps.executeQuery();
while(rs.next()){
	从数据库取数据存到student对象中并返回
}
```
### 多个参数传值@Param
多个参数是用@Param命名参数
在接口中方法的参数是用@Param
```java
student SelectStudentByIdAndSex(@Param("ById") Integer id, @Param("BySex") String sex);
```
在sql映射文件中使用
```html
    <select id="SelectStudentByIdAndSex" resultType="com.javase.entity.student">
        select * from usertable where id = #{ById} and sex = #{BySex}
    </select>
```
测试：
```java
    @Test
    public void testSelectStudentByIdAndSex(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        student stu = dao.SelectStudentByIdAndSex(69,"男");
        System.out.println(stu.toString());
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/59384f4fdb7c4d5785b0dc8d6131816d.png)
### 使用对象传参
传多个参数，使用java对象的属性值作为参数
创建接口方法：
```java
student SelectStudentsByObject(student stu);
```
sql映射文件：
```html
语法：#{属性名，javaType=类型名，jdbcType=数据类型}
javaType是java中的对象属性数据类型
jdbcType是数据库中表结构字段的属性类型
    <select id="SelectStudentsByObject" resultType="com.javase.entity.student">
        select * from usertable where id = #{id,javaType=java.lang.Integer,jdbcType=BIGINT} and
                                      username = #{username,javaType=java.lang.String,jdbcType=VARCHAR}
    </select>
简化版本写法：
    <select id="SelectStudentsByObject" resultType="com.javase.entity.student">
        select * from usertable where id = #{id} and username = #{username}
    </select>
```
测试：
```java
    @Test
    public void testSelectStudentByObject(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        student stu = dao.SelectStudentsByObject(new student(69,"艾伦",null,null,null));
        System.out.println(stu.toString());
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/7d985dcd99154717abe8b5379d8006e9.png)
mybatis支持的数据库中的类型
![在这里插入图片描述](https://img-blog.csdnimg.cn/c5a9a4b77ad54b66b02a45a939da5985.png)
#### 按位置传参
接口：
```java
student SelectStudentByPosition(Integer id,String name);
```
sql映射文件：
```html
    <select id="SelectStudentByPosition" resultType="com.javase.entity.student">
        select * from usertable where id = #{arg0} and username = #{arg1}
    </select>
```
测试：
```java
    @Test
    public void testSelectStudentByPosition(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        student stu = dao.SelectStudentByPosition(69,"艾伦");
        System.out.println(stu.toString());
    }
```
#### Map传参
使用map的key进行传参
接口：
```java
student SelectStudentByMap(Map<String,Object> map);
```
创建map测试：
```java
    @Test
    public void testSelectStudentByMap(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        Map<String,Object> map = new HashMap<>();
        map.put("mapId",69);
        map.put("mapName","艾伦");
        student stu = dao.SelectStudentByMap(map);
        System.out.println(stu.toString());
    }
```
sql映射文件：
```html
    <select id="SelectStudentByMap" resultType="com.javase.entity.student">
        select * from usertable where id = #{mapId} and username = #{mapName}
    </select>
```
### #{}和${}
#{}和${}区别
```java
#{}底层使用了PrepareStatement对象执行sql语句#{}代表sql语句的?这样更安全，更迅速
${}底层使用了Statement对象执行sql语句使用了字符串的连接和替换，有sql注入现象，效率低，不安全
```
## 封装mybatis的输出结果
### resultType
resultType结果类型，执行完sql语句后将数据转为的结果类型
resultType转为的结果类型是任意的，不需要实体类
```html
    <select id="SelectStudents" resultType="com.javase.entity.student">
        select * from usertable
    </select>
    
    sql语句执行完毕后会把结果转为java对象（com.javase.entity.student）
    查询结果为id username password sex email
    相当于new student(id,username,password,sex,email);
    如果有多个对象则生成List集合储存
```
### 返回简单类型
接口：
```java
int SelectStudentsCount();
```
sql映射文件：
**建议resultType写类型的全限定名称**
```html
    <select id="SelectStudentsCount" resultType="java.lang.Integer">
        select count(username) from usertable
    </select>
```
测试：
```java
    @Test
    public void testSelectsCount(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        int result = dao.SelectStudentsCount();
        System.out.println("总人数为：" + result);
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b30e54e2f17445f1b654759f219b4fbe.png)
### 定义别名
可以在mybatis主配置文件中定义别名
```java
将全限定名com.javase.entity.student定义为stu
第一种定义方式：
    <typeAliases>
        <typeAlias type="com.javase.entity.student" alias="stu"></typeAlias>
    </typeAliases>
第二种定义方式：
将包内的所有类不需要写全限定名称，比如entity包下的student只需要在sql映射文件中写stuent就可以
    <typeAliases>
        <package name="com.javase.entity"/>
    </typeAliases>

"注：两个包下的类名有可能重复，谨慎使用，建议用全限定名"
```
sql映射文件：
```java
    <select id="SelectStudents" resultType="student">
        select * from usertable
    </select>
```
测试：
```java
    @Test
    public void testSelect() throws IOException {
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        List<student> studentList = dao.SelectStudents();
        for (student stu:studentList) {
            System.out.println(stu.toString());
        }
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b2d19e18b96f46a79faf2f192a6c30d4.png)
### 查询结果返回Map
接口：
```java
Map<Object,Object> SelectStudentByResultTypeMap(Integer id);
```
sql映射文件：
```html
    <select id="SelectStudentByResultTypeMap" resultType="java.util.HashMap">
        select * from usertable where id = #{id}
    </select>
```
测试：
```java
    @Test
    public void testSelectByResultTypeMap(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        Map<Object,Object> studentMap = dao.SelectStudentByResultTypeMap(69);
        System.out.println(studentMap);
    }
```
## resultMap结果映射
resultMap结果映射是用来指定你的列名和java对象的属性对应关系
**当列名和属性名不一样时需要使用resultMap**
**resultType和resultMap不能一起用**
接口：
```java
List<student> SelectStudentsByResultMap();
```
sql映射文件：
```html
    <!--定义resultMap
    id：自定义名称，用来标识这个resultMap标签
    type是类的全限定名
    -->
    <resultMap id="studentMap" type="com.javase.entity.student">
        <!--
        主键列使用id标签
        非主键用result标签
        column是列名
        property是java类型的属性名
        -->
        <id column="id" property="id" /><!--意思是sql语句中的id对应转为结果类型对象的id-->
        <result column="username" property="username"/>
        <result column="password" property="password"/>
        <result column="sex" property="sex"/>
        <result column="email" property="email"/>
    </resultMap>
    <select id="SelectStudentsByResultMap" resultMap="studentMap">
        select id,username,password,sex,email from usertable
    </select>
```
测试：
```java
    @Test
    public void testSelectByResultMap() {
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        List<student> studentList = dao.SelectStudentsByResultMap();
        for (student stu:studentList) {
            System.out.println(stu.toString());
        }
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/79f957959d494ca5ae930d5d4a90d2e6.png)
也可以自己指定列名赋值
```html
    <!--定义resultMap
    id：自定义名称，用来标识这个resultMap标签
    type是类的全限定名
    -->
    <resultMap id="studentMap" type="com.javase.entity.student">
        <!--
        主键列使用id标签
        非主键用result标签
        column是列名
        property是java类型的属性名
        -->
        <id column="id" property="id" /><!--意思是sql语句中的id对应转为结果类型对象的id-->
        <result column="username" property="username"/>
        <result column="password" property="password"/>
        <result column="sex" property="sex"/>
        <result column="email" property="username"/><!--将sql数据库中的email值赋给java类型对象的username属性-->
    </resultMap>
    <select id="SelectStudentsByResultMap" resultMap="studentMap">
        select id,username,password,sex,email from usertable
    </select>
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/24e914c03a664afd82536d94a0f4f89d.png)
**resultType解决列名和属性名不一致的第二种解决方案**
设一个普通类
```java
package com.javase.entity;

public class NewStudent {
    private Integer id;
    private String name;
    private String psw;
    private String sex;
    private String email;

    public NewStudent() {
    }

    public NewStudent(Integer id, String name, String psw, String sex, String email) {
        this.id = id;
        this.name = name;
        this.psw = psw;
        this.sex = sex;
        this.email = email;
    }

    @Override
    public String toString() {
        return "NewStudent{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", psw='" + psw + '\'' +
                ", sex='" + sex + '\'' +
                ", email='" + email + '\'' +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPsw() {
        return psw;
    }

    public void setPsw(String psw) {
        this.psw = psw;
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
sql映射文件：
sql语句内的列名与转为的结果类型的属性名不一样时可以使用sql语句的别名as
```html
    <select id="SelectStudents" resultType="com.javase.entity.NewStudent">
        select id,username as name,password as psw,sex,email from usertable
    </select>
```
## like模糊查询参数
模糊查询语法：
接口：
```java
List<student> SelectStudentsByLike(String name);
```
sql映射文件：
```html
第一种写法：
    <select id="SelectStudentsByLike" resultType="com.javase.entity.student">
        select * from usertable where username like #{name}
    </select>
第二种写法：
    <select id="SelectStudentsByLike" resultType="com.javase.entity.student">
        select * from usertable where username like "%" #{name} "%"
    </select>
```
测试：
```java
    @Test
    public void testSelectByLike() {
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        
        第一种sql映射文件写法的传参：
        List<student> studentList = dao.SelectStudentsByLike("%张%");
        
        第二种传参
        List<student> studentList = dao.SelectStudentsByLike("张");
        
        for (student stu:studentList) {
            System.out.println(stu.toString());
        }
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/cab4bfd85782410cac4373ed2ec063d1.png)
## 动态sql
### if标签
使用动态sql时需要使用java对象作为参数
接口：
```java
List<student> SelectSql(student stu);
```
sql映射文件：
```html
    <select id="SelectSql" resultType="com.javase.entity.student">
        select * from usertable
        where
        <!--
        动态sql
        if标签的test作为判断属性写入判断条件
        如果表达式成立就把if标签内的部分sql语句加入到主sql中
        假如第一条成立就是select * from usertable where username = ?
        -->
        <if test="username != null and username != ''">
            username = #{username}
        </if>
        <!--如果以下这条标签表达式成立则sql语句会变成
			select * from usertable where username = ? or id < ?
		-->
        <if test="id > 0">
            or id &lt; #{id}
        </if>
    </select>
如果第一个if不成立而第二个if成立的话sql会变成select * from usertable where or id < ?
这样会语法错误，改进代码where 1=1：
    <select id="SelectSql" resultType="com.javase.entity.student">
        select * from usertable
        where 1=1
        <if test="username != null and username != ''">
            username = #{username}
        </if>
        <if test="id > 0">
            or id &lt; #{id}
        </if>
    </select>
```
```java
> 以上代码中or id &lt; #{}中的&lt;表示小于的意思，因为直接写<会造成标签冲突
```
mybatis的对应符号：
| 原符号 | 对应符号 |
|--|--|
| < | &lt\; |
|>|&gt\;|
|<>|&lt\;&gt\;|
|&|&amp\;|
|'|&apos\;|
|"|&quot\;|

测试：
```java
    @Test
    public void testSelectSql(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        student stu = new student();
        stu.setId(69);
        stu.setUsername("艾伦");
        List<student> studentList = dao.SelectSql(stu);
        for (student stuList:studentList) {
            System.out.println(stuList.toString());
        }
    }
```
### where
接口：
```java
List<student> SelectSqlWhere(student stu);
```
sql映射文件：
```html
语法格式：<where> <if></if> </where>


    <select id="SelectSqlWhere" resultType="com.javase.entity.student">
        select * from usertable
        <where>
            <if test="username != null and username != ''">
                username = #{username}
            </if>
            <if test="id > 0">
                or id &lt; #{id}
            </if>
        </where>
    </select>

如果条件成立则会组装成select * from usertable WHERE username = ? or id < ?
注：如果第一个if不成立那where标签会自动删除第二个if的or[自动删除无效条件]
如果所有的if都不满足则where也没有
```
### foreach
foreach标签主要是用在sql语句中in的语法
```java
比如select * from usertable where id in (69,68,67)

foreach的底层原理：
目标语句：select * from usertable where id in(69,68,67);
		//使用List集合存储in里面的参数
		List<Integer> list = new ArrayList();
		list.add(69);
		list.add(68);
		list.add(67);
		//创建基本语句用来字符串拼接的方式完成
		String sql = "select * from usertable where id in";
		StringBuilder builder = new StringBuilder();
        //添加开始时的小括号
        builder.append("(");
        for(Integer i:list){
            //这个i就是集合中的值69 68 67每个数字后面需要加，就是69,68,67,
            builder.append(i).append(",");
        }
        //删除最后一个逗号
        builder.deleteCharAt(builder.length()-1);
        //添加结束的小括号
        builder.append(")");
        //执行字符串拼接
        sql += builder;
        System.out.println(sql);
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/1c9917f38da6458ab9acd6a9f2380ebc.png)
**foreach的语法：**
接口：
```java
//参数基础类型的写法：
List<student> SelectForeach(List<Integer> list);

//参数对象类型的写法：
List<student> SelectForeach(List<student> list);
```
sql映射文件：
```html
list集合中是基础类型的写法：
    <select id="SelectForeach" resultType="com.javase.entity.student">
        select * from usertable where id in
        <foreach collection="list" open="(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </select>
list集合是对象类型的写法：
    <select id="SelectForeach" resultType="com.javase.entity.student">
        select * from usertable where username in
        <foreach collection="list" open="(" close=")" item="stu" separator=",">
            #{stu.username}
        </foreach>
    </select>
conllection：表示接口中的方法的参数的类型,如果是数组使用array,如果是集合使用list（小写）
item:自定义，表示数组和集合的成员的变量
open:循环开始的符号 比如原理中的拼接字符串开始字符是(
close:循环结束的符号 结束字符是)
这两个是(69,68,67)中的小括号()
separator:表示循环每一个数据中分割的符号 在(69,68,67)中的逗号,
```
测试：
```java
    @Test
    public void testForeach(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        
        //基础类型的写法：
        List<Integer> list = new ArrayList<>();
        list.add(69);
        list.add(68);
        list.add(67);

		//对象类型的写法：
        List<student> stuList = new ArrayList<>();
        stuList.add(new student(null,"爱丽丝",null,null,null));
        stuList.add(new student(null,"艾伦",null,null,null));
        stuList.add(new student(null,"张三",null,null,null));
        
        List<student> studentList = dao.SelectForeach(list);//对象类型是stuList
        for (student stu:studentList) {
            System.out.println(stu.toString());
        }
    }
```
基础类型的结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/48bf641b6e2e475ca516d569c00f4aea.png)
对象类型的结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/afa087e8f1284bcc971cd88c10657f57.png)
### sql代码片段
代码片段是用来复用语句的
语法：
```html
先使用<sql id="唯一自定义名称">sql语句</sql>
在使用<include refid="定义复用语句id的值">
```
sql映射文件：

```html
<!--定义代码片段-->
    <sql id="Sql">select * from usertable</sql>
    <!--使用-->
    <select id="SelectSqlWhere" resultType="com.javase.entity.student">
    	<!--将原来的select * from usertable 替换成<include refid="Sql">定义的可复用的代码-->
        <include refid="Sql"></include>
        <where>
            <if test="username != null and username != ''">
                username = #{username}
            </if>
            <if test="id > 0">
                or id &lt; #{id}
            </if>
        </where>
    </select>
```
测试：
```java
    @Test
    public void testSelectSqlWhere(){
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        student stu = new student();
        stu.setUsername("艾伦");
        stu.setId(69);
        List<student> studentList = dao.SelectSqlWhere(stu);
        for (student stuList:studentList) {
            System.out.println(stuList.toString());
        }
    }
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2d1163a29cd846848e07d239403166c3.png)
## mybatis配置文件
**数据库连接配置文件properties**
把数据库的连接信息单独写到一个文件中，方便管理连接数据库的信息
在resources配置文件夹下创建文件：自定义文件名.properties
![在这里插入图片描述](https://img-blog.csdnimg.cn/d1634bfcad294d7c8f7756a03ed6a795.png)
文件中写入连接数据库的信息
格式：key=value
```
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/ccxg
username=root
password=123465
```
在主配置文件下导入文件位置并修改连接信息（使用${文件的key值}）：
```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--导入找到配置文件的位置-->
    <properties resource="Jdbc.properties"></properties>
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <typeAliases>
        <package name="com.javase.entity"/>
    </typeAliases>

    <environments default="development">

        <environment id="development">

            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${name}"/>
                <property name="password" value="${password}"/>
            </dataSource>

        </environment>
    </environments>

    <mappers>
        <mapper resource="com/javase/dao/StudentDao.xml"/>
    </mappers>

</configuration>
```
**指定多个sql映射文件（mapper）**
假如有多个mapper文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/d41ac6c2eba84402826fa84b011bb71a.png)
需要在主配置文件下添加sql映射文件
```html
第一种写法：
    <mappers><!--指定文件路径-->
        <mapper resource="com/javase/dao/StudentDao.xml"/>
        <mapper resource="com/javase/dao/NewStudentDao.xml"/>
    </mappers>
第二种写法：
    <mappers><!--指定文件的包，此包下所有的xml文件都会被加载到mybatis中-->
        <package name="com.javase.dao"/>
    </mappers>
```
## 扩展
数据展示的翻页插件：
在pom.xml文件中加入pagehelper的依赖：
```html
<!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.3.0</version>
</dependency>
```
在mybatis的主配置文件加入插件：
在environments标签之前
```html
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
```
测试：
```java
	@Test
    public void testSelect() {
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentDao dao = sqlSession.getMapper(StudentDao.class);
        //加入PageHelper的方法，分页
        //参数pageNum：从第几页开始
        //PageSize：一页中有多少数据
        PageHelper.startPage(1,3);

        List<student> studentList = dao.SelectStudents();

        for (student stu:studentList) {
            System.out.println(stu.toString());
        }
        sqlSession.close();
    }
```

