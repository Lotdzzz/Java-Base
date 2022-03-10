@[TOC](目录)
# HTML：
## html标签/head标签/body标签
```html
<!--!DOCTYPE html表示使用HTML5-->
<!DOCTYPE html>

<!--根-->
<html>

	<!--头-->
  <head>
    <title>标题</title>
  </head>
  
  	<!--体-->
  <body>
 	标签体（主要写内容）
  </body>
  
</html>

HTML注释语法：<!---->
```
## HTML基本标签
```html
<!DOCTYPE html>
<html>
  <head>
    <title>基础标签</title>
  </head>
  <body>
	  <!--段落标签-->
	  <p>第一段落</p>
	  
	  <!--标题字-->
	  <H1>标题</H1>
	  <H2>标题</H2>
	  <H3>标题</H3>
	  <H4>标题</H4>
	  <H5>标题</H5>
	  <H6>标题</H6>
	  
	  <!--换行-->
	  hello<br>world<br>
	  
	  <!--横线-->
	  <hr>
	  <!--color是hr标签的属性-->
	  <hr color="aqua">
	  
	  <!--预留格式-->
	  <pre>
123456
		  123456
		  123456
	  </pre>
	  
	  <del>删除字</del>
	  <ins>插入字</ins>
	  <b>粗体字</b>
	  <i>斜体字</i>
	  <br>
	  
	  <!--右上角加字-->
	  10<sup>2</sup>
	  <!--右下角加字-->
	  10<sub>m</sub>
	  <br>
	  
	  <!--字体标签-->
	  <font color="aqua" size="20">字体标签</font>
	  
  </body>
</html>
```
**展示效果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c7e9ea7a20c14099a1d09c7877383539.png)
## HTML实体符号
```html
<!DOCTYPE html>
<html>
  <head>
    <title>实体符号</title>
  </head>
  <body>
	  <!--实体符号表示<>的转义字符-->
	  <!--
	  &gt; 是 >
	  &lt; 是 <
	  -->
	  a &lt; b &gt; c
	  
	  <!--空格的实体符号：&nbsp;-->
	  abc&nbsp;abc
  </body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/51f9942e52aa49628e28c7eacc22cd78.png)
## HTML表格
### 基本表格
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML表格</title>
  </head>
  <body>
	  <!--表格标签
	  属性：
	  border设置表格的边框为1像素
	  width设置宽度 还可以写成百分比的形式
	  height设置高度
	  -->
	  <table align="center" border="1px" width="30%" height="100px">
		  <!--一行
		  tr的属性：
		  align对齐方式 center居中 left左对齐 right右对齐
		  -->
		  <tr align="center">
			  <!--一个单元格-->
			  <td>a</td>
			  <td>b</td>
			  <td>c</td>
		  </tr>
		  <tr>
			  <td>a</td>
			  <td>b</td>
			  <td>c</td>
		  </tr>
		  <tr>
			  <td>a</td>
			  <td>b</td>
			  <td>c</td>
		  </tr>
	  </table>
  </body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/dfc6aba93954483b8f8329dc18269ad3.png)
### HTML单元格合并
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML表格单元格合并</title>
  </head>
  <body>
	  <table align="center" border="1px" width="30%" height="100px">
		  <tr align="center">
			  <!--一个单元格-->
			  <td>a</td>
			  <td>b</td>
			  <td>c</td>
		  </tr>
		  <tr>
			  <!--单元格合并属性行合并：colspan-->
			  <td colspan="2">a</td>
			  <!-- <td>b</td> -->
			  
			  <!--单元格合并属性列合并：rowspan-->
			  <td rowspan="2">c</td>
		  </tr>
		  <tr>
			  <td>a</td>
			  <td>b</td>
			  <!--删掉一个-->
			  <!-- <td>c</td> -->
		  </tr>
	  </table>
  </body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/59d26a565cec4f63a33fcd6aa51672cb.png)
**th标签：**
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML表格单元格合并th标签</title>
  </head>
  <body>
	  <table align="center" border="1px" width="30%" height="100px">
		  <tr align="center">
			  <!--一般第一行是th：自动加粗，居中-->
			  <th>a</th>
			  <th>b</th>
			  <th>c</th>
		  </tr>
		  <tr>
			  <td colspan="2">a</td>
			  <td rowspan="2">c</td>
		  </tr>
		  <tr>
			  <td>a</td>
			  <td>b</td>
		  </tr>
	  </table>
  </body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e93219d3d7904454a0ba5ecd3c7ef855.png)
### thead标签/tbody标签/tfoot标签
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML表格单元格合并th标签</title>
  </head>
  <body>
	  <table align="center" border="1px" width="30%" height="100px">
		  <!--表格头-->
		  <thead>
			  <tr align="center">
				  <!--一般第一行是th：自动加粗，居中-->
				  <th>a</th>
				  <th>b</th>
				  <th>c</th>
			  </tr>
		  </thead>
		  <!--表格体-->
		  <tbody>
			  <tr>
				  <td colspan="2">a</td>
				  <td rowspan="2">c</td>
			  </tr>
			  <tr>
				  <td>a</td>
				  <td>b</td>
			  </tr>
		  </tbody>
		  <!--表格尾-->
		  <tfoot>
			  
		  </tfoot>
		  
	  </table>
  </body>
</html>
```
## 背景色和背景图片
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML背景色背景图片</title>
  </head>
  <!--设置背景颜色
  属性：bgcolor
  背景图片
  属性：background
  -->
  <body bgcolor="aqua" background="img/register.jpg">
  </body>
