@[TOC](目录)
# JavaScript
## 1：HTML嵌入JS代码的第一种方式
> 第一个例子：实现弹窗功能
> JS是一门事件驱动型的编程语言，依靠事件去驱动，然后执行对应的程序
> JS中的鼠标单击事件：click
> 事件句柄：onclick=""
> 注：事件句柄是以HTML标签的属性存在的，每一个js语句的分号可写可不写
> ****
> 弹出消息框：
> JS有个内置对象叫做window表示此窗口对象，对象的函数：alert
> window可以省略

**代码：**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
	  <input type="button" value="hello" onclick="window.alert('hello js');alert('hellp JavaScript')" />
  </body>
</html>
```
**结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f414808a700e4b2999f9a64efe25b01d.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8a75b2f5599b487d945ce4ec2b685d40.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7e14984c895f4a73865595e3a13df15d.png)
## 2：HTML嵌入JS代码的第二种方式

> 第二种方式：脚本快的方式\<script>\</script>
> 在标签内的代码会按照以上而下的顺序依次执行，当浏览器加载页面完毕会自动执行
> 可以放在\<html>标签内，也可以放在任何标签内，或者文件开头或结尾
> js代码的注释可以有：//，多行注释：/**/
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		window.alert("hello js");
	</script>
  </head>
  <body>
	  
  </body>
</html>
```
## 3：HTML嵌入JS代码的第三种方式
> 从后缀.js文件中引入
> **\<script type="text/javascript" src="文件路径">\</script>**
> 注：script标签必须是\<script>\</script>，不能是\<script />
> 并且引入文件的话，在代码块内写的代码不会执行

**HTML文件**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript" src="JavaScript.js"></script>
  </head>
  <body>
	  
  </body>
</html>
```
**JS文件**
```html
window.alert("hello js");
```
## 4：JS的标识符
> 丶变量名只能用字母，数字，下划线，美元符号$组成不能用其他特殊符号
> 丶变量名不能以数字开头
> 丶变量名是区分大小写的
> 丶关键字不能做变量名

- **标识符命名建议用驼峰式命名规范**
  * 类名，接口名：首字母大写，后面每个单词首字母大写
  * 变量名，方法名：首字母小写，后面每个单词首字母大写
  * 常量名：全部大写
## 5：JS的变量

> JS的变量可以是任何类型，所有类型都写成var，var可以存入所有的基础数据类型
> 语法格式：**变量声明**：var 变量名; **变量赋值**：var 变量名 = 数据;
> JS属于弱类型编程语言
> 注：如果变量没有赋值默认值是：undefined
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		var value;
		alert(value);
		value = 10;
		alert(value);
		value = 3.14;
		alert(value);
		value = "hello world";
		alert(value);
		value = false;
		alert(value);
	</script>
  </head>
  <body>
	  
  </body>
</html>
```
## 6：JS的函数
> JS函数的作用就相当于c/c++的函数，java中的方法
> 语法：function 函数名(函数参数列表){代码块}
> 语法：函数名 = function(函数参数列表){代码块}
> 注：JS中的函数的参数不需要写类型，不需要写返回值，如果需要返回值则直接return就可以
> JS中的函数没有重载，如果两个函数同名则会使用后声明的函数，前声明的函数会被覆盖
**> 函数可以用在事件句柄onclick内**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		//调用函数
		dosome();
		
		function dosome(x,y){
			alert("hello");
		}
		dosome1 = function(){
			alert("hello");
		}
	</script>
  </head>
  <body>
	  
  </body>
