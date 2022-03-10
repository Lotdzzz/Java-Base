@[TOC](目录)
# 消息队列介绍

> 消息是指传送的数据
> 消息队列（Message Queue）是一种通信方式，消息发送后可以立即返回
> 有消息系统来确保消息的可靠传递
> 消息发布者只管把消息发布到MQ中不需要管其他的
> 消息使用者只管从MQ中获取消息而不需要关心是谁发布的（异步处理，先进先出）
> ****
> 消息队列用于业务解耦合，保证消息的最终一致性，广播，错峰流控等

> RabbitMQ是由Erlang语言开发的AMQP的开源
> AMQP：高级消息队列协议
> RabbitMQ特点：
> 1：可靠性
> 2：灵活的路由（转发信号）
> 3：消息集群（支持RabbitMQ组成集群）
> 4：高可用（在集群上支持镜像）
> 5：支持多种协议
> 6：支持java语言
> 7：管理界面（gui界面）
> 8：跟踪机制
# RabbitMQ安装
> 安装下载Erlang语言：[Erlang官网](https://www.erlang.org/downloads)
> 安装下载RabbitMQ-server：[github官网](https://github.com/rabbitmq/rabbitmq-server/releases/tag/v3.9.12)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1611ccfbb6ab47d590a1ab02815db6b0.png)
> 安装依赖命令：yum install gcc glibc-devel make ncurses-devel openssl-devel xmlto
> ****
> 安装Erlang：
> 1：到安装目录下解压erlang源码包
> 命令：tar -zxvf otp_src_24.2.tar.gz
> 2：创建erlang安装目录
> mkdir /usr/local/erlang
> 4：进入erlang解压目录
> cd otp_src_24.2
> 5：配置erlang的安装信息
> ./configure --prefix=/usr/local/erlang --without-javac
> 6：编译并安装
> make && make install
> 7：配置环境变量
```bash
ERL_HOME=/usr/local/erlang
PATH=$ERL_HOME/bin:$PATH
export ERL_HOME PATH
```
> 8：重载环境变量
> 命令：source /etc/profile
> 检查erlang：erl -version
> ****
> 安装RabbitMQ
> 进入到存放rabbitmq-server-3.9.12-1.el7.noarch.rpm的目录
> 命令：rpm -ivh --nodeps rabbitmq-server-3.9.12-1.el7.noarch.rpm
> RabbitMQ启动服务：rabbitmq-server strat
> 注意：这里可能会出现错误，错误原因是/var/lib/rabbitmq/.erlang.cookie文件权限不够。
解决方案对这个文件授权
  chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie
chmod 400 /var/lib/rabbitmq/.erlang.cookie
> RabbitMQ关闭服务：rabbitmqctl stop
> ****
> 添加插件：rabbitmq-plugins enable 插件名
> 添加：rabbitmq-plugins enable rabbitmq_management
> 删除插件：rabbitmq-plugins disable 插件名
> 插件添加完毕后可以到linux本机的浏览器访问地址：http://192.168.73.132:15672/
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/2c7712ba86d14c399adedc90b4495dc7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ****
> RabbitMQ用户管理
> RabbitMQ默认用户guest密码guest
> 添加用户：rabbitmqctl add_user 用户名 密码
> 删除用户：rabbitmqctl delete_user 用户名
> 修改用户：rabbitmqctl change_password 用户名 密码
> 设置用户角色：rabbitmqctl set_user_tags 用户名 角色
> 角色（等级）：management（1），monitoring（2），policymaker（2），administrator（4）

> 权限
```bash
1、	授权命令：rabbitmqctl set_permissions [-p vhostpath] {user} {conf} {write} {read}
-p vhostpath ：用于指定一个资源的命名空间，例如 –p / 表示根路径命名空间
user：用于指定要为哪个用户授权填写用户名
     conf:一个正则表达式match哪些配置资源能够被该用户配置。
       write:一个正则表达式match哪些配置资源能够被该用户读。
       read:一个正则表达式match哪些配置资源能够被该用户访问。
   例如：
     rabbitmqctl set_permissions -p / root '.*' '.*' '.*'
   用于设置root用户拥有对所有资源的 读写配置权限
2、查看用户权限 rabbitmqctl  list_permissions [vhostpath]
例如
  查看根径经下的所有用户权限
  rabbitmqctl  list_permissions 
  查看指定命名空间下的所有用户权限
  rabbitmqctl  list_permissions /abc 
3、查看指定用户下的权限rabbitmqctl  list_user_permissions {username}
例如
  查看root用户下的权限
  rabbitmqctl  list_user_permissions root
4、清除用户权限rabbitmqctl  clear_permissions {username}
例如：
	清除root用户的权限
rabbitmqctl  clear_permissions root
```
> vhost管理
```bash
vhost是RabbitMQ中的一个命名空间，可以限制消息的存放位置利用这个命名空间可以进行权限的控制有点类似Windows中的文件夹一样，在不同的文件夹中存放不同的文件。
1、添加vhost: rabbitmqctl add vhost {name}
例如
  rabbitmqctl add vhost test
2、删除vhost：rabbitmqctl delete vhost {name}
   例如
    rabbitmqctl delete vhost test
```
# RabbitMQ消息发送和接收
> 消息发送和接收机制
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/546ae640527e4f66a9dd59d54ccf6497.png)
```bash
1、Message
消息，消息是不具体的，它由消息头和消息体组成。消息体是不透明的，而消息头则
由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息
的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。 
2、Publisher
消息的生产者，也是一个向交换器发布消息的客户端应用程序。 
3、Exchange
交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。 
4、Binding
绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换
器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的
路由表。 
5、Queue
消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的
终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接
到这个队列将其取走。 
6、Connection
网络连接，比如一个TCP连接。 
7、Channel
信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的
TCP连接内地虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、
订阅队列还是接收消息，这些动作都是通过信道完成。因为对于操作系统来说建
立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 
连接。 
8、Consumer
消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。 
9、Virtual Host
虚拟主机，表示一批交换器、消息队列和相关对象。虚拟主机是共享相同的
身份认证和加密环境的独立服务器域。每个 vhost 本质上就是一个 mini 版的 
RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制。vhost 是 AMQP 
概念的基础，必须在连接时指定，RabbitMQ 默认的 vhost 是 / 。 
10、Broker
表示消息队列服务器实体。
```
> Exchange交换器类型：direct，fanout，topic，headers
> ****
> direct：
> 一对一精准匹配
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/70df57c667ee4d7c8c2d8fccdd211a6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/91c3a246d7a7474896dafd9a6f907269.png)
> ****
> Fanout交换器
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1e461d1b1d5a4a23a0b7cdbe12101619.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/555058a9463c43688a6d28c605d17fc7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ****
> topic交换器
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/25d624a83ea8431cb6c92d829c2747a1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/b583ddafdbe04fef83772fb035dce14a.png)
## 消息测试
> 添加依赖
```html
<dependency>
    <groupId>com.rabbitmq</groupId>
    <artifactId>amqp-client</artifactId>
    <version>5.14.0</version>
</dependency>
```
> 测试发送消息到队列
```java
public class App {
    public static void main( String[] args ) {
        //创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");//设置ip
        factory.setPort(5672);//设置端口
        factory.setUsername("root");//账号
        factory.setPassword("123");//密码
        Connection connection = null;//定义连接
        Channel channel = null;//定义通道
        try {
            connection = factory.newConnection();//获取连接
            channel = connection.createChannel();//获取通道

            //声明一个队列
            //参数1 为队列名 自定义
            //参数2 为是否为持久化的队列
            //参数3 是否排外，如果排外则这个队列只允许一个消费者监听
            //参数4 是否自动删除队列，true表示队列中没有数据时删除
            //参数5 为队列属性设值 null
            channel.queueDeclare("myQueue",true,false,false,null);
            //定义消息
            String message = "RabbitMQ Message Test";

            //发送消息
            /*
            * 参数1 交换机名称 暂不用
            * 参数2 队列名或routingKey
            * 参数3 消息的属性信息
            * 参数4 为具体的消息的字节数组
            * */
            channel.basicPublish("","myQueue",null,message.getBytes("utf-8"));
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }finally {
            if(channel != null){
                try {
                    channel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                } catch (TimeoutException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null){
                try {
                    connection.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/fa98859a11a24b5ab159b23f9cfbc6cb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> 接收消息
```java
public class App {
    public static void main( String[] args ) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            //接收消息
            /*
            * 参数1 接收消息的队列名称 监听的队列
            * 参数2 为消息是否自动确认 true表示自动确认 接收消息之后会自动将消息从队列中弹出
            * 参数3 为消息接受者的标签 用于当多个消费者同时监听一个队列时用于区分不同消费者，通常为空字符串
            * 参数4 为消息接收的回调方法 具体完成对消息的处理
            * 注：使用了basicConsume方法后会启动一个线程监听队列，如果队列中有新的信息进入队列会自动接收消息
            * 因此不能关闭连接和通道
            * */
            channel.basicConsume("myQueue",true,"",new DefaultConsumer(channel){
                //重写方法 消息的具体接收和处理的方法
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body,"utf-8");
                    System.out.println("消息为"+message);
                }
            });

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }
    }
}
```
## 交换器测试
### direct交换机
> 消息发送到
```java
public class App {
    public static void main( String[] args ) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            //声明队列
            channel.queueDeclare("myQueue",true,false,false,null);

