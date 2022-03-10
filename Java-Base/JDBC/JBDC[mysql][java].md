@[TOC](目录)
# 1：JBDC介绍
**JDBC：Java DataBase Connectivty（java语言连接数据库）**
> JDBC的本质是一个接口，功能是java用来连接数据库的接口 
> 每一个数据库的实现原理不同，而jdbc提供了每个数据库的连接方式
> jdbc接口让java语言连接数据库的灵活性增高
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/81a0f08832e144fc85531d2ac6721311.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
# 2：JDBC编程六步
## 准备工作
**准备JDBC的jar包（在C:\Program Files (x86)\MySQL\Connector J 8.0\）目录下的mysql-connector-java-8.0.26.jar**
****
在高级系统设置中的环境变量中的CLASSPATH中添加mysql-connector-java-8.0.26.jar的目录：.;%JAVA_HOME%\lib\tools.jar;%JAVA_HOME%\lib\mysql-connector-java-8.0.26.jar;%JAVA_HOME%
**找到mysql-connector-java-8.0.26.jar就可以**
![在这里插入图片描述](https://img-blog.csdnimg.cn/db2a2bf52a4a48f6ab616ca5865357fa.png)
**IDEA配置方法：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/ead1646d140b4e469f7d511c27fa7414.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/cd0ff0df083a4432b1c200e53d7175f3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**选择模块导入jar包**
![在这里插入图片描述](https://img-blog.csdnimg.cn/d9979be5a11b419d9bd23241770f2155.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3460909348d1493e986c26c93e5a38a2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## JBDC六步
```java
package com.jdbc;

import java.sql.*;

public class JdbcTest {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        try {
            //第一步:注册驱动
            //有受检异常需要try或者抛出
            /*
            代码：java.sql.Driver driver = new com.mysql.cj.jdbc.Driver();
            用父类型的引用(java.sql.Driver)指向子类型的对象(com.mysql.cj.jdbc.Driver())
             */
            Driver driver = new com.mysql.cj.jdbc.Driver();
            DriverManager.registerDriver(driver);

            //第二步:获取连接
            /*
            Connection获取连接，使用DriverManager的静态方法getConnection();
            需要穿三个参数：
            url是连接的 数据库类型mysql 连接地址localhost 数据库名ccxg
            username是数据库的登陆名
            password是数据库的登陆密码
             */
            String url = "jdbc:mysql://localhost/ccxg";
            String username = "root";
            String password = "123456";
            connection = DriverManager.getConnection(url,username,password);

            //第三步:获取数据库操作对象
            statement = connection.createStatement();

            //第四步:执行sql语句
            String sql = "insert into tbl_user(id,name,sex) values(3,'张三','男')";
            /*
            int count是返回结果，如果插入成功则返回1 加入插入3条 成功的话则会返回3
             */
            int count = statement.executeUpdate(sql);


        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //第六步:关闭资源
            /*
            关闭资源需要从小打到关
            先关闭statement再关闭Connection
            注：不能一起try catch 要分开try
             */
            if(statement != null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(connection != null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
**结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/3259561a1d8d4f459c8cc99fe12612af.png)
## 类加载的方式/读取资源文件的方式
**类加载的方式：**
```java
package com.jdbc;

import java.sql.*;

public class JdbcTest {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        try {
            //用类加载的方式注册驱动：Class.forName();
            Class.forName("com.mysql.cj.jdbc.Driver");
            
            String url = "jdbc:mysql://localhost/ccxg";
            String username = "root";
            String password = "123456";
            connection = DriverManager.getConnection(url,username,password);
            statement = connection.createStatement();
            String sql = "insert into tbl_user(id,name,sex) values(3,'张三','男')";
            int count = statement.executeUpdate(sql);
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        }finally {
            if(statement != null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(connection != null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
**读取资源文件的方式：**
> 创建资源文件：jdbc.properties
> 内容如下：
```java
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost/web-crm
jdbc.name=root
jdbc.password=123456
```
```java
package com.jdbc;

import java.sql.*;
import java.util.ResourceBundle;

public class JdbcTest {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        try {
            Class.forName(bundle.getString("jdbc.Driver"));
            String url = bundle.getString("jdbc.url");
            String username = bundle.getString("jdbc.name");
            String password = bundle.getString("jdbc.password");
            connection = DriverManager.getConnection(url,username,password);
            statement = connection.createStatement();
            String sql = "insert into tbl_user(id,name,sex) values(3,'张三','男')";
            int count = statement.executeUpdate(sql);
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        }finally {
            if(statement != null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(connection != null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
## JDBC第五步处理结果集
```java
package com.jdbc;

import java.sql.*;
import java.util.ResourceBundle;

public class JdbcTest {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        ResourceBundle bundle = ResourceBundle.getBundle("com/jdbc/jdbc");
        /*
        创建结果集的对象
         */
        ResultSet resultSet = null;
        try {
            Class.forName(bundle.getString("jdbc.driver"));
            String url = bundle.getString("jdbc.url");
            String username = bundle.getString("jdbc.name");
            String password = bundle.getString("jdbc.password");
            connection = DriverManager.getConnection(url,username,password);
            statement = connection.createStatement();

            String sql = "select * from tbl_user";
            resultSet = statement.executeQuery(sql);//专门执行DQL语句的方法
            //处理结果集
            //循环遍历结果集
            /*
            resultSet.next()这个方法如果有数据则返回true没有返回false
            resultSet.getString("字段名");获取一行的数据
             */
            while(resultSet.next()){
                Integer id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                String sex = resultSet.getString("sex");
                System.out.println(id+"  "+name+"  "+sex);
            }
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        }finally {
        	if(resultSet != null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(statement != null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(connection != null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
**结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/83aa1853f99940ed9d3d45226014f829.png)
# 3：PreparedStatement的使用
**PreparedStatement使用方法（可以解决sql注入等问题）：**
```java
package com.jdbc;

import java.sql.*;
import java.util.ResourceBundle;
import java.util.Scanner;

public class JdbcTest {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Connection connection = null;
        /*
        创建PreparedStatement对象
        预编译的数据操作对象
         */
        PreparedStatement ps = null;
        ResourceBundle bundle = ResourceBundle.getBundle("com/jdbc/jdbc");
        ResultSet resultSet = null;
        try {
            Class.forName(bundle.getString("jdbc.driver"));
            String url = bundle.getString("jdbc.url");
            String username = bundle.getString("jdbc.name");
            String password = bundle.getString("jdbc.password");
            connection = DriverManager.getConnection(url,username,password);
            /*
            将条件写成?
            一个?表示一个占位符
             */
            String sql = "select * from tbl_user where name = ?";
            ps = connection.prepareStatement(sql);

            //给?占位符传值，按照?个数以1递增 第一个?下标是1
            //ps.setString参数第一个是?的下标第二个是值
            ps.setString(1,"zs");

            //获取结果集不用传参数了
            resultSet = ps.executeQuery();

            if(resultSet.next()){
                Integer id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                System.out.println(id+name);
            }
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        }finally {
            if(resultSet != null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(ps != null){
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(connection != null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
# 4：JDBC事务机制
```java
package com.jdbc;

import java.sql.*;
import java.util.ResourceBundle;
import java.util.Scanner;

public class JdbcTest {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Connection connection = null;
        PreparedStatement ps = null;
        ResourceBundle bundle = ResourceBundle.getBundle("com/jdbc/jdbc");
        ResultSet resultSet = null;
        try {
            Class.forName(bundle.getString("jdbc.driver"));
            String url = bundle.getString("jdbc.url");
            String username = bundle.getString("jdbc.name");
            String password = bundle.getString("jdbc.password");
            connection = DriverManager.getConnection(url,username,password);
            /*
            关闭自动提交，开启事务机制connection.setAutoCommit(false);
             */
            connection.setAutoCommit(false);
            
            String sql = "insert into tbl_user(id,name,sex) values(5,'ww','男')";
            ps = connection.prepareStatement(sql);
            int count = ps.executeUpdate();
            if(count != 1){
                throw new Exception();
            }
            /*
            如果程序执行到这没有异常的话提交事务connection.commit();
             */
            connection.commit();
            
            if(resultSet.next()){
                Integer id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                System.out.println(id+name);
            }
        } catch (Exception e) {
            /*
            设置回滚事务
            当程序出现异常时，会执行事务回滚
             */
            if(connection != null){
                try {
                    connection.rollback();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
            e.printStackTrace();
        }finally {
            if(resultSet != null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(ps != null){
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(connection != null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
# 5：悲观锁和乐观锁
**悲观行级锁：for update**
**例子：select ename,sal from emp where sal = 3000 for update;**
**作用：sal = 3000的数据在事务内被锁住了，其他事务无法对这些数据进行操作**
> 悲观锁特点：事务必须排队，数据锁住了，不能并发
> 乐观锁特点：支持并发，事务不需要排队，但需要版本号