</html>
```
## 7：JS的全局变量和局部变量
> 全局变量：在函数体外，全局变量的生命周期是浏览器打开时声明，关闭时销毁----[占内存]
> 局部变量：在函数体内，在函数执行时声明，在函数执行完销毁
**> JS也会遵循就近原则**
## 8：JS的数据类型
> JS中有原始类型，引用类型
> 原始类型(ES6之后有新类型:Symbol类型)：Undefined，Number，String，Boolean，Null
> 引用类型：Object，Object子类
> JS中有个运算符typeof，可以在程序运行阶段动态获取变量的类型
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		var un;
		alert(typeof un); //undefiend
		alert(typeof 1); //number
		alert(typeof "1"); //string
		alert(typeof false); //boolean
		alert(typeof new Object()); //object
		alert(typeof null); //object
		alert(typeof function doSome(){}); //function
	</script>
  </head>
  <body>
	  
  </body>
</html>
```
### Undefined数据类型
> undefined的类型是一个值而且只有这一个值
> 当一个变量没有手动赋值时，默认赋值undefined
> 也可以手动给一个变量赋值undefined
### Number数据类型
> Number类型包括，byte short int long float double 还包括NaN，Infinity
> 注NaN和Infinity是一个值
> NaN：表示not a number(不是一个数字，但属于number类型，运算结果应该是一个数字，但结果不是数字的时候是NaN)
> 比如var num = 100/"abc";(做除法运算结果应该是数字，但结果不是数字，则结果是NaN)
> Infinity：当运算结果无穷大时，则值为Infinity
> 比如var num = 10/0;
> 而var num = 10/3;则结果为3.33333333333。。。
函数:isNaN();
isNaN:返回结果为true表示不是一个数字，结果为false表示是一个数字
函数:parseInt();作用：可以将字符串自动转换数字，并且取整数位
函数:parseFloat();作用：将字符串自动转换成小数
函数:Math.ceil();作用：向上取整，比如1.1向上取整是2
### Boolean数据类型
> JS中的布尔类型只有两个值：true,false
> JS如果需要布尔表达式不是布尔类型的时候，if,while的时候会将非布尔类型转换成布尔类型函数：Boolean();
### String数据类型
> JS的string类型可以使用双引号，也可以使用单引号
> 创建字符串对象(String是JS的内置支持类)：
> var s = "";属于string类型
> var s = new String("");这种形式创建字符串属于Object类型
> length属性：每个字符串都有个length属性比如"abc".length;作用：获取字符串长度
> trim();作用：去除前后空白-----变量 = 变量.trim();
### Object数据类型
> Object是所有类型的超类
> 包括属性：
> prototype属性：作用：作用是给类动态的扩展属性和函数
> JS中定义类：
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		/*
		第一种：
		function 类名(形参){
			
		}
		第二种：
		类名 = function(形参){
			
		}
		在JS可以把函数当做类new 对象();
		*/
	   function User(a,b,c){
		   this.a = a;
		   this.b = b;
		   this.c = c;
		   //类中定义函数:
		   this.getC = function(){
			   return this.c;
		   }
	   }
	   //new类的时候会自动使用类内的方法（类被new出来的时候类内的代码会被当做构造函数被使用）
	   var user = new User(1,2,3);
	   alert(user.a);//第一种访问属性
	   alert(user["a"]);//第二种访问属性
	   //使用函数
	   alert(user.getC());
	   
	   //可以通过prototype这个属性可以给类扩展属性或函数
	   User.prototype.getB = function(){
		   return this.b;
	   }
	   alert(user.getB());
	</script>
  </head>
  <body>
	  
  </body>
</html>
```
***null----NaN----undefined区别/等同运算符/全等运算符***
> null和NaN和undefined数据类型不一样
> null != NaN
> null = undefined
> undefined != NaN
> ****
> ==(等同运算符，只判断值是否相等)
> ===(全等运算符，即判断值是否相等，又判断数据类型是否相等)
## 9：JS的常用事件
> JS中常用事件：
> ****
> blur：失去焦点
> focus：获得焦点
> ****
> click：鼠标点击
> dblclick：鼠标双击
> keydown：键盘按下
> keyup：键盘弹起
> load：页面加载完毕
> ****
> mousedown：鼠标按下
> mouseover：鼠标经过
> mousemove：鼠标移动
> mouseout：鼠标离开
> mouseup：鼠标弹起
> ****
> reset：表单重置
> select：文本被选定
> change：下拉列表选中项改变，或文本框内容改变
> submit：表单提交
### 注册事件
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		//window.onload表示当页面对象全部加载完毕后执行的,如果没有这个那script标签只能写在其他标签下面
		window.onload = function(){
			function doSome(){
				alert("doSome");
			}
			//可以通过标签的id获取这个标签的对象
			var BtnObj = document.getElementById("Btn");
			//给标签对象注册事件
			BtnObj.onclick = doSome;//注：script标签要写在其他标签下面
			BtnObj.onclick = function(){//可以给标签对象注册一个匿名函数
				alert("test");
			}
			//更改标签的type属性
			BtnObj.type = "checkbox";
		}
	</script>
  </head>
  <body>
	  <input type="button" value="按钮" id="Btn"/>
  </body>
</html>
```
### 捕捉回车键
> 键盘的每一个键都对应一个数字
> 回车键=13，esc键=27
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		window.onload = function(){
			var BtnObj = document.getElementById("Btn");
			//使用绑定键盘的事件,当按下一个键的时候浏览器会传来一个数字,当数字==13的时候表示按的是回车键
			//浏览器传递的参数用event接收 名字可以自定义
			BtnObj.onkeydown = function(event){
				//参数的属性keyCode是浏览器传入的值的数字
				if(event.keyCode == 13){
					alert("hello");
				}
				if(event.keyCode == 27){
					alert("close");
				}
			}
		}
	</script>
  </head>
  <body>
	  <input type="text" id="Btn"/>
  </body>
