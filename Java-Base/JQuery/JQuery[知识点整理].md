

<font color=#999AAA >

</font>

@[TOC](文章目录)


<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

## JQuery的认识
**JQuery是一款JavaScript的库(工具类)，对JavaScript的方法进行封装的，优化JavaScript对HTML DOM
简单来说就是JavaScript的代码更方便**
**JQuery的下载地址**：[JQuery的官网](https://jquery.com/)
JQuery的常用方式

```java
js：document.getElementById()
JQuery：$("#id")


js：document.getElementByClassName()
JQuery：$(".class")


js：document.getElementByTagName()
JQuery：$("标签名")
```

## DOM对象
通过DOM对象，可以将页面元素解析为元素结点，属性结点，文本结点
这些解析出的结点对象，就是dom对象，dom对象可以使用JavaScript中的方法
比如document.getElementById()等

## JQuery使用方式

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
    /*
    $(document)，$是函数的函数名，$()是一个函数，内的document是这个页面加载完之后生成的document对象(DOM对象)
    作用是document对象变成Jquery函数库可以使用的对象
    
    ready是jquery中的函数，是当页面加载完成生成dom对象后会执行ready中的内容，相当于js中的onLoad事件
    
    funciton()自定义的表示当页面加载完之后ready要执行的函数
    */
        $(document).ready(function (){
            alert("hello jquery");
        })
        $(function(){
        	alert("hello jquery");
        })
        //以上两种写法作用是一样的
        //第二种写法是简写
        //$(document).ready()与$()，JQuery()，window.JQuery()都是一样的作用最常用是$()
    </script>
</head>
<body>

</body>
</html>
```

## DOM对象与JQuery对象
```java
/*
dom对象，使用JavaScript的语法创建的对象叫做dom对象，也就是js对象
*/
var obj = document.getElementById();
//这里的obj就是一个dom对象
/*
用Jquery语法创建的对象叫做jquery，所有的jquery对象都是数组。
*/
var jqobj = $("#txt1");
//这里的jquery对象jqobj是一个数组，这里数组只有一个值txt1
//jquery对象和dom对象是可以相互转换的
//dom转换jquery语法是:
$(dom对象);
//jquery转换为dom对象：
var jqobj = $("#txt1");
jqobj[0]
//这里的jqobj[0]就是一个dom对象，也就是jquery数组中的每个元素都是dom对象
```

## 基本选择器使用
选择器就是定位条件，是一个字符串，用来定位jquery中的dom对象
选择器常见的是：

```java
ID选择器：$("#dom对象的ID值")；ID是唯一值，不会重复的
class选择器：$(".class样式名")；样式名来定位dom对象
标签选择器：$("标签名")；用标签名来定位dom对象
```
**选择器的例子：**

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <style type="text/css">
        /*div标签设置*/
        div{
            background: aqua;
            width: 150px;
            height: 100px;
        }
    </style>
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        function funOne(){
            /*
                id选择器
                返回jquery对象obj
                调用jquery中的函数css()改变dom对象的颜色
             */
            var obj = $("#one");
            obj.css("background","red");
        }
        function funTwo(){
            /*
            .class样式选择器
             */
            var obj = $(".two");
            obj.css("background","green");
        }
        function funThree(){
            /*
            标签选择器，获取所有div的标签来改变所有div的颜色
             */
            var obj = $("div");
            obj.css("background","black");
        }
        function funFour(){
            /*
            改变所有dom对象的颜色
            */
            var obj = $("*");
            obj.css("background","yellow");
        }
        function funFive(){
            /*
            混合选择器，id，class样式，标签
             */
            var obj = $("#one,.two,span");
            obj.css("background","red");
        }
    </script>
</head>
<body>
    <div id="one">第一个div</div>
    <div class="two">第二个class样式的div</div>
    <div>标签div</div>
    <span>span标签</span>
    <br/>
    <input type="button" value="改变第一个div的颜色" onclick="funOne()"/>
    <br/>
    <input type="button" value="改变class样式的颜色" onclick="funTwo()"/>
    <br/>
    <input type="button" value="改变div标签的颜色" onclick="funThree()"/>
    <br/>
    <input type="button" value="改变所有的颜色" onclick="funFour()"/>
    <br/>
    <input type="button" value="组合选择器" onclick="funFive()"/>
</body>
</html>
```
## 表单选择器
表单选择器就是对<input type="text">中的type属性进行管理并使用的
语法：

```java
$(":type属性值");
$(":text");
$(":radio");
$(":checkbox");
//注：$(":tr")这种tr标签是没有type属性值的所以是错误的写法
```
表单管理器：

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        function fun1(){
            var obj = $(":text");
            //val()是jquery的函数作用是jquery对象的值
            alert(obj.val());
        }
        function fun2(){
            /*
            因为radio是个数组所以可以for循环遍历
            而这是一个jquery数组内的每个元素都是dom对象
            所以要用dom对象js的方法而不是jquery的方法
             */
            var obj = $(":radio");
            for (var i = 0; i < obj.length; i++) {
                alert(obj[i].value);
            }
        }
        function fun3(){
            /*
            如果要用val()来显示数据必须要是jquery对象
            这里转换成jquery对象来使用val()函数
            将obj[i]每个dom对象元素转成jquery是$(obj[i])
            然后调用它的val()函数
             */
            var obj = $(":checkbox");
            for (let i = 0; i < obj.length; i++) {
                alert($(obj[i]).val());
            }
        }
    </script>
</head>
<body>
<input type="text" value="第一个text"/><br/>
<input type="radio" value="man"/>男
<input type="radio" value="woman"/>女<br/>
<input type="checkbox" value="football"/>足球<br/>
<input type="checkbox" value="music"/>音乐<br/>
<input type="button" value="读取text的值" onclick="fun1()"/><br/>
<input type="button" value="读取radio数组的值" onclick="fun2()"/><br/>
<input type="button" value="读取checkbox数组的值" onclick="fun3()"/><br/>
</body>
</html>
```

## 定义元素监听事件

```java
//语法：
//$(选择器).监听事件名称(处理函数);
//作用等同于js中的onclick
//为页面中的button绑定onclick事件并关联处理函数
$("button").click(fun1);
//为页面中的所有tr标签绑定onmouseover事件的写法
$("tr").mouseover(fun2);
要在页面加载完document对象后给按钮或者标签绑定函数
$(function(){
	$("#btn1").click(function(){
		alert("按钮单击事件");
	})
})
```

## 过滤器
过滤器就是过滤条件，对数组中的dom对象进行条件筛选，过滤条件不能独立出现在jquery函数中华，只能出现在选择器后方

```java
<div>1</div>//dom1
<div>2</div>//dom2
<div>3</div>//dom3
$("div");//这个数组里面就会有三个dom对象，过滤器的作用就是对这些对象进行筛选
```
**基本过滤器**
过滤器不能单独用，必须和选择器使用。
过滤器是一个字符串，来筛选dom对象的

```java
//选择第一个元素first
$("选择器:first");
//选择最后一个元素last
$("选择器:last");
//选择数组中指定的对象
$("选择器:eq(数组索引)");
//选择数组中小于指定索引的所有dom对象
$("选择器:lt(数组索引)");
//选择数组中大于指定索引的所有dom对象
$("选择器:gt(数组索引)");
```
过滤器：

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            //在页面加载完成生成document之后执行以下绑定事件
            /*
            给btn1绑定单击事件.click
            函数内容是获取所有div的第一个div并改变这个div的颜色为红色
             */
            $("#btn1").click(function (){
                $("div:first").css("background","red");
            })
            /*
            获取最后一个div，写法与上一个绑定事件一样只需要将first改成last
             */
            $("#btn2").click(function (){
                $("div:last").css("background","green");
            })
            //获取等于3的div
            $("#btn3").click(function (){
                $("div:eq(3)").css("background","black");
            })
            **//获取小于3的
            //这里会把四和五，六都改变
            //因为第三个div就是下标为2的div包括了子标签四和五，六**
            $("#btn4").click(function (){
                var obj = $("div:lt(3)");
                obj.css("background","orange");
            })
            //**获取大于3的大于三的是不看子标签的**
            $("#btn5").click(function (){
                var obj = $("div:gt(3)");
                obj.css("background","blue");
            })
        })
    </script>
</head>
<body>
<div>第一个div</div>
<div>第二个div</div>
<div>第三个div
    <div>第四个div</div>
    <div>第五个div</div>
    <div>第六个div</div>
</div>
<div>第七个div</div>
<span>span标签</span>
<p>功能按钮</p>
<input type="button" id="btn1" value="选择第一个div"/><br/>
<input type="button" id="btn2" value="选择最后一个div"/><br/>
<input type="button" id="btn3" value="选择索引等于3的div"/><br/>
<input type="button" id="btn4" value="选择索引小于3的div"/><br/>
<input type="button" id="btn5" value="选择索引大于3的div"/><br/>
</body>
</html>
```
## 表单属性过滤器
表单属性过滤器：根据表单中dom对象的状态情况，定位dom对象的
```java
启动状态：enabled
//选择可用的文本框
$(":text:enabled");
不可用状态：disabled
//选择不可用的文本框
$(":text:disabled");
复选框：checked
//选择所有的复选框
$("checkbox:checked");
----------------------
//被指定下拉列表的被选中元素
选择器>option:selected
```
表单属性过滤器：

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn1").click(function (){
                /*
                使用表单属性过滤器
                $(:选择器:过滤条件);
                 */
                $(":text:enabled").val("hello");
            })
            $("#btn2").click(function (){
                /*
                显示所有复选框的值是一个数组
                要写循环
                 */
                var obj = $(":checkbox:checked");
                for (let i = 0; i < obj.length; i++) {
                    window.alert(obj[i].value);
                }
            })
            $("#btn3").click(function (){
                /*
                选择下拉列表的语法
                $(选择器>子标签:过滤条件);
                这也是个数组
                 */
                window.alert($("select>option:selected").val());
                //也可以写成
                window.alert($("#lang>option:selected").val());
            })
        })
    </script>
