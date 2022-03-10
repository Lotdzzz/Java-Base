## 全局刷新与局部刷新

> **全局刷新：整个浏览器被新的数据覆盖，在网络中传输大量的数据，浏览器需要被重新加载。**
> **局部刷新：在浏览器的内部发起请求，发起请求，获取数据，改变页面中的内容，无需重新加载， 网络传输的数据少**。

**全局刷新的原理：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/a2094c3f970d4da29f32ad17ab09f720.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/083c1d1f934f4189b7dedccde8af6d35.png)
**局部刷新的原理：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/016d3301c1154445bb24df041d9419a6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
用异步对象代替浏览器的行为来发请求并且获取数据到浏览器，可以有多个异步对象存在。

## Ajax实现局部刷新：
**Ajax创建异步对象：var xmlHttp = new XMLHttpRequest();**
*XMLHttpRequest对象：
在不重新加载页面的情况下更新网页
在页面已加载后向服务器请求数据
在页面已加载后向服务器接收数据*

## Ajax的语法分为四步
Ajax使用javaScript语法创建
Ajax的技术主要有JavaScript和xml
JavaScript：负责创建异步对象发起请求，更新页面的dom对象
xml：网络中的传输的数据格式使用json替换了xml
**1：创建异步对象**
var xmlHttp = new XMLHttpRequest()；
**2：onreadstatechange事件**
用来处理dom对象绑定事件，获得请求的状态变化，当异步对象发起请求，获取了数据就会触发这个事件，这个事件需要绑定一个函数，在函数中处理数据
列如：

```java
xmlHttp.onreadstatechange = function(){	
	if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
		处理请求(必须等到数据解析完成并且网络状况200才可以处理数据)；
	}
}；
```

状态变化的属性：readyState属性
存异步对象的状态，有0到4种状态
	0：表示创建异步对象，new XMLHttpRequest()；
	1：初始化异步请求对象，xmlHttp.open()；
	2：异步对象发送请求，xmlHttp.rend()；
	3：表示异步对象从服务器端返回的数据；
	4：表示异步对象已经解析完成数据，这时才可以读取数据(局部刷新，展示数据)；
异步对象的status属性，表示网络请求的状况：比如200，404，500等；
**3：初始异步请求对象**
xmlHttp.open(请求方式get或者post，“服务器端的访问地址”，异步true同步false)；
比如访问loginServlet
xmlHttp.open("get","loginServlet?name=zs&psd=123",true)；
**4：使用异步对象发送请求**
xmlHttp.send()；
**处理服务器端返回的数据：**
服务器端返回的数据属性是异步对象的responseText
xmlHttp.responseText

```java
xmlHttp.onreadstatechange = function(){	
	if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
		var data = xmlHttp.responseText;
		document.getElementById("text").innerText = data;
	}
}；
```
***回调：当请求的状态变化时，异步对象会自动调用onreadystatechange事件对应的函数***
***比如执行0到4种状态时每执行一种状态都会调用onreadystatechange对应的函数***

## 异步和同步
为xmlHttp.open()的第三个参数true为异步或false同步
异步：
使用异步对象发起请求后，不用等待数据处理完毕就可以执行其他的操作
等执行完send之后数据返回来本次请求完成，异步的意思是不用等待send返回还可以进行js里面的其他操作。
同步：
必须等待send请求完成之后才可以继续进行js下面的代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/85b6066df4954eb39fc462990496b72c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

## Ajax的例子：
**计算加法的例子用html网页实现并实现局部刷新**
index.html：
```java
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>局部刷新</title>
  <script type="text/javascript">
    function ResultSum(){
      //准备要发送的数据
      var num1 = document.getElementById("num1").value;
      var num2 = document.getElementById("num2").value;
      //使用内存中的异步对象，代替浏览器发起请求，异步对象试用JavaScript创建和管理
      //1:创建异步对象
      var xmlHttp = new XMLHttpRequest();
      //2:绑定事件
      xmlHttp.onreadystatechange = function (){
        //处理服务器端返回的数据条件是处理状态要到4并且网络状态是200
        if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
          document.getElementById("div").innerText = xmlHttp.responseText;
        }
      }
      //3:初始请求数据
      xmlHttp.open("get","sum?num1="+num1+"&num2="+num2+"",true);
      //4:发起请求
      xmlHttp.send();
    }
  </script>
</head>
<body>
<p>计算sum</p>
  <div>
    数据1<input type="text" id="num1"><br/>
    数据2<input type="text" id="num2"><br/>
    <input type="button" value="确认" onclick="ResultSum()"/>
    <br/>
    <br/>
    <div id="div">等待结果刷新....</div>
    </form>
  </div>
</body>
</html>
```
处理结果的SumServlet：

```java
package com.ajax.controller;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class SumServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=utf-8");
        String num1 = req.getParameter("num1");
        String num2 = req.getParameter("num2");
        PrintWriter pw = resp.getWriter();
        if("".equals(num1.trim()) || "".equals(num2.trim())){ //判断传入的数据是否有问题
            pw.println("无数据");//此时就可以用Ajax的状态4解析当前数据并处理
            pw.flush();
            pw.close();
            return;
        }
        //计算结果
        int result = Integer.valueOf(num1) + Integer.valueOf(num2);
        String msg = "结果为："+result;
        //输出结果
        pw.println(msg);//此时就可以用Ajax的状态4解析当前数据并处理
        pw.flush();
        pw.close();
    }
}
```
页面展示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6018e09a1c146c59fde6820eb290ba7.png)
输入结果点击确认后：
![在这里插入图片描述](https://img-blog.csdnimg.cn/7c97b4d9ff554f52a1afeb747ee45e23.png)
为了防止空数据：
![在这里插入图片描述](https://img-blog.csdnimg.cn/80b9002843db40b6b14b48d3f94dc595.png)

