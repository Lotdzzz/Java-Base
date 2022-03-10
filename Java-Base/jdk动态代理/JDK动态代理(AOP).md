@[TOC](目录)


## 动态代理：
***使用JDK的反射机制，创建对象的能力，创建的是代理类的对象，而不是.java类文件，不需要写java文件
动态：在程序执行时，调用jdk提供的方法才能创建代理类的对象
jdk动态代理，必须有接口，目标类必须实现接口***
## 动态代理的作用：
可以在不改变原来目标方法功能的前提下，可以在代理中添加或增强自己的功能代码

## 代理(静态代理/动态代理)：

> 静态代理：代理类是自己实现的，需要自己创建java类，来表示代理类，同时代理的目标类是固定的 优点：静态代理实现简单，容易理解
> 缺点：当目标类增加了，代理类也可能需要成倍增加，当在目标继承的接口类中的方法功能增加了，会影响更多的类都需要重写方法，代码难度加大。

**使用代理模式的作用：
功能增强，在原功能上增加额外的功能。
控制访问，代理类不让你访问目标类，列如商家不让用户访问厂家。**

代理在程序中的体现是：

> **接口类——> 实现接口中方法的类(目标)[其他类不能直接通过目标类来调用调用这个类的方法]——> 需要一个代理类来提供这个类的入口(代理)——> 其他类需要通过这个代理类来去调用目标类中的方法，这叫做代理。**

目标类：

```java
public interface SayHello {
    //创建一个接口类，来提供目标类中所需要实现的方法
    int sayHello(String say);
}
```

```java
//定义一个目标类，来提供方法[假设这个方法不能被其他普通的类调用]
public class HelloMessage implements SayHello{
    @Override
    public int sayHello(String say) {
        System.out.println(say);
        return 1;
    }
}
```
代理类：

```java
//代理类实现sayHello的功能,需要向目标类声明
public class middleman implements SayHello{
    //声明目标类
    private SayHello hm = new HelloMessage();
    @Override
    //实现sayHello方法
    public int sayHello(String say) {
        //目标类返回值为1，而代理类不满意返回结果的话需要增强功能
        int result = hm.sayHello(say);
        //增强功能
        //在原有的结果做改变的行为都叫做增强功能
        result+=10;
        return result;
    }
}
```
测试：

```java
public class Main {
    public static void main(String[] args) {
        //创建代理类对象
        SayHello middleman = new middleman();
        int result = middleman.sayHello("你好，世界");
        System.out.println("增强功能后结果为："+result);
        //结果为11
    }
}
```

## 动态代理实现：
cglib动态代理(了解)：cglib是第三方的工具库，创建代理对象，cglib的原理是继承，
cglib通过继承目标类，创建它的子类，在子类中重写父类中同名的方法，实现功能的修改
cglib中目标类不能是final修饰的方法也不能是final修饰的，在很多框架中使用。

> 代理中的目标类很多的时候，可以使用动态代理，避免静态代理的缺点
> 动态代理中目标类即使很多，代理类数量却很少，修改接口中的方法时，不会影响其他的类
> 动态代理中目标类是可以改变的，不用写代理类就可以创建代理类对象

**jdk动态代理需要java.lang.reflect反射包下的三个类：**

```java
> InvocationHandler： //表示你的代理要干什么
> 这是个接口，内只有一个方法invoke()；
> invoke方法表示你代理对象要执行的功能代码
> 作用：调用方法，增强功能
> 方法原形：public Object invoke(Object proxy, Method method, Object[] args)
> proxy创建的代理对象，无需赋值
> method目标类中的方法，无需赋值
> args目标类中方法的参数，无需赋值
> 使用方法：创建类实现接口InvocationHandler
> 重写invoke()方法，把功能写到这里面
```
```java
Method：//用来执行目标方法的
表示方法的，用来标识目标类中的方法
作用：通过method来执行目标类的某个方法
使用：Method.invoke(目标对象,方法参数);返回Object类型
```
```java
Proxy：
核心的对象，是用来创建对象的
使用proxy的方法代理new对象：静态方法Proxy.newProxyInstance();
方法原形：public static Object newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h)
loader类加载器，负责向内存中加载对象，使用反射来获取ClassLoader
使用方法：类对象.getClass().getClassLoader();需要目标对象的类加载器
interfaces接口，目标对象的接口，也是通过反射获取
h自己创建的InvocationHandler
通过这个方法加载出一个Object对象，这个对象就是代理对象
```

## 动态代理的使用：
实现步骤：

> 1：创建接口，定义目标类要完成的功能 
> 2：创建目标类实现接口
> 3：创建InvocationHandler接口的实现类，在invoke方法中完成代理类的功能
> 4：使用Proxy类的静态方法newProxyInstance方法创建代理对象，并把返回值转为接口类型

接口和目标类：

```java
public interface SayHello {
    //创建一个接口类，来提供目标类中所需要实现的方法
    int sayHello(String say);
}
```

```java
//定义一个目标类，来提供方法[假设这个方法不能被其他普通的类调用]
public class HelloMessage implements SayHello{
    @Override
    public int sayHello(String say) {
        System.out.println(say);
        return 1;
    }
}
```
创建InvocationHandler接口实现类完成代理类的功能：

```java
//实现InvocationHandler来实现功能，增强功能
public class MyInvocationHandler implements InvocationHandler {
    //创建目标对象，目标对象是活动的不是固定的需传入
    private Object target = null;

    public MyInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object result = null;
        //使用method方法来调用目标类方法
        //args就是参数
        //第一个参数就是目标对象
        result = method.invoke(target,args);
        //增强功能
        if(result!=null){
            Integer num = (Integer) result;
            num+=10;
            result = num;
        }
        return result;
    }
}
```
使用Proxy类的静态方法newProxyInstance方法创建代理对象，并把返回值转为接口类型：

```java
public class Main {
    public static void main(String[] args) {
        //创建代理对象，使用Proxy
        //创建目标对象
        SayHello sayHello = new HelloMessage();
        //创建InvocationHandler对象
        //给handler设置动态目标对象
        InvocationHandler handler = new MyInvocationHandler(sayHello);
        //创建代理对象
        //将返回值转成接口
        SayHello proxy = (SayHello) Proxy.newProxyInstance(sayHello.getClass().getClassLoader(),sayHello.getClass().getInterfaces(),handler);
        //通过代理对象执行方法
        int result = proxy.sayHello("你好，世界");
        System.out.println(result);
    }
}
```