</head>
<body>
    <input type="text" value="text1" disabled/><br/>
    <input type="text" value="text2" /><br/>
    <input type="text" value="text3" /><br/>
    <input type="text" value="text4" disabled/><br/>
    <input type="checkbox" value="LOL" checked/>LOL
    <input type="checkbox" value="CF" />CF
    <input type="checkbox" value="WOW" checked/>WOW
    <br/>
    <select id="lang">
        <option value="c">C语言</option>
        <option value="java">java语言</option>
        <option value="c++">C++语言</option>
    </select><br/>
    <input type="button" value="设置可用的text文本框的value是hello" id="btn1"/><br/>
    <input type="button" value="显示选中复选框的值" id="btn2"/><br/>
    <input type="button" value="显示下拉列表框的值" id="btn3"/><br/>
</body>
</html>
```

## JQuery函数
**第一组函数**
1：val
操作数组中的dom对象的value属性
语法：

```java
$("选择器").val();//无参数形式,读取数组中华的第一个dom对象的value属性值
$("选择器").val(值);//对数组中的所有dom对象的value属性值统一赋值
--------------------例子：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn1").click(function (){
            //输入值之后会改变type为text的所有dom对象的value值
                $(":text").val("hello");
            })
        })
    </script>
</head>
<body>
<input type="button" value="按钮" id="btn1"/>
<input type="text" value="txt1"/>
</body>
</html>
```
2：text
操作数组中的所有dom对象的innerText[文字显示内容属性]
语法：

```java
$("选择器").text();//无参形式，读取数组中所有dom对象的文字显示内容，将得到内容拼接为一个字符串
$("选择器").text(值);//对数组中所有dom对象的文字显示内容进行统一的赋值
--------------------例子：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn1").click(function (){
                //会改变掉下面[一二三个平平无奇的div]为[消失的div]
                $("div").text("消失的div");
                window.alert($("div").text());//返回的是拼接后的字符串
                /*
                结果为：消失的div消失的div消失的div
                 */
            })
        })
    </script>