</html>
```
## 图片img标签
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML图片</title>
  </head>
  <body>
	  <!--
	  图片标签img 属性src：图片的位置 width宽度
	  属性title设置鼠标悬停时，设置图片的信息
	  属性alt图片没加载时显示的信息
	  -->
	  <img src="img/register.jpg" width="500px" title="图片" alt="图片找不到" />
  </body>
</html>
```
## 超链接
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML超链接</title>
  </head>
  <body>
	  <!--超链接:<a></a>
	  属性：href链接地址 点一下可以跳转到此地址
	  可以是绝对路径地址或相对路径 静态资源
	  -->
	  
	  <a href="http://www.baidu.com">百度</a>
	  
	  <!--图片超链接-->
	  <a href="http://www.baidu.com">
		  <img src="img/register.jpg" />
	  </a>
	  <!--属性target：_blank表示开启新窗口
	  target:_self _top _parent-->
	  <a href="http://www.baidu.com" target="_blank">
	  		  <img src="img/register.jpg" />
	  </a>
  </body>
</html>
```
## 列表
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML超链接</title>
  </head>
  <body>
	  <!--有序列表
	  type属性：
	  1
	  a
	  I
	  -->
	  <ol type="I">
		  <li>a</li>
		  <li>b</li>
		  <li>c</li>
	  </ol>
	  
	  <!--无序列表
	  属性type
	  circle 实心圆圈
	  square 方块
	  -->
	  <ul type="circle">
		  <li>a
			<ul>
				<li>a.1</li>
				<li>a.2</li>
				<li>a.3</li>
			</ul>
		  </li>
		  <li>b</li>
		  <li>c</li>
	  </ul>
  </body>
</html>
```
## 表单
```html
<!DOCTYPE html>
<html>
  <head>
    <title>HTML超链接</title>
  </head>
  <body>
	  <!--表单form
	  属性action：服务器的地址
	  method属性：get:地址后显示参数 post:不显示参数，隐藏参数
	  -->
	  <form action="http://www.baidu.com" method="post">
		  
		  <!--带参数传值到访问地址
		  form表单会把name属性以键值对的形式传入到访问地址
		  结果：https://www.baidu.com/?username=jack&password=132&sex=man&interest=java&interest=c%2B%2B&grade=bk&text=1234567498
		  -->
		  <input type="text" name="username" />
		  <input type="text" name="password" />
		  <!--radio如果name属性一样则是单选按钮
		  checked属性默认选中
		  -->
		  <input type="radio" name="sex" value="man" checked/>男
		  <input type="radio" name="sex" value="woman"/>女
		  
		  <!--复选框可多选checkbox-->
		  <input type="checkbox" name="interest" value="java" />java
		  <input type="checkbox" name="interest" value="c++" />c++
		  
		  <!--下拉列表选项
		  selected默认选中
		  -->
		  <select name="grade">
			  <option value="gz">高中</option>
			  <option value="zk">专科</option>
			  <option value="bk" selected>本科</option>
		  </select>
		  
		  <!--文本域
		  rows行数
		  cols列数
		  文本域没有value 填写的内容就是value
		  -->
		  <textarea rows="5" cols="10" name="text"></textarea>
		  
		  <!--提交表单内的数据 input
		  属性type：submit
		  属性value：提交按钮的名字
		  -->
		  <input type="submit" value="提交表单"/>
		  <input type="reset" value="清空" />
	  </form>
  </body>
</html>
```
### 下拉列表框多选
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <!--select属性multiple支持多选 按住ctrl或shift
	  size显示下拉条目数量
	  -->
	  <select multiple="multiple" size="2">
		  <option>山东</option>
		  <option>广西</option>
		  <option>浙江</option>
	  </select>
  </body>
