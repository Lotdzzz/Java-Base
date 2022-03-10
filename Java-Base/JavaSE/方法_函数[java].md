@[TOC](目录)
# 方法机制：
方法简单介绍：
```java
public class main {
    public static void main(String[] args) {
        doSome();//调用方法doSome
    }
    public static void doSome(){//main方法体是static静态方法所以调用的方法也要是静态的
        System.out.println("方法执行");
    }
}
    //方法语法：public 返回值 方法名(方法参数){代码块}
    //方法是不能重名的
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2676744176cd4019a668bcb1e09bd0c1.png)
方法只能写在类体里面，方法内的执行顺序是自上而下
方法如果不被调用是不会执行的（main方法除外）

**方法的局部变量：**

```java
public class main {
    public static void main(String[] args) {
        Sum(10,20);//调用方法Sum
    }
    public static void Sum(int x,int y){
        System.out.println(x+"+"+y+"="+(x+y));
    }
}
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b87708a84e5b4872bdd851841a2682ea.png)
**方法的参数写法：public 返回值 方法名(类型 参数名，类型 参数2名){代码块}**
参数在传参的时候会自动创建一个空间地址来存放这个参数的数据
在这个方法被调用结束后，参数的地址将会被释放掉，等下次调用的时候会重新开辟内存地址
不止是参数，方法内创建的变量也会被释放掉
## 方法的定义
**方法是一段可以完成某个特定功能的并且可以被重复利用的代码片段
方法的出现，让代码具有很强的复用性**
```java
语法：
[修饰符列表] 返回值类型 方法名 (形式参数列表){
	方法体;
	return 返回值;
}
-------------------------------------------------
[]内的内容是可选的，不是必须的
方法体是java语句构成
返回值类型可以是任何类型，数据类型包括[基本数据类型]和[引用数据类型]
return返回值的类型必须是方法的返回值类型
返回值类偶像是void类型这个方法是没有返回值的，但也可以写return;不带任务参数，return;之后结束方法
方法名不能相同（重载除外）
```
# 方法的jvm内存结构

```java
public class main {
    public static void main(String[] args) {
        Sum(10,20);
    }
    public static void Sum(int x,int y){
        System.out.println(x+"+"+y+"="+(x+y));
    }
}
```

JVM主要分为三块内存：栈内存，堆内存，方法区内存
![在这里插入图片描述](https://img-blog.csdnimg.cn/105f1b993afa4ecf8a88d41c80e2621f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**方法区（最先有数据）：存放代码片段，存放class字节码**
![在这里插入图片描述](https://img-blog.csdnimg.cn/b64fa2e61823486a971703d638df7e10.png)
**栈内存：方法“*调用*”的时候，该方法需要的内存空间分配在栈中，方法调用结束后，将释放此内存**
栈帧在创建的时候顺序是最先调用的方法在栈的最底部，依次进入栈，按调用顺序main方法最先执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/510b89a2b57745b2871ef61b7205e60e.png)
**代码执行完释放顺序根据栈先进后出的顺序，先释放Sum方法栈帧，再释放main方法栈帧**

# 方法的重载
方法重载：

> 功能相似的，可以让方法名相同，更易于代码的编写 在方法名相同时，它的参数列表必须不相同，不然是语法错误 
> java编译器会通过方法名进行区分
> 但是在java中允许方法名相同的情况出现 
> 如果方法名相同的情况下，编译器会通过方法的参数类型进行方法的区分

**例子：**
假如要用一个方法来实现整数，浮点数，长整型的加法运算可以使用方法重载：
```java
public class main {
    public static void main(String[] args) {
        System.out.println(Sum(10,20));
        System.out.println(Sum(3.5,3.5));
        System.out.println(Sum(10L,10L));
    }

    public static int Sum(int x,int y){
        return x+y;
    }

    public static double Sum(double x,double y){
        return x+y;
    }

    public static long Sum(long x,long y){
        return x+y;
    }

}
```
**方法重载的条件：**

> 1：在同一个类中
> 2：方法名相同
> 3：参数列表不同{参数的个数不同，参数的类型不同，参数的顺序不同}
**满足以上三个条件，就会被认为是方法重载**
# 方法的递归
方法递归：人用循环，神用递归
>**方法自己调用方法自己，就是方法递归**

```java
public class main {
    public static void main(String[] args) {
        doSome();
    }

    public static void doSome(){
        System.out.println("doSome begin");
        doSome();
        System.out.println("doSome over");//永远轮不到这段代码执行
    }
}
方法里自己调用自己，一直执行doSome();会形成死递归，一般的递归要设置结束条件，和循环差不多
```
代码执行：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6df2b60cf60b4b75ba6d942275f5ca04.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
结果：StackOverflowError（栈溢出错误）
![在这里插入图片描述](https://img-blog.csdnimg.cn/df049b969466445aa51bb843b5a25578.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**递归例子：**
**使用递归计算1到n的和：**

```java
public class main {
    public static void main(String[] args) {
        System.out.println(Sum(3));//结果是6
    }
    public static int Sum(int n){
        if(n == 1){//判断条件同时也是结束条件
            return 1;
        }
        //递归执行：3 + 2 + 1
        return n + Sum(n-1);
    }
}
```
代码执行顺序理解：
![在这里插入图片描述](https://img-blog.csdnimg.cn/4a423a24b97645c1b897fef2c6a4f8f0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

## 递归的内存图分析：
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd544f8ed968428fbe13eef1aa4c438f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)