</head>
<body>
<input type="button" value="按钮" id="btn1"/>
<div>一个平平无奇的div</div>
<div>二个平平无奇的div</div>
<div>三个平平无奇的div</div>
</body>
</html>
```
3:：attr
对val，text之外的其他属性操作
语法：

```java
$("选择器").attr("属性名");//获取dom数组中的第一个对象的属性值
$("选择器").attr("属性名","值");//对数组中所有dom对象的属性设为新值
-----------------例子：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn1").click(function (){
                //对jquery对象id为text获取他的type属性的值
                //值为reset
                window.alert($("#text").attr("type"));
                //将他的type属性值reset改为radio
                $("#text").attr("type","radio");
            })
        })
    </script>
</head>
<body>
<input type="button" value="按钮" id="btn1"/>
<input type="reset" id="text"/>
</body>
</html>
```
**第二组函数**
1：remove

```java
语法：
$("选择器").remove();//将数组中所有的dom对象及其子对象一并删除
```
2：empty

```java
语法：
$("选择器").empty();//将数组中所有dom对象的子对象删除
```
3：append
为数组中的所有dom对象添加子对象

```java
$("选择器").append("<div>动态添加的div</div>");
-------------------例子：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn1").click(function (){
                //为每一个div添加一个子对象为：动态添加的div
                $("div").append("<div>动态添加的div</div>");
            })
        })
    </script>
