@[TOC](目录)

## Maven

> maven是一个项目的构建工具 
> maven的下载地址：[maven官网](https://maven.apache.org/download.cgi)
> 配置环境变量和jdk配置方法相同
> 使用方式：
> 1：独立使用：cmd命令来完成maven的使用比如编译命令：mvn compile
> 2：配合IDEA使用
> maven的作用： 
> 1：管理依赖jar包，jar的管理，自动下载jar包
> 2：构建项目，完成项目代码的编译，测试，打包，部署

Maven的核心概念：
目录的构造：
![在这里插入图片描述](https://img-blog.csdnimg.cn/d6d3cd6e661443bf97b02f278161b039.png)
和src平级目录下有个pom.xml文件是配置文件
src目录下
main的java
主程序java的所有文件
resources
为项目的所有配置文件
src下的test为测试文件夹(可选)

## Maven的仓库
在启动maven的时候maven会从网络上自动下载资源依赖到仓库内
默认的仓库路径是C：用户/administrator/.m2./repository
目录可以在maven的安装目录下的conf/settings.xml下的locaRepository更改

**仓库的分类**

> 本地仓库：个人计算机上的文件夹，存放各种jar 
> 远程仓库：在互联网上的，需要使用网络才能使用的仓库 	远程：
> 		1：中央仓库，最权威的，共享使用的一个集中仓库[中央仓库](https://repo.maven.apache.org/maven2/)
> 		2：中央仓库的备份
> 		3：私服，在局域网中使用

**仓库的使用**
需要mysql的驱动——>
maven调用仓库顺序：本地仓库(如果没有资源会往下调用)——私服——中央仓库的镜像——中央仓库

## pom.xml

```java
<modelVersion>4.0.0</modelVersion>
modelVersion是maven模型的版本，目前是4.0.0
-----------------------------------------
<groupId>com.javaMaven</groupId>
表示组织id，一般都是域名的倒写，
比如com.javaMaven.项目名(项目名可不写)
-----------------------------------------
<artifactId>项目名</artifactId>
可以自定义的，项目的名字
-----------------------------------------
<version>1.0-SNAPSHOT</version>
项目的版本号，加上SNAPHOT是快照，表示项目不是稳定的还在开发中
-----------------------------------------
packaging
项目打包的类型：jar，war，rar，ear，pom，默认是jar
-----------------------------------------
dependencies和dependency是依赖
项目中要使用的各种资源说明
比如从中央仓库中搜索的mysql驱动
<dependencies>
	<dependency>
	    <groupId>mysql</groupId>
	    <artifactId>mysql-connector-java</artifactId>
	    <version>8.0.27</version>
	</dependency>
</dependencies>
-----------------------------------------
```

> 依赖范围： 
> 依赖范围用scope表示 
> scope有：compile(默认的),test,provided
> scope范围就是在maven项目中哪些阶段起作用

![在这里插入图片描述](https://img-blog.csdnimg.cn/91b9c462c4ce43ecb27b3f6cdd055832.png)
以compile依赖的范围：**是compile阶段，test阶段，package阶段，deploy阶段**
以test依赖的范围是：**test阶段**
以provided依赖的范围是：**compiole阶段，test阶段**
```java
-----------------------------------------
properties
设置属性
-----------------------------------------
build
maven在进行项目的构建时，配置信息
```
groupId+artifactId+version=坐标(gva)(项目中必须有)
可以通过坐标来查找项目
[搜索使用的中央仓库](https://mvnrepository.com/) 使用groupId和artifactId来搜索

## Maven常用命令

```java
mvn clean:清理删除target，仓库和main不会被清理
mvn compile:编译主程序，会生成target里面存放class
mvn test-compile:编译测试程序
mvn test:测试会生成一个surefire-reports文件，保留测试结果
mvn package:打包主程序，会编译主程序，测试程序，测试，并按照pom配置文件打包
mvn install:安装主程序，会把本工程打包，并且吧工程坐标保存到本地仓库中
mvn deploy:部署主程序，会把工程打包保存本地仓库，私服仓库，还会把项目部署到web容器中
```
**单元测试(测试方法)**
junit：
junit是一个测试工具，可以批量的测试类中的方法
使用步骤：
加入单元测试的依赖

```html
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
	</dependency>
```
cmd指令：mvn test-compile
**配置maven构建项目的参数设置**
比如在pom文件设置jdk版本：

```html
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>插件的名字
			<version>3.8.1</version>插件的版本
			<configuration>
				<source>1.8</source>写的代码是在jdk1.8上编译的
				<target>1.8</target>程序运行在1.8的jdk上
			</configuration>
		</plugin>
	</plugins>
</build>
```

## Maven常用操作：

> maven的全局变量 
> 可以自定义的属性，在properties通过这个标签来声明变量
> 在pom.xml文件中的其他位置，可以通过${变量名}来使用变量的值
> 作用：
> 自定义全局变量一般是定义依赖的版本号，项目中要使用的版本号，
> 
> 先使用全局变量定义变量，在使用${变量名}

**资源插件**

> maven在编译代码时，会把src/main/resources目录下的配置文件拷贝到target/classes目录中
> 对于src/main/java目录下的配置文件不处理 
> 如果需要java目录下的配置文件时，需要用到《resources》这个标签命令

```html
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
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>  
    </resources>
```

## 在idea中配置maven
idea配置maven方法：
配置当前工程：file——settings——Build，Execution，Deployment——Build Tools
配置以后新工程：file——new project settings——Build，Execution，Deployment——Build Tools
两个都要设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/9b2bd089b5d2477392fc542b31c7ac1d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_13,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/95f96cfd94ca4ea68548b7bf2f0e8024.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0ec743a40a2d4f3cb1113901b316a7cf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/abce0b6ea8b240e7bb7e5bb4a4f787db.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**创建maven项目**
![在这里插入图片描述](https://img-blog.csdnimg.cn/cc42f2e51f694d318e3c48c384fff123.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**如果想要创建web工程就是：maven-archetype-webapp**
![在这里插入图片描述](https://img-blog.csdnimg.cn/246b6260b23348b0940eb682d75428e4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/482d7df9c1af41b3baf1b619cd7ce30a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/765a0c90c6f24845a2eeae7e69c1e6a9.png)
**创建配置文件resources**
在main目录下创建resources文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/f7260eba24ce4043b9e1f54ca26c6644.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/d8bee24a6aec4ce1ae928f372452c899.png)
会自动转为配置文件夹
**test文件夹下同样创建resources**
![在这里插入图片描述](https://img-blog.csdnimg.cn/1939a5cbcb444c9fbb68d47478a88151.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

## pom文件依赖报红处理
![在这里插入图片描述](https://img-blog.csdnimg.cn/fbd72a810bfc4720bce21556b93818f4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## Maven继承
> Maven多模块管理
> 让一个模块管理所有模块的依赖
> 让模块之间可以继承，达到统一管理的程度
> 提升程序的统一性
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/82930fae350047799a85520866546172.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
### Maven多模块管理使用方法
#### 创建父类模块
> 创建空项目
> 创建父类模块
> 父类条件：
> 1：packaging标签的内容必须设置为pom
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1090855dd0074ebb94a0cb74ea5bc7d5.png)
```html
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.java</groupId>
  <artifactId>java-parent</artifactId>
  <version>1.0.0</version>

  <packaging>pom</packaging>

<!--  统一管理依赖的版本号
使用方法：依赖的artifactId+-version标签
-->
  <properties>
    <mysql-connector-java-version>8.0.27</mysql-connector-java-version>
    <junit-version>3.8.1</junit-version>
  </properties>

  <!--统一管理所有子模块的依赖-->
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit-version}</version>
      </dependency>
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <!--使用时，直接引用版本号-->
        <version>${mysql-connector-java-version}</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
</project>
```
#### 创建子类模块
***先创建普通web/java项目***
![在这里插入图片描述](https://img-blog.csdnimg.cn/4367fb057ec14651b0edea1f95f284dc.png)
***在pom文件内加入parent标签实现继承，并且删除自己pom的groupId标签和version标签***
```html
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>com.java</groupId>
    <artifactId>java-parent</artifactId>
    <version>1.0.0</version>
    <relativePath>../java-parent/pom.xml</relativePath>
  </parent>

  <artifactId>java-web</artifactId>
  <packaging>war</packaging>
  
  <dependencies>
    <!--继承依赖，不需要写版本号直接声明依赖即可-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
    </dependency>
  </dependencies>

</project>
```