</html>
```
### file控件
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <!--file控件 可以自己选本地文件-->
	  <input type="file" />
  </body>
</html>
```
### hidden控件
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <form action="">
		  <!--hidden隐藏控件
		  属性是hidden的标签不会显示到页面上 但是会被提交数据
		  -->
		  <input type="hidden" name="id" value="1" />
		  <input type="submit" />
	  </form>
  </body>
</html>
```
### readonly和disabled
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <!--readonly 不可改-->
	  <input type="text" value="123" readonly />
	  <!--disabled 不可以改也不可选 同时也不能被form表单提交数据-->
	  <input type="text" value="123456" disabled>
  </body>
</html>
```
### maxlength属性
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <!--maxlength控制文本框中可输入的字符数量-->
	  <input type="text" maxlength="3" />
  </body>
</html>
```
## HTML中标签的id属性
**每个HTML标签都可以有id属性，而JavaScript可以通过id对这条标签进行操作**
**ID值不能重复 ID可自定义 不会被表单提交**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <input type="text" id="text" />
	  
	  <input type="button" id="Btn" />
	  
	  <select id="select">
		  <option>1</option>
	  </select>
	  
	  <tbody id="Body">
		  
	  </tbody>
	  
	  ......
	  
  </body>
</html>
```
## div和span
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <!--div：图层-->
	  <div>第一个div</div>
	  
	  <div>第二个div
		  <div>div嵌套</div>
	  </div>
	  
	  <span>span标签</span>
  </body>
</html>
```
# CSS
## HTML引入CSS的三种方式
### 内联定义
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <!--内联定义方式
	  在style属性定义样式表属性 以分号结尾 可以添加多个样式
	  <标签 style="样式名:样式值; 样式名:样式值"></标签>
	  属性 display:none; 表示隐藏此标签 display:block; 表示显示
	  样式：border:边框
	  -->
	  <div style="width: 300px; height: 300px;background-color: aqua;display: block;
	  border: 10px solid black;"></div>
	  
  </body>
</html>
```
### 定义内部样式块对象
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<!--
	在<head>标签<html>标签之间插入一个<style></style>块对象
	<style type="text/css">
		选择器{
			样式名:样式值;
			样式名:样式值;
		}
	</style>
	-->
	<style type="text/css">
		/*
		id选择器
		*/
		#exceptionMessage{
		color: red;
		font-size: 50px;
		}

		/*
		标签选择器
		*/
		div{
		color: black;
		background-color: #FF0000;
		}
		/*
		类选择器
		*/
	    .text{
			border: aqua solid red;
		}
	</style>
  </head>
  <body>
	  <span id="exceptionMessage">异常错误信息</span>
	  <div>div异常错误信息</div>
	  <input type="text" value="123" class="text"/>
  </body>
</html>
```
### 链入外部样式表文件xxx.css
**HTML：**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<!--独立xxx.css文件引入到当前html
		语法格式:<link type="text/css rel="stylesheet" href="css文件的路径">
		-->
	<link rel="stylesheet" type="text/css" href="css.css" />
  </head>
  <body>
	  <span id="exceptionMessage">异常错误信息</span>
	  <div>div异常错误信息</div>
	  <input type="text" value="123" class="text"/>
  </body>
</html>
```
**CSS：**
```c
#exceptionMessage{
	color: red;
			font-size: 50px;
}
div{
	color: black;
			background-color: #FF0000;
}
.text{
	border: aqua solid red;
}
```
## 列表样式
**HTML：**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<link rel="stylesheet" type="text/css" href="css.css" />
  </head>
  <body>
	  <ul>
		  <li>中国
			  <ul>
				  <li>北京</li>
				  <li>上海</li>
				  <li>广东</li>
			  </ul>
		  </li>
		  <li>美国</li>
	  </ul>
  </body>
</html>
```
**CSS：**
```html
ul{
	/*
	隐藏列表标签的圆点list-style-type: none;
	*/
	list-style-type: none;
}
```
## 绝对定位
**HTML：**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<link rel="stylesheet" type="text/css" href="css.css" />
  </head>
  <body>
	  <div id="div1">div1</div>
  </body>
</html>
```
**CSS：**
```html
#div1{
	background-color: aqua;
	/*
	绝对定位
	left距离左边距离
	top距离顶部距离
	*/
	position: absolute;
    left: 100px;
	top: 100px;
}
```