</head>
<body>
<div>第一个div</div>
<div>第二个div</div>
<input type="button" value="添加子对象" id="btn1"/>
</body>
</html>
```
4：html
设置或者返回被选元素的内容(innerHTML)
语法：

```java
$("选择器").html();//无参方法，获取dom数组第一匹元素的内容
$("选择器").html(值);//设置dom数组中所有元素的内容
-------------------例子：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn1").click(function (){
                window.alert($("div").html());//这里获取的是第一个文本值[第一个div]
                /*
                可以循环遍历这个所有div的html()的值
                注意要把所有的div元素都转成jquery对象才可以调用html()函数
                 */
                var obj = $("div");
                for (let i = 0; i < obj.length; i++) {
                    window.alert($(obj[i]).html());
                }
                //有参调用改变所有dom对象的值
                obj.html("新的html值");
            })
        })
    </script>
</head>
<body>
<div>第一个div</div>
<div>第二个div</div>
<input type="button" value="获取dom对象的文本值" id="btn1"/>
</body>
</html>
```
5：each
each是对数组json和dom数组的value的遍历，对每个元素调用一次函数
语法：

```java
$.each(function(index,element){
})
jquery对象.each(function(index,element){
})
index是数组的下标
element是数组的对象
两种写法作用相同
------------------例子：
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn1").click(function (){
                var json = {"name":"张三","age":20};//数组的循环
                $.each(json,function (index,element){
                    window.alert("属性"+index+"值"+element);
                    /*
                    结果为
                    属性name值张三
                    属性age值20
                     */
                })
                //第二种语法格式
                //jQuery对象.each() jQuery对象就是dom数组
                $("div").each(function (index,element){
                    window.alert("下标"+index+"数据"+element.innerText);
                    /*
                    结果为：
                    下标0数据第一个div
                    下标1数据第二个div
                     */
                })
            })
        })
    </script>
</head>
```

## 事件
on()绑定事件
on()方法在被选元素上添加事件处理程序，比较常用
语法：

```java
$("选择器").on(event,function);
event：一个事件或多个事件，多个之间空格分开
function：可选，当时间发生后运行的函数
-----------------------例子：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript " src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            //根$("#btn1").click()一样但是下面的方法更灵活
            $("#btn1").on("click",function (){
                window.alert("按钮被点击");
            })
        })
    </script>