</html>
```
## 10：void运算符
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		
	</script>
  </head>
  <body>
	  <!--点击完超链接，让超链接的路径失效
	  语法：javascript:void(0)
	  javascript:是告诉浏览器这是一段js代码
	  void(0):执行表达式，但不会返回任何结果
	  -->
	  <a href="javascript:void(0)">超链接</a>
  </body>
</html>
```
## 11：js控制语句
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		var arr = [1,2,3,4,5,6];
		//js中的增强for循环：
		/*
		for中的i是数字中元素的下标, in arr表示访问那个数组
		注：for in语句也可以遍历对象的属性
		*/
		for (var i in arr) {
			alert(arr[i]);
		}
		//函数join
		arr.join("-");//结果:1-2-3-4-5-6
		//函数push 作用：在数组的末尾添加元素
		arr.push("7");
		//函数pop作用：弹出元素 根栈差不多 自动模拟栈
		arr.pop();
		//反转函数reverse
		arr.reverse();
	</script>
  </head>
  <body>
  </body>
</html>
```
# DOM编程
> DOM：Document Object Model（对网页中的节点进行增删改查）
> DOM顶级对象是Document
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b9beb7dd2324b92977ee9702e5ee4f5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 1：获取或设置文本框的value
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		window.onload = function(){
			var SubBtn = document.getElementById("SubBtn");
			var Btn = document.getElementById("Btn");
			//获取text文本框的value：Btn.value
			SubBtn.onclick = function(){
				alert(Btn.value);
			}
		}
	</script>
  </head>
  <body>
	  <input type="text" id="Btn" />
	  <input type="button" value="按钮" id="SubBtn" />
  </body>
</html>
```
## 2：innerHTML和innerText属性
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	
	<script type="text/javascript">
		window.onload = function(){
			var SubBtn = document.getElementById("SubBtn");
			SubBtn.onclick = function(){
				//设置div的标签之间的内容就是<div>这是标签之间的内容</div>
				document.getElementById("divBtn").innerText = "div123";
				//获取div的标签之间的内容
				alert(document.getElementById("divBtn").innerHTML);
			}
		}
	</script>
  </head>
  <body>
	  <input type="button" value="按钮" id="SubBtn" />
	  <div id="divBtn"></div>
  </body>
</html>
```
## 3：正则表达式
> 正则表达式作用：字符串格式匹配

**常用的元字符**
符号     | 作用
-------- | -----
.	  | 匹配除换行符以外的任意字符
\w  | 匹配字母或数字或下划线或汉字
\s  | 匹配任意的空白符
\d  | 匹配数字
\b  | 匹配单词的开始或结束
^   | 匹配字符串的开始
$   | 匹配字符串的结束

**常用的限定符**
符号     | 作用
-------- | -----
*|重复零次或更多次
+|重复一次或更多次
?|重复零次或一次
{n}|重复n次
{n,}|重复n次或更多次
{n,m}|重复n到m次
**常用的反义代码**
符号 | 作用
-------- | -----
\W|匹配任意不是字母，数字，下划线，汉字的字符
\S|匹配任意不是空白符的字符
\D|匹配任意非数字的字符
\B|匹配不是单词开头或结束的位置
[^x]|匹配除了x以外的任意字符
[^aeiou]|	匹配除了aeiou这几个字母以外的任意字符
****
> 正则表达式例子：
> QQ号的正则表达式：\^[1-9][0-9]{4,}$
> 邮箱地址：\w+([-+.]\w+)*@\w+([-.]\w+)\*\\.\w+([-.]\w+)\*
> 常用的正则表达式网站：[正则表达式大全](https://deerchao.cn/tutorials/regex/common.htm)

**使用正则表达式：**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	
	<script type="text/javascript">
		window.onload = function(){
			//第一种创建方式：var regExp = /正则表达式/;
			//第二种:var regExp = new RegExp("正则表达式或字符串","flags");
			//flags可取值:g(全局匹配) i(忽略大小写) m(多行查找,如果是正则表达式不能用m)
			//正则表达式对象.test(用户填写的文本); 返回true表示成功 false表示失败
			document.getElementById("SubBtn").onclick = function(){
				//获取文本信息
				var email = document.getElementById("TBtn").value;
				//创建正则表达式
				var emailRegExp = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
				//判断正则表达式与用户信息
				var ok = emailRegExp.test(email);
				//提示信息
				if(!ok){
					alert("输入的邮箱不合法");
				}
			};
		}
	</script>
  </head>
  <body>
	  <input type="text" id="TBtn" />
	  <input type="button" value="按钮" id="SubBtn" />
  </body>
</html>
```
## 4：周期函数setInerval
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	
	<script type="text/javascript">
		//获取毫秒数(从1970年1月1日 00:00:00 000到当前的系统时间的总毫秒数)
		//注：获取系统时间不能写到onload内
		var t = new Date();
		document.write(t.getTime());
		function doTime(){
			//获取系统当前时间
			var nowTime = new Date();
			//将时间本地化(简化)
			var nowTime1 = nowTime.toLocaleString();
			//输出
			document.getElementById("Time").innerHTML = nowTime1;
		}
		
		window.onload = function(){
			
			document.getElementById("SubBtn").onclick = function (){
				//使用周期函数 v全局变量 方便清理
				v = window.setInterval("doTime()",1000);
			}
			
			document.getElementById("Btn").onclick = function (){
				//清理周期函数，停止时间
				window.clearInterval(v);
			}
		}
	</script>
  </head>
  <body>
	  <input type="button" value="按钮" id="SubBtn" />
	  <input type="button" value="停止" id="Btn" />
	  <div id="Time"></div>
  </body>
