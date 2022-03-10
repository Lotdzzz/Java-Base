@[TOC](目录)
# Thymeleaf介绍

> Thymeleaf是比jsp功能更加强大，效率更快的模板
> SpringBoot集成了Thymeleaf模板
> Thymeleaf模板是基于HTML的，以HTML标签为载体
> Thymeleaf官网：[Thymeleaf](https://www.thymeleaf.org/)
> Thymeleaf手册：[Thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html)
## Thymeleaf常用设置
> 在SpringBoot的application配置文件中
> 在Controller层写路径时可以省略前后缀
> 字符串连接可以用双||
> 运算符：
> gt——>
> lt——<
> ge——>=
> le——<=
> ==——eq
> !=——ne
```html
#模板引擎的缓存机制
#一般在开发阶段关闭，在发布阶段开启
spring.thymeleaf.cache=false

#编码格式（默认是utf-8）
spring.thymeleaf.encoding=utf-8

#模板类型（默认是HTML）
spring.thymeleaf.mode=HTML

#模板的前缀，默认是resources下的templates
spring.thymeleaf.prefix=classpath:/templates/

#模板的后缀，默认是.html
spring.thymeleaf.suffix=.html
```
# Thymeleaf使用

> 在HTML的html标签中加入引用xmlns:th="http://www.thymeleaf.org"
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
</html>
```
> thymeleaf模板起步依赖
```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
## 标准变量表达式
> 语法格式：th:text="${key}"
> 作用：获取key对应的作用域数据
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div>
    <p th:text="${user}"></p>
    <br>
    <p>对象属性</p>
    <!--使用${user.id}或${user.getId()}都可以获取数据-->
    <p th:text="${user.id}">id</p>
    <p th:text="${user.name}">name</p>
    <p th:text="${user.sex}">sex</p>
</div>
</body>
</html>
```
## 选择变量表达式
> 语法：*{key}
> 作用：获取key对应的作用域对象，但不能单独用，需要和th:object一起使用
> 注：*{key}需要在th:object="${key}的标签子标签内
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div th:object="${user}">
    <p>对象属性</p>
    <p th:text="*{id}">id</p>
    <p th:text="*{name}">name</p>
    <p th:text="*{sex}">sex</p>
</div>
</body>
</html>
```
## 链接表达式
> 语法：@{url}
> 作用：表示超链接
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<a th:href="@{http://www.baidu.com}">绝对路径</a>
<a th:href="@{/tpl/value1}">相对地址</a>
<a th:href="@{'/tpl/value1?id='+1}">相对地址带参数</a>
<a th:href="@{/tpl/value1(id=1,name='zs')}">传参数方式</a>
<!--字符串连接可以用双||-->
<a th:href="|@{/tpl/value1?id=1}|">相对地址带参数</a>
</body>
</html>
```
## Thymeleaf属性
> Thymeleaf属性是原HTML原属性加th即可
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" th:src="@{/js/jquery.js}"></script>
</head>
<body>

<form th:action="@{/地址}" th:method="get或post，或者${key}可以动态获取方式">
</form>

<input type="text" th:id="${key}" th:name="${name}" th:text="${text}" th:value="${value}" />

<a th:onclick="'fun('+${key}+')'" th:style="${color}"></a>

</body>
</html>
```
### th:each循环
> 语法：th:each
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" th:src="@{/js/jquery.js}"></script>
</head>
<body>

<!--
<div th:each="集合循环成员，循环的状态变量:${key}">
    <p th:text="${集合循环成员}"></p>
</div>
循环的状态变量表示整个循环体
作用：
index：当前迭代对象的index（从0迭代）
count：当前迭代对象的个数（从1迭代）
size：被迭代的对象的大小
current：当前迭代的变量
even/odd：布尔值，当前循环是否是偶数/奇数个
first：布尔值，当前循环是否是第一个
last：布尔值，当前循环是否是最后一个
-->

<div th:each="userList,userIter:${users}">
    <p th:text="${userList.id}"></p>
    <p th:text="${userList.name}"></p>
    <p th:text="${userList.sex}"></p>
    <!--循环状态变量-->
    <p th:text="${userIter.index}"></p>
    <p th:text="${userIter.count}"></p>
    <p th:text="${userIter.size}"></p>
    <p th:text="${userIter.current}"></p>
    <p th:text="${userIter.even}"></p>
    <p th:text="${userIter.odd}"></p>
    <p th:text="${userIter.first}"></p>
    <p th:text="${userIter.last}"></p>
</div>

<!--循环Map集合-->
<div th:each="map,mapIter:${users}">
    <p th:text="${m.key}"></p>
    <p th:text="${m.value.id}"></p>
    <p th:text="${m.value.name}"></p>
    <p th:text="${m.value.sex}"></p>
</div>

</body>
</html>
```
### if判断
> 语法：th:if="boolean条件"
> 作用：如果表达式为真，则显示标签内容
> 语法：th:unless="boolean条件"
> 作用：如果表达式为假，则显示标签内容
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" th:src="@{/js/jquery.js}"></script>
</head>
<body>

<div th:if="1==1">第一个div</div>

<div th:if="1==2">unless</div>

</body>
</html>
```
### switch判断
> 语法：th:switch="${key}"  th:case="值"
> 作用：显示case相等的
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" th:src="@{/js/jquery.js}"></script>
</head>
<body>

<div th:switch="${key}">
    <p th:case="id1">显示zs</p>
    <p th:case="id2">显示ls</p>
    <p th:case="id3">显示ww</p>
</div>

</body>
</html>
```
### th:inline内联

> 语法：th:inline="text"
> 语法：th:inline="javascript"
> 作用：可以直接用内联内容显示表达式结果
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--使用JavaScript内联必须加th:inline="javascript"-->
    <script type="text/javascript" th:inline="javascript">
        //获取作用域的值
        var name = [[${user.name}]];
        var sex = [[${user.sex}]];
        alert(name+sex);
    </script>
</head>
<body>
<!--注：th:inline="text"可以省略不写-->
<div th:inline="text">
    <p>我是[[${user.name}]]</p>
</div>
</body>
</html>
```
# Thymeleaf基本对象
> #request表示HttpServletRequest对象
> #session表示HttpSession对象
> session表示HttpSession对象
## #request对象/#session对象/session对象
> 语法：${#request.key}
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" th:inline="javascript">

    </script>
</head>
<body>

<p th:text="${#request.getAttribute('user')}"></p>
<p th:text="${#request.getAttribute('user').name}"></p>

<p th:text="${#session.getAttribute('user')}"></p>
<p th:text="${#session.getAttribute('user').name}"></p>

<!--session获取属性值-->
<p th:text="${session.user}"></p>
<p th:text="${session.user.id}"></p>
<p th:text="${session.user.name}"></p>
<p th:text="${session.user.sex}"></p>

<!--获取请求参数-->
<p th:text="${param.id}"></p>
<!--参数数量-->
<p th:text="${param.size()}"></p>
</body>
</html>
```
# Thymeleaf内置工具类
## #dates
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" th:inline="javascript">

    </script>
</head>
<body>

<!--format内的date是作用域参数，不需要加单双引号-->
<p th:text="${#dates.format(date)}"></p>
<!--格式化-->
<p th:text="${#dates.format(date,'yyyy-MM-dd')}"></p>

</body>
</html>
```
## #numbers
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" th:inline="javascript">

    </script>
</head>
<body>

<p th:text="${#numbers.formatCurrency(23.4324)}"></p>
<p th:text="${#numbers.formatDecimal(1000,5,2)}"></p>
</body>
</html>
```
## #strings
```html
${#strings.toString(obj)}
${#strings.isEmpty(name)}
${#strings.arrayIsEmpty(nameArr)}
${#strings.listIsEmpty(nameList)}
${#strings.setIsEmpty(nameSet)}
${#strings.indexOf(name,frag)}
${#strings.substring(name,3,5)}
${#strings.substringAfter(name,prefix)}
${#strings.substringBefore(name,suffix)}
${#strings.replace(name,'las','ler')}
```
## #lists
```html
${#lists.toList(object)}
${#lists.size(list)}
${#lists.isEmpty(list)}
${#lists.contains(list, element)}
${#lists.containsAll(list, elements)}
${#lists.sort(list)}
```
## null处理
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" th:inline="javascript">

    </script>
</head>
<body>

<!--如果key为null则显示在页面为null处理-->
<p th:text="${user?.name}"></p>

</body>
</html>
```
# Thymeleaf自定义模板（内容复用）
> 定义模板语法：th:fragment="模板自定义名称"
> 引用模板语法：~{模板的文件名称 :: 自定义模板名称}
> 第二种格式：模板的文件名称 :: 自定义模板名称
> *****
> 模板的使用：包含模板（th:include），插入模板（th:insert）
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div th:fragment="top">
    <p>自定义模板</p>
    <p>www.localhost.com</p>
</div>

</body>
</html>
```
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div th:insert="~{ head :: top }">
    insert方式使用模板
</div>

<div th:insert="head :: top">
    insert方式使用模板
</div>

<div th:include="~{ head :: top }">
    include方式
</div>

<div th:include="head :: top">
    include方式
</div>

<div th:include="footer :: html">
    整个html文件当做模板使用
</div>

<div th:include="footer">
    整个html文件当做模板使用
</div>

<div th:include="fragment :: html">
    使用其他目录中的模板
</div>

<div th:include="fragment">
    使用其他目录中的模板
</div>

</body>
</html>
```