</head>
<body>
<input type="button" value="按钮" id="btn1"/>
</body>
</html>
```

## AJAX
jquery中提供了新的ajax使用方式不同以前的ajax四步
没有jquery之前使用的是异步对象(比较繁琐)
jquery把ajax的处理简化成了函数：
**$.ajax()**
jquery中实现ajax核心

```java
$.ajax()使用方法：
函数的参数表示用来表示请求方式，url，参数值等。
$.ajax()函数参数是一个json的结构
函数参数：
=================================================================
async：布尔值，表示请求是否异步处理，默认是true
contentType：发送数据到服务器时所用的内容类型，列如application/json
data：规定要发送到服务器的数据，可以是字符串，数组，json
dataType：期望从服务器响应的数据类型，可选有：xml，html，text，json
发送请求时，会把dataType的值发给服务器，servlet能够读到dataType的值
就知道你的浏览器需要的是什么类型的数据，返回什么类型不一定！
error();如果请求失败要运行的函数
success(resp);当请求成功时运行的函数
type：规定请求的类型(get,post)默认是get
url：规定发送请求的URL
=================================================================
语法：
$.ajax({
		async:true,
		contentType:"application/json",
		data:{name:"张三",age:"20"},
		dataType:"json",
		error(function(){
			请求发生错误时，实行的函数
		}),
		success:function(resp){
			返回的数据进行处理
		},
		url:"请求地址",
		type:"get"
})
```

```java
$.post();//使用post方式做ajax请求
$.get();//使用get方式发送ajax请求
以上两个函数内部都是调用$.ajax()函数
```
例子：

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--找到JQuery文件-->
    <script type="text/javascript" src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn").on("click",function (){
                //获取id的value值要查找的省份id
                var id = $("#id").val();
                //发送ajax请求
                //其中有一些参数是可以省略的因为有默认值
                $.ajax({
                    data:{//传入的值json形式
                        "id":id
                    },
                    dataType:"json",//希望返回的数据类型json
                    success:function (resp){//处理从服务器返回的数据
                        $("#name").val(resp.proName);//给省份名称标签设置value值
                        $("#simple").val(resp.simple);//给省份简称设置value值
                        $("#province").val(resp.province);//给省会设置value值
                    },
                    url:"search",//访问的地址在web.xml的简称search
                })
            })
        })
    </script>
</head>
<body>
<p>根据省份id获取名城</p>
<table>
    <tr>
        <td>省份编号:</td>
        <td>
            <input type="text" id="id"/>
            <input type="button" id="btn" value="搜索"/>
        </td>
    </tr>
    <tr>
        <td>省份名称:</td>
        <td>
            <input type="text" id="name"/>
        </td>
    </tr>
    <tr>
        <td>省份简称:</td>
        <td>
            <input type="text" id="simple"/>
        </td>
    </tr>
    <tr>
        <td>省份省会:</td>
        <td>
            <input type="text" id="province"/>
        </td>
    </tr>
</table>
</body>
</html>
```