</html>
```
# BOM编程

> 浏览器的窗口，打开一个新浏览器的窗口，后退，前进，浏览器地址栏的url等
> BOM顶级对象是window
> BOM是包括DOM的
![在这里插入图片描述](https://img-blog.csdnimg.cn/7dabea7320ef4ea4bd1d5703f7a0d797.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 1：window.open()和window.close()
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		//open方法 开启窗口
	</script>
  </head>
  <body>
	  <!-- window.open实例 
	  参数open("地址","窗口");
	  窗口参数：_self(当前窗口) _blank(新窗口) _parent(父窗口) _top(顶级窗口)
	  -->
	  <input type="button" value="打开百度" onclick="window.open('http://www.baidu.com')" />
	  
	  <input type="button" value="关闭窗口" onclick="window.close()" />
  </body>
</html>
```
## 2：window.confirm()
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		function del(){
			//删除提示框
			window.confirm("确认删除吗");
		}
	</script>
  </head>
  <body>
	  <input type="button" value="确认删除" onclick="del()" />
  </body>
</html>
```
## 3：history和location
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
	</script>
  </head>
  <body>
	  <!--返回上一页是window.history.back()方法-->
	  <input type="button" value="返回上一页" onclick="window.history.back()" />
	  <!--第二种返回方法-->
	  <input type="button" value="返回上一页" onclick="window.history.go(-1)" />
	  <!--前进-->
	  <input type="button" value="返回上一页" onclick="window.history.go(1)" />
  </body>
</html>
```
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		//如果当前窗口不是顶级窗口,将当前窗口设置为顶级窗口，防止页面错乱
		if(window.top != window.self){
			//将当前窗口设置为顶级窗口
			window.top.location = window.self.location;
		}
	
		function Baidu(){
			//设置url地址window.location.href属性
			window.location.href = "http://www.baidu.com";
			//还可以写成window.location = 地址
			//document.location = 地址
		}
	</script>
  </head>
  <body>
	  <!-- 设置浏览器地址栏上的url 
	  -->
	  <input type="button" value="百度" onclick="Baidu()" />
  </body>
</html>
```
# Json
> json是一种数据交换格式 作用：进行数据交换(流行的格式)
> json是一种标准的轻量级的数据交换格式(json还可以嵌套json)
> 特点：体积小，易解析

**json的创建**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		//创建一个json
		var json = {
			"username":"张三",
			"password":"132456"
		};
		//访问json对象
		alert(json.username + "  " + json.password);
		//创建json数组
		var jsonArr = [{
			"username":"张三",
			"password":"132456"
		}];
		//for遍历json数组
		for(var i = 0;i < jsonArr.length;i++){
			alert(jsonArr[i].username);
		}
	</script>
  </head>
  <body>
	  
  </body>
</html>
```
**eval函数(将字符串当做js代码解释并执行)**
```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
	<script type="text/javascript">
		window.eval("var i = 100;");
		alert(i);
	</script>
  </head>
  <body>
	  
  </body>
</html>
```

