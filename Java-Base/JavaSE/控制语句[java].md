@[TOC](目录)

# 接收用户输入的数据：
接收用户输入需要用到java.util.Scanner导包
然后创建Scanner对象：Scanner sc = new Scanner(System.in);
接收输入：

```java
import java.util.Scanner;

public class main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String massage = sc.next();
        System.out.println(massage);
    }
}
```

```java
接收一行内容：
sc.next();
接收整数类型：
sc.nextInt();
接收字符：
```

# 控制语句：if，switch
语法机制（写法）：
## if
```java
第一种：
if(布尔表达式){

}
/*
if语句的作用是：如果if语句的布尔表达式为true则执行语句内的内容，如果为false则跳过这个if语句执行下面的代码
注：if语句只能写布尔表达式
*/
比如：
int num = 10;
if(num == 10){
	Systyem.out.println("数字是"+num);
}
这个语句判断num是否为10如果为10则执行if语句的内容输出数字是10
if(num == 100){
	Systyem.out.println("数字是"+num);
}
判断num是否为100，结果为false，则整个if语句不会执行
```
```java
第二种：
if(布尔表达式){

}else{

}
作用是：第一个if语句执行结果如果为true则else不会执行，如果if结果为false则执行else语句内容
```
```java
第三种：
if(布尔表达式){

}else if(布尔表达式){

}else if(布尔表达式){

}
作用：如果第一个if语句的结果为false则判断else if的结果，如果这个结果还是false则判断下一个else if的结果，如果全都是false则全部的语句都不会执行，如果第一个if为true则只执行第一个if，往下的else if则不会执行，如果第一个if为false，第二个第三个else if为true，则只执行第二个else if，其他的不会执行
注：只能有一个语句被执行
```
```java
第四种：
if(布尔表达式){

}else if(布尔表达式){

}else if(布尔表达式){

}else{

}
加了else的作用：如果以上三个语句都是false则执行else，因为else是除了以上三种情况外的其他情况
加了else的控制语句是必会执行一条语句
else意思就是剩下的情况会怎么样
```
```java
if控制语句嵌套写法：
if(布尔表达式){
	if(布尔表达式){

	}
}
嵌套循环执行：先判断外面的第一个if语句如果为true则进入语句块判断内容的if是否为true或者false
注：只有外面的if为true才会进入里面判断if
```
只要有一个if语句执行，则结束整个控制语句
当布尔表达式的结果为true的时候，分支语句才会执行
## switch
语法机制：

```java
switch(值){//值允许是String,int（byte short char会自动转换为int）
	case 值1:
		内容
		break;
	case 值2:
		内容
		break;
	case 值3:
		内容
		break;
	case 值4:
		内容
		break;
}
switch的值会往下依次判断case的值是否等于switch括号里的值，如果相等则执行case内的语句
注：如果一个case不加break则会继续往下判断case
加break的意思是跳出整个switch语句
```
# 循环语句：for，while，do while
## for

执行原理：

```java
for(初始化表达式，条件表达式，更新表达式){
	循环体;
}
写法：
for(int i = 0;i < 10;i++){
	System.out.println(i);
}
顺序：初始化i为0，条件是i小于10，如果条件为真则执行循环体输出i，执行完循环体然后更新表达式：i++，更新之后继续判断条件表达式，如果还是为true则继续执行循环体
```
```java
嵌套for循环：
for(初始化表达式，条件表达式，更新表达式){
	for(初始化表达式，条件表达式，更新表达式){
	
	}
}
写法：
for(int i = 0;i < 10;i++){
	for(int j = 0;j < 10;j++){
		System.out.println(i+j);
	}
}
嵌套for循环的顺序是：
先判断外面的for循环条件表达式是否为true，为true则进入循环体，此时的循环体就是一个for循环那么，继续判断里面for循环的条件表达式，为true则执行里面的for循环，直到里面的for循环一直循环完毕才继续执行外面for循环的更新，然后在进行外面for循环的条件判断
```
1：先执行初始化表达式，并且只执行一次
2：判断条件表达式
3：如果为true，则执行循环体
4：循环体结束之后，执行更新表达式
5：继续判断条件，如果条件为true，继续循环
6：直到条件为false，循环结束

## while
**while循环结构：**

```java
while(条件表达式){
	循环体
}
只要表达式为true，循环则会一直进行不会停止
写法：
while(10<100){
	System.out.pirntln("循环");
}
顺序：判断条件表达式，为true则执行循环体，执行完循环体，在进行条件表达式判断
这段代码的条件为true，条件不会迭代永远为true，则为死循环一直循环

while循环的迭代处理：
int index = 0;
//设置索引来进行循环迭代
while(index < 10){
	index ++;
	//更新条件
}
这样循环一次则会执行index++，一直到index大于10条件为false则会停止循环

同样while循环也可以嵌套：
while(条件表达式){
	while(条件表达式){
	}
}
```
## do while
do while循环和while循环的区别是：
while循环如果条件不满足则不会执行循环
而do while循环是先执行循环再进行判断，这样会导致do while循环必须执行一次

```java
do while循环语法：
do{
	循环体
}while(条件表达式)

写法：
do{
	System.out.println("循环");
}while(10>100)

迭代写法：
int index = 0;
do{
	index++;
}while(index < 10)
```
# 转向语句：break，continue，return
## break
break作用：
默认情况下，终止离它最近的循环
也可以通过标识符的方式，终止特定的循环

```java
for(int i = 0;i < 10;i++){
	if(i == 5){
		break;
	}
	System.out.println("数字"+i);
}
这段代码的意思是循环0到9如果遇到i等于5则跳出这个循环(结束掉整个for循环)
结果：
0
1
2
3
4
```
## continue
continue作用：
终止当前本次循环，直接跳入下一次循环

```java
for(int i = 0;i < 10;i++){
	if(i == 5){
		continue;
	}
	System.out.println("数字"+i);
}
这段代码的意思是循环0到9，如果遇到i等于5的情况则终止i等于5的那次循环直接跳入下一次循环
结果：
0
1
2
3
4
6
7
8
9
```