## 级联查询小项目
选择省份名称，城市就会显示该省份下的城市
服务器：tomcat
json工具包：Jackson
数据库：mysql
jquery库：jquery3.6.0.js
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b4b850ca8c1484db7e2f2d488bc2348.png)
项目目录：
![在这里插入图片描述](https://img-blog.csdnimg.cn/837f439d66914c068f1dd0b1b6cce381.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
设置基础的页面：

```java
<body>
<p>省市级联查询</p>
<div>
    <table border="2">
        <tr>
            <td>省份名称:</td>
            <td>
                <select id="province">
                    <option value="0">请选择省份</option>
                </select>
            </td>
        </tr>
        <tr>
            <td>城市:</td>
            <td>
                <select id="city">
                    <option value="0">请选择城市</option>
                </select>
            </td>
        </tr>
    </table>
</div>
</body>
```
封装dao层：
先把第一个省份名称下拉列表的九个省份从mysql数据库获取利用append方式追加到网页的下拉列表

```java
public class levelDao {
    JdbcUtil jdbcUtil = new JdbcUtil();

    public List<Province> SearchName(){
        String sql = "select * from province";
        List<Province> provinceList = new ArrayList<>();
        //因为省份有很多所以封装成很多对象装到list集合内
        ResultSet rs = null;
        PreparedStatement ps = jdbcUtil.JdbcGetPreparedStatement(sql);
        try {
            rs = ps.executeQuery();
            while(rs.next()){
                Integer id = rs.getInt("id");
                String name = rs.getString("name");
                String simple = rs.getString("jiancheng");
                String province = rs.getString("shenghui");
                Province provinceNode = new Province(id,name,simple,province);
                provinceList.add(provinceNode);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            if(rs!=null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            jdbcUtil.JdbcDeleteUtil();
        }
        return provinceList;
    }
}
```
获取省份的Servlet：

```java
public class SearchCityName extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String json = "{}";//初始化为{},防止空数据
        levelDao levelDao = new levelDao();
        List<Province> Pro = levelDao.SearchName();
        if(Pro!=null){//判断是否为空数据
        	//调用json工具包的ObjectMapper类
            ObjectMapper om = new ObjectMapper();
            json = om.writeValueAsString(Pro);
        }
        resp.setContentType("application/json;charset=utf-8");
        PrintWriter pw = resp.getWriter();
        pw.println(json);
        pw.flush();
        pw.close();
    }
}

```

第一次利用jquery的ajax发送请求获取省份添加到下拉列表框：

```java
//独自封装成函数，需要等待页面的document对象加载完才可以执行这个函数需要$(function(){})
function doAjax(){
            $.ajax({
                data:{},
                dataType:"json",
                success:function (resp){
                    $.each(resp,function (index,element){
                        $("#province").append("<option value='"+element.id+"'>"+element.name+"</option>");
                    })
                },
                url:"search"
            })
        }
```
dao层的根据发送的省份id来查询该省份下的城市：

```java
public List<city> LevelCity(String id){
        String sql = "select id,name from city where provinceid = ?";
        ResultSet rs = null;
        PreparedStatement ps = jdbcUtil.JdbcGetPreparedStatement(sql);
        List<city> cityList = new ArrayList<>();
        try {
            ps.setInt(1,Integer.valueOf(id));
            rs = ps.executeQuery();
            while (rs.next()){
                Integer CityId = rs.getInt("id");
                String name = rs.getString("name");
                city city = new city(CityId,name,Integer.valueOf(id));
                cityList.add(city);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return cityList;
    }
```
根据省份查询城市的Servlet：

```java
public class LevelServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String json = "{}";
        resp.setContentType("application/json;charset=utf-8");
        String id = req.getParameter("id");
        if(id!=null && !"".equals(id.trim())){
            levelDao levelDao = new levelDao();
            List<city> cityList = levelDao.LevelCity(id);
            if(cityList!=null){
                ObjectMapper om = new ObjectMapper();
                json = om.writeValueAsString(cityList);
            }
        }
        PrintWriter pw = resp.getWriter();
        pw.println(json);
        pw.flush();
        pw.close();
    }
}
```

利用jquery的change改变事件来获取被选中的下拉列表框的元素：

```java
$(function (){
            doAjax();
            $("#province").change(function (){
                var id = $("select>option:selected").val();
                $.ajax({
                    data: {"id":id},
                    dataType:"json",
                    success:function (resp){
                        $("#city").empty();
                        $.each(resp,function (index,element){
                            $("#city").append("<option value='"+element.id+"'>"+element.name+"</option>")
                        })
                    },
                    url:"level"
                })
            })
        })
```

