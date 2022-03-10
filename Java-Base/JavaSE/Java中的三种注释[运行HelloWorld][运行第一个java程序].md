# 注释的作用

java中的注释只是java对代码的解释 注释是不会被java运行的 只是一行文字而已 也不会被编译到class文件当中

写注释是用来解释代码的作用 就像新买了鼠标键盘中用来解释鼠标键盘怎么使用的说明书一样

java中的注释分为三种：

第一种 单行注释 只能写一行信息： //

第二种 多行注释 能写很多行的注释：/*    */

第三种 javadoc注释 能被javadoc命令生成文档：/******/

![img](https://img-blog.csdnimg.cn/47322def88eb4ade9feb9536fb79ed6e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

![img](https://img-blog.csdnimg.cn/13b5858637594004b7e288f82243d87f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

HelloWorld
注：所有代码执行顺序都是自上而下，一行代码写完都需要以分号结尾；

创建一个文件夹   位置随意 名称随意

并且创建一个名称为HelloWorld后缀名为.java的文件：HelloWorld.java

用notepad++文本编辑器打开这个文件

notepad++下载地址：点击此链接

打开后复制这段信息

public class HelloWorld {
	public static void main(String[] args){
		System.out.println("Hello World");
	}
}
然后Win+R打开输入cmd打开命令窗口

输入javac 文件路径编译

比如文件在D盘的test文件夹中

![img](https://img-blog.csdnimg.cn/7f7e77414c5142d58bc7e2bdac908213.png)

就需要输入javac D:\JavaTest\HelloWorld.java

完成后没有错误会显示一下内容并在文件夹内生存一个HelloWorld.class文件

![img](https://img-blog.csdnimg.cn/7b10a6e6395643e0a3a3de7f17807d87.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_19,color_FFFFFF,t_70,g_se,x_16)

然后运行程序找到文件点击地址栏输入cmd

![img](https://img-blog.csdnimg.cn/2885f330c1f94a6f9b0c98c04879b06f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

 窗口内容会显示

![img](https://img-blog.csdnimg.cn/33b9baa5406646c2a9b0bef216d71b73.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_15,color_FFFFFF,t_70,g_se,x_16)

然后运行程序输入java HelloWorld

![img](https://img-blog.csdnimg.cn/648cea38c3824e45b28af76bbf8f63ee.png)

这样会在屏幕内显示Hello World

当然也可以写成别的内容比如

![img](https://img-blog.csdnimg.cn/8af35c1f09a5477aba2d431abd88f2fc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

如果是中文则需要输入chcp 65001

![img](https://img-blog.csdnimg.cn/1ca4d5de274040bb8dfa482963144747.png)

在编译输入javac HelloWorld.java        运行java HelloWorld

![img](https://img-blog.csdnimg.cn/edd2d7249a4f45c88c2c7c633546807f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)

可以看到注释在程序内不会被执行显示出来

到这里第一个java程序运行完成