            /*
            * 声明一个交换机
            * 参数1 位交换机名称
            * 参数2 为交换机的类型（direct，fanout，topic，headers）
            * 参数3 为是否为持久化交换机
            * */
            channel.exchangeDeclare("myExchange","direct",true);

            /*
            * 绑定交换机
            * 参数1 队列名称
            * 参数2 交换机名称
            * 参数3 消息的routingKey或者bindingKey
            * 注：在进行队列和交换机绑定时必须要确保交换机和队列声明成功
            * */
            channel.queueBind("myQueue","myExchange","directRoutingKey");

            String message = "direct message test";
            //发送消息到队列
            channel.basicPublish("myExchange","directRoutingKey",null,message.getBytes(StandardCharsets.UTF_8));

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }finally {
            if(channel != null){
                try {
                    channel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                } catch (TimeoutException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null){
                try {
                    connection.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
> 接收消息
```java
ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            channel.queueDeclare("myQueue",true,false,false,null);
            channel.exchangeDeclare("myExchange","direct",true);
            channel.queueBind("myQueue","myExchange","directRoutingKey");

            //获取消息
            channel.basicConsume("myQueue",true,"",new DefaultConsumer(channel){
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body);
                    System.out.println("消息"+message);
                }
            });

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }
```
### fanout交换机
> 发送消息
```java
public class App {
    public static void main( String[] args ) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            channel.exchangeDeclare("fanoutExchange","fanout",true);

            String message = "direct message test";
            channel.basicPublish("fanoutExchange","",null,message.getBytes(StandardCharsets.UTF_8));

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }finally {
            if(channel != null){
                try {
                    channel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                } catch (TimeoutException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null){
                try {
                    connection.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
> 接收消息
```java
public class App1 {
    public static void main(String[] args) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            /*
            * 由于Fanout类型的交换机的消息是类似广播的模式，他不需要绑定RoutingKey
            * 而又有可能会有很多个消费者来接收这个交换机的数据，因此我们创建队列时创建一个随机的队列名称
            *
            * 没有参数的queueDeclare方法会创建一个名字为随机的一个队列
            * 这个队列的数据是非持久化
            * 是排外的
            * 自动删除的
            * getQueue这个方法用于获取这个随机的队列名
            * */
            String queueName = channel.queueDeclare().getQueue();
            channel.exchangeDeclare("fanoutExchange","fanout",true);
            //将这个随机的队列绑定到交换机中，由于是fanout类型的交换机因此不需要指定routingKey
            channel.queueBind(queueName,"fanoutExchange","");

            /*
            * 监听某个队列并获取队列中的数据
            * */
            channel.basicConsume(queueName,true,"",new DefaultConsumer(channel){
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body);
                    System.out.println("消息"+message);
                }
            });

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }
    }
}
```
### topic交换机
> 消息发送
```java
public class App {
    public static void main( String[] args ) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            channel.exchangeDeclare("topicExchange","topic",true);

            String message = "message test";
            //aa.123有aa.*或aa.#能收到 aa.123.123只有aa.#能收到
            channel.basicPublish("topicExchange","aa.123",null,message.getBytes(StandardCharsets.UTF_8));

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }finally {
            if(channel != null){
                try {
                    channel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                } catch (TimeoutException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null){
                try {
                    connection.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
> 消息接收
```java
public class App1 {
    public static void main(String[] args) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            channel.queueDeclare("topicQueue",true,false,false,null);
            channel.exchangeDeclare("topicExchange","topic",true);
            channel.queueBind("topicQueue","topicExchange","aa.*");

            channel.basicConsume("topicQueue",true,"",new DefaultConsumer(channel){
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body);
                    System.out.println("消息"+message);
                }
            });

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }
    }
}
```

> topic和fanout使用场景：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/23ed904027f147a5822e0007ce4bf428.png)
## 事务性消息
> 事务机制与数据库的事务相似
> 保证所有消息命令要么同时写入队列要么同时失败
> RabbitMQ事务：
> 1：通过AMQP提供的事务机制实现
> 2：发送者确认模式
```java
channel.txSelect();//声明启动事务模式
channel.txCommit();//提交事务
channel.txRollback();//回滚事务
```
> 发送者事务
```java
public class App {
    public static void main( String[] args ) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            channel.exchangeDeclare("fanoutExchange","fanout",true);

            String message = "message test";
            //启动事务
            channel.txSelect();

            channel.basicPublish("fanoutExchange","",null,message.getBytes(StandardCharsets.UTF_8));

            //提交事务
            channel.txCommit();
        } catch (IOException e) {
            e.printStackTrace();
            try {//事务回滚
                channel.txRollback();
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        } catch (TimeoutException e) {
            e.printStackTrace();
            try {
                channel.txRollback();
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }finally {
            if(channel != null){
                try {
                    channel.txRollback();//添加事务回滚，放弃当前事务中所有没有提交的消息，释放内存
                    channel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                } catch (TimeoutException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null){
                try {
                    connection.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
### 发送者确认模式
> 普通事务当消息发送失败会进行事务回滚报错
> 而确认者模式当消息发送失败会进行补发操作

> 语法：
```java
package com.java;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.util.concurrent.TimeoutException;

public class App {
    public static void main( String[] args ) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            channel.exchangeDeclare("fanoutExchange","fanout",true);

            //启动发送者确认模式
            channel.confirmSelect();
            String message = "message test";

            channel.basicPublish("fanoutExchange","",null,message.getBytes(StandardCharsets.UTF_8));
            //确认模式
            channel.waitForConfirmsOrDie();
            
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            if(channel != null){
                try {
                    channel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                } catch (TimeoutException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null){
                try {
                    connection.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```
> 发送者确认者模式----普通确认
> ****
> 启动发送者确认模式
> channel.confirmSelect();
> boolean flag = channel.waitForConfirms();
> waitForConfirms的作用：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/e7404e01a15e4386b104c58327170439.png)

> 发送者确认者模式----批量确认
> 启动发送者确认模式
> channel.confirmSelect();
> 批量确认：channel.waitForConfirmsOrDie();
> 作用：
> 批量消息确认的速度比普通的消息确认快
> 但是如果一旦消息出现了消息补发的情况，不能确定具体是哪条消息没有完成发送
> 需要将本次所有消息全部进行补发

> 发送者确认模式----异步确认
> 启动发送者确认模式
> channel.confirmSelect();
> 异步确认：
```java
public class App {
    public static void main( String[] args ) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            channel.exchangeDeclare("fanoutExchange","fanout",true);

            //启动发送者确认模式
            channel.confirmSelect();
            String message = "message test";

            
            //声明异步监听器
            channel.addConfirmListener(new ConfirmListener() {
                //消息确认后的回调方法
                /*
                * 参数1 为被确认的消息的编号 从1开始
                * 参数2 当前是否处理了多条消息并确认 编号也会变
                * */
                @Override
                public void handleAck(long l, boolean b) throws IOException {

                }
                //消息没有确认的回调方法
                //如果这个方法被执行说明需要消息补发
                @Override
                public void handleNack(long l, boolean b) throws IOException {

                }
            });

            channel.basicPublish("fanoutExchange","",null,message.getBytes(StandardCharsets.UTF_8));
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if(channel != null){
                try {
                    channel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                } catch (TimeoutException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null){
                try {
                    connection.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
> 消费者确认模式----手动确认
```java
public class App1 {
    public static void main(String[] args) {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.73.132");
        factory.setPort(5672);
        factory.setUsername("root");
        factory.setPassword("123");
        Connection connection = null;
        Channel channel = null;
        try {
            connection = factory.newConnection();
            channel = connection.createChannel();

            String queueName = channel.queueDeclare().getQueue();
            channel.exchangeDeclare("fanoutExchange","fanout",true);
            channel.queueBind(queueName,"fanoutExchange","");

            //basicConsume方法的参数2为自动确认消息 改成false表示接收到消息 但是没有处理 需要手动确认
            //手动确认：调用方法
            /*
            * 方法basicAck();用于肯定确认 可以批量确认
            * */
            channel.basicConsume(queueName,false,"",new DefaultConsumer(channel){
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body);
                    System.out.println("消息"+message);
                    long num = envelope.getDeliveryTag();//获取消息的编号
                    Channel c = super.getChannel();//获取当前内部类中的通道 使用this也可以

                    //手动确认消息 确认之后表示消息已经被处理需要从队列中弹出 这个方法在当前消息处理程序全部完成后执行
                    //参数1 为消息的编号
                    //参数2 true需要确认小于当前编号的所有消息 ，false表示只处理当前编号
                    c.basicAck(num,true);
                    
                }
            });

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }
    }
}
```
# SpringBoot集成RabbitMQ
> 选中依赖项：Spring for RabbitMQ
> applicaition配置文件配置RabbitMQ
```bash
spring.rabbitmq.host=192.168.73.132
spring.rabbitmq.port=5672
spring.rabbitmq.username=root
spring.rabbitmq.password=123
```
> 创建交换机并绑定

> 配置文件方式创建交换机
```java
@Configuration
public class RabbitMQConfig {

    //配置一个Direct类型的交换机
    @Bean
    public DirectExchange directExchange(){
        return new DirectExchange("directExchange");
    }

    //配置一个队列
    @Bean
    public Queue directQueue(){
        return new Queue("directQueue");
    }

    //绑定
    /*
    * 参数1 Queue directQueue 需要绑定的队列 参数名必须和@Bean下的某个队列方法名完全相同 会自动注入到这个参数内
    * 参数2 DirectExchange directExchange 需要绑定的交换机 参数名必须和@Bean下的某个队列方法名完全相同 会自动注入到这个参数内
    * */
    @Bean
    public Binding binding(Queue directQueue,DirectExchange directExchange){
        return BindingBuilder.bind(directQueue).to(directExchange).with("directRouting");
    }

}
```
> 发送消息

```java
@Controller
public class app {

    //注入amqp模板 利用这个对象发送和接收消息
    @Resource
    private AmqpTemplate amqpTemplate;

    @RequestMapping(value = "/send")
    @ResponseBody
    public void rabbitMQMessage(){
        /*
        * 发送消息方法convertAndSend
        * 参数1 交换机名
        * 参数2 RoutingKey
        * 参数3 发送的消息
        * */
        amqpTemplate.convertAndSend("directExchange","directRouting","Message test");
    }
}
```
> 异步监听接收消息
```java
@Component
public class app {

    //注入amqp模板 利用这个对象发送和接收消息
    @Resource
    private AmqpTemplate amqpTemplate;

    /*
    * 注解RabbitListener用于标记当前方法是一个监听方法
    * 作用是持续性的自动接收消息，这个方法不需要手动调用spring会自动运行这个监听
    *
    * 参数message是每次监听到的消息
    * */
    @RabbitListener(queues = {"directQueue"})
    public void rabbitMQMessage(String message){
        System.out.println(message);
    }
}
```
> fanout类型接收消息
```java
@Component
public class app {

    //注入amqp模板 利用这个对象发送和接收消息
    @Resource
    private AmqpTemplate amqpTemplate;

    @RabbitListener(bindings = {//注解RabbitListener参数bindings绑定队列交换机
            @QueueBinding(
                    value = @Queue(),//@Queue创建一个队列 随机队列
                    exchange = @Exchange(name = "fanoutExchange",type = "fanout"))//创建一个交换机
    })
    public void rabbitMQMessage(String message){
        System.out.println(message);
    }
}
```
> fanout消息发送

```java
@Configuration
public class RabbitMQConfig {

    @Bean
    public FanoutExchange fanoutExchange(){
        return new FanoutExchange("fanoutExchange");
    }

}
```
```java
@Controller
public class RabbitMQConfig {

    @Resource
    private AmqpTemplate amqpTemplate;

    @RequestMapping(value = "/send")
    @ResponseBody
    public void sendMessage(){
        amqpTemplate.convertAndSend("fanoutExchange","message test");
    }

}
```
> topic接收消息
```java
@Component
public class app {

    //注入amqp模板 利用这个对象发送和接收消息
    @Resource
    private AmqpTemplate amqpTemplate;

    @RabbitListener(bindings = {//注解RabbitListener参数bindings绑定队列交换机
            @QueueBinding(
                    value = @Queue("topic"),//@Queue创建一个队列 随机队列
                    key = {"aa.#"},
                    exchange = @Exchange(name = "topicExchange",type = "topic"))//创建一个交换机
    })
    public void rabbitMQMessage(String message){
        System.out.println(message);
    }
}
```
> 配置交换机
```java
@Bean
    public TopicExchange topicExchange(){
        return new TopicExchange("topicExchange");
    }
```
> 发送消息
```java
@Controller
public class RabbitMQConfig {

    @Resource
    private AmqpTemplate amqpTemplate;

    @RequestMapping(value = "/send")
    @ResponseBody
    public void sendMessage(){
        amqpTemplate.convertAndSend("topicExchange","message test");
    }

}
```
# RabbitMQ集群
> 安装2两台Linux操作系统并修改hostname 分别为A和B
执行vi /etc/hosts 文件内容如下
```bash
第一台机器
127.0.0.1 A  localhost localhost.localdomain localhost4 localhost4.localdomain4
::1       A  localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.222.129 A
192.168.222.130 B
第二台机器
127.0.0.1 B  localhost localhost.localdomain localhost4 localhost4.localdomain4
::1       B  localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.222.129 A
192.168.222.130 B
```
> 两台机器安装RabbitMQ（执行本文章目录的RabbitMQ安装，两台机器都需要安装）

> 配置Cookie文件
> Erlang Cookie是保证不同节点可以互相通信的秘钥,要保证集群中的不同节点互相通信必须共享相同的Erlang Cookie,具体存放在/var/lib/rabbitmq/.erlang.cookie
```bash
命令：cat /var/lib/rabbitmq/.erlang.cookie
会获取一串密钥CGCMOJQVMLUNUCQNSLFD
```
> 必须要保证2台Linux的Cookie 文件内容完全相同，可以选择使用vim进行编辑
也可以使用scp命令完成文件跨机器拷贝例如
```bash
命令：scp /var/lib/rabbitmq/.erlang.cookie 192.168.222.130:/var/lib/rabbitmq
```
> 注意：由于这个文件的权限是只读因此无论是使用vim还是scp来实现Cookie文件的同步都会失败，因此必须要修改这个文件的权限,
例如 chmod 777 /var/lib/rabbitmq/.erlang.cookie
当Cookie文件同步完成以后再修改权限回只读
例如 chmod 400 /var/lib/rabbitmq/.erlang.cookie

> 启动两台机器的RabbitMQ服务
> 在机器A中执行以下命令：
```bash
rabbitmqctl stop_app
rabbitmqctl join_cluster rabbit@B
rabbitmqctl start_app
```
> 查看结点数量：rabbitmqctl cluster_status
## SpringBoot链接集群
> 配置application配置文件
```bash
spring.rabbitmq.addresses=192.168.73.132:5672,192.168.73.133:5672
spring.rabbitmq.username=root
spring.rabbitmq.password=123
```
> 添加镜像规则
![在这里插入图片描述](https://img-blog.csdnimg.cn/c0d7d13cb23f4294818a4f75b65915de.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

