@[TOC](目录)
## Linux的安装与工具安装

> **linux系统下载安装教程：[Linux的安装与工具的安装](https://blog.csdn.net/qq_39888626/article/details/121085295?spm=1001.2014.3001.5502)**

## Linux目录结构
使用Xshell工具连接进入linux系统
**在命令行终端输入指令进入根目录：cd /
然后输入ls查看根目录下的文件：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/4c14efb8e64f4d428cbd6d8763b50cec.png)
linux目录结构类似于树
![在这里插入图片描述](https://img-blog.csdnimg.cn/e9d5c257e1a84c66b54064263395a3dc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**部分目录介绍：**
> bin：存放最常用的命令文件
> :
> etc：这个目录存放系统的配置文件，配置环境变量就是在此目录下
> :
> lib：存放目录的库文件
> :
> opt：这是给用户提供的文件，安装自己的软件或者存放自己的文件在此目录下
> :
> home：存放自己创建的用户，类似于windows的系统用户，底下有文档，图片，下载等
## Linux系统vi和vim编辑器
vi编辑器类似于windows的记事本，vim是vi的升级版

vi/vim的使用方法：

vi和vim对文件的操作分为三种模式：**查看模式，写入模式，命令模式**
用vim打开文件是不能直接进行写入的，这时候是查看模式

**查看模式能做的事：**
|指令 | 作用 |
|--|--|
| yy | 复制游标所在的那一行 |
| x, X| 在一行字当中，x 为向后删除一个字符 (相当于 [del] 按键)， X 为向前删除一个字符 |
|dd|删除游标所在的那一整行|
|u|复原前一个动作|
|[Ctrl]+r|重做上一个动作|
**查看模式中按a或者按i可进入写入模式**
**写入完成之后需要按esc退出写入模式进入查看模式然后按(shift+：)进入命令模式**
在命令模式中才可以保存退出指令：
wq：保存并退出
q!：强制退出不保存
q：只是退出编辑器
![在这里插入图片描述](https://img-blog.csdnimg.cn/948816a91eec42559f5c75ea43b0283b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
## Linux中对用户的操作命令
**linux中多数使用root超级账号来创建用户**

> **添加用户：useradd 用户名** 	
> ：此方法创建用户，用户的文件会自动放到home文件夹下
> ：创建用户的时候如果不指定组，会自动创建一个组，并把用户放到组中
> **指定用户目录命令：useradd -d 文件路径 用户**

> **给用户设置密码：passwd 用户**

> **删除用户：userdel 用户**
> ：这种删除会保留用户目录
> **删除用户并级联删除用户目录：userdel -r 用户**

> **查看用户信息：id 用户**
![在这里插入图片描述](https://img-blog.csdnimg.cn/afd4cd8a2e894d08ae3de40b4864e588.png)
gid修饰的组是这个用户的主组，每个用户都有一个主组，不可更改，后面的组=是附加组

> **切换用户：su 用户**
> root转用户不用输入密码，其他用户转root需要输入密码

## Linux中关于组的管理命令
组的意思就是给用户分类管理，不同权限的用户做不同的事，可以对有共性的用户进行统一管理
每一个用户至少属于一个组，不能独自存在
如果创建用户的时候不指定组，则用户会自动创建一个组

> **创建组：groupadd 组名**

> **删除组：groupdel 组名**

>**把用户添加到组中（一个用户可以拥有多个组）：gpasswd -a 用户 组名**

>**把用户移出组中（主组不能更改）：gpasswd -d 用户 组名**

>**创建用户时指定组（主组）：useradd -g 组名 用户**

## Linux中的系统操作命令
| 指令 | 作用 |
|--|--|
| sync | 将数据由内存同步到硬盘中 |
|shutdown|关机指令|
|shutdown –h 10 ‘十分钟后关机’|计算机将在10分钟后关机，并且会显示在用户的屏幕中|
|shutdown –h now|立马关机|
|shutdown –h 20:25|系统会在今天20:25关机|
|shutdown –h +10|十分钟后关机|
|shutdown –r now|系统立马重启|
|shutdown –r +10|系统十分钟后重启|
|reboot|重启|
**Linux帮助命令**

> linux手册大全：[linux手册大全网站](https://man.linuxde.net/) 
> linux帮助命令：
> ——>linux系统手册帮助：man 命令 
> ——>分屏显示，按回车翻一行，按空格翻一页，按q退出 
> linux内置帮助命令： ——>help 命令

**Linux文件和目录的操作命令**

> **查看当前所在的目录：pwd**
>**查看当前目录下的内容：ls**
>**ls也可以指定目录：ls 路径**

**文件和目录操作命令**

> **以列表的形式查看目录：ls -l**
> **查看目录下的子文件夹，文件和隐藏文件：ls -a**
> **以列表形式查看目录下的所有：ls -al**

>根目录或以盘符开始的目录叫做绝对目录
>以当前目录或目录名开始的叫做相对目录

>**切换目录：cd (相对/绝对)路径**

>**创建目录：mkdir 目录名**
>**创建多级目录（在目录1下创建目录2并在目录2下创建目录3）：mkdir -p 目录名1/目录名2/目录3**

>**删除目录：rmdir 目录名**

>**创建文件：touch 文件名，文件名(多个文件可选)**

>**复制文件：cp 源文件 目标目录**
>**复制目录：cp -r 源文件 目标目录**

>**删除文件或者目录：rm 文件名或目录名**
>**强制删除：rm -f 文件名或目录名**
>**强制删除目录：rm -rf 目录名**

>**移动(剪切)文件或目录：mv 文件名 目录名**

>**在命令行查看文件：cat 文件名**
>**显示行号：cat -n 文件名**

>**分页查看文件：more 文件名**
>**分页查看文件类似：less 文件名**

>**查看文件头10行：head 文件名**
>**显示行号指定行数：head -n 行数 文件名**
>**查看后10行：tail 文件名：tail -n 函数 文件名**

>**查看环境变量：echo $变量(大多数都是大写)**

>**将一个查看命令的输出结果写入到文件中：查看命令 > 文件名**
>如果文件不存在，则新建文件
>如果文件存在，则把文件的内容覆盖

**查看日期操作**
>**查看系统当前时间：date**
>**系统当前年份：date +%y**
>**系统当前月份：date +%m**
>**系统当前日期：date +%d**
>**按格式显示日期：date '+%y-%m-%d %H:%M:%S'**
>**设置系统时间：date -S '时间'**
>**查看系统日历：cal**
>**查看指定年份的日历：cal 年份**

**查找文件或者目录的命令**
>**查找文件或者目录：find 关键字**
>模糊查找：find *关键字 *
>查找指定后缀的所有文件：find *.后缀名
>按文件大小查找：find 目录 -size +5M
>按用户创建的文件搜索：find -user 用户
>

>**类似find命令的搜索：locate**
>**locate命令跟find区别是locate去层级式的数据库查找**
>**执行locate前先输入updatedb同步数据库**

**过滤参数**
>**查看/搜索命令 |grep 过滤条件**
>**用于过滤查找**

**压缩与解压命令**
>**1.压缩单个文件gzip 文件名**
>**解压：gunzip 文件名**
>压缩完之后会生成一个后缀.gz的文件
>**2.压缩打包多个文件(兼容gzip)：zip 文件名或目录，文件名(多个文件可选)**
>**解压：unzip 文件名**
>**解压指定目录名：unzip 文件名 -d 解压到目录名**
>**3.压缩解压命令：tar [选项]**
>**压缩：tar -zcvf 压缩后生成的文件名 被压缩的文件名目录名，文件名(可选)**
>**解压：tar -zxvf 压缩包名 -C 被解压到的文件名**

## Linux文件与组管理
linux中每一个用户至少属于一个组，用户不能独立存在，一个用户可以属于多个组
linux中每一个文件也属于一个组，文件只能属于一个组，可以通过组对文件的控制来决定哪个用户来对他进行不同的操作
对文件来说，系统的所有用户分为三类：
所有者：默认文件和目录的所有者都是它的创建者
同组用户：跟文件或者目录属于同一个组的用户
其他组用户：既不是文件或者目录的所有者，也不是同组用户

> **修改文件的所有者：chown 新的所有者 文件名**
> **修改文件所有者并修改组：chown [参数-R(递归的修改)] 新的所有者:新的组 文件名**
另一个命令改文件或目录的组：chgrp 新的组 文件名

## Linux文件或目录的权限管理
文件或者目录的三种权限：
> 读（read）查看文件的内容指令cat，more，head，less等 
> 目录的read权限：比如ls，
> 写（write）修改文件，vi/vim等命令 
> 目录的write权限：比如创建删除子目录或文件等操作
> 执行（execute）目录里的可执行文件xxx.sh等
> 目录的execute权限：能不能进入到目录的权限cd等命令

**文件或目录的权限控制**
任何文件或者目录都有三类权限：
权限的表示
读：r
写：w
执行：x
用u(拥有者)，g(同组)，o(其他)，a(全部)来表示文件的三种权限的使用
用+，-，=来表示增加，减少，设置对应的权限
查看文件有哪些权限：ls -al

> **设置权限：chmod**
> 增加权限：chmod u+r,g-w,o+w test.txt(文件名)
> 以上指令表示对test.txt的拥有者增加读权限，对同组删除写权限，对其他增加写权限
> 也可以用数组的方式对文件进行权限管理：r表示4，w表示2，x表示1
> 数字对应的顺序是从右往左用2的0~n次方表示：从右第一位2的0次方所以
> x=1(2^0)
> w=2(2^1)
> r=4(2^2)
> 比如代码：chmod 777 test.txt
> 这段代码表示给test.txt的拥有者，同组，其他全部增加rwx权限
> rwx--表示7
> r-x--表示5
> -wx--表示3

## Linux的网络配置
配置局域网方法：
**在linux进入目录：cd /etc/sysconfig/network-scripts**
打开目录后进入ifcfg-ensxx后面几位数字可能每个系统不一样
设置局域网的话需要修改里面的参数：
BOOTPROTO=static
ONBOOT=yes
添加
需要查看linux系统的网络：

**打开vmware**
![在这里插入图片描述](https://img-blog.csdnimg.cn/ebd2733f3136410ba7caa403eb9392ac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**然后点击【虚拟网络编辑器】**
![在这里插入图片描述](https://img-blog.csdnimg.cn/00f64693f6bf4477a0b08e2ebb647d4d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ee8ed582eddf48ed9797c8e29c99282d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_18,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/915a678292624a7ebacf37b4ce34c249.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/097b84e47f474480a97189696c02395a.png)
IPADDR=192.168.73.128（这里的参数是在DHCP的128~254范围之间）
GATEWAY=192.168.73.2（这里的参数是NAT下网关IP）
DNS1=192.168.73.2（这里的参数是NAT下网关IP）
**之后重启系统就完成局域网配置**

## Linux关于进程的管理
一个进程会占一个端口，一个软件运行一定有一个入口，比如tomcat端口8080

> 查看进程：ps
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/88133e5ea47b4f91b0d4b9b9f9cf9d47.png)
> 查看所有进程：ps -e
> 每个进程都有pid，可以通过pid杀死进程
> 以全格式查看所有进程：ps -ef
> 查看指定进程状态：ps -ef|grep 进程名
> 杀死进程：kill -9 pid
## Linux关于服务的管理
服务也是程序，本质上是进程，叫守护进程，在后台使用的
操作服务的指令：systemctl，service（旧版）
> systemctl  
> [start(启动| 
> stop(停止)| 
> restart(重启)| 
> reload(重载)|
> status(查看服务状态)| 
> enable(设置服务是否开机启动))]  
> 服务名称 
> 比如查看防火墙服务状态：systemctl status firewalld
## Linux中rpm包的管理
rpm就是软件安装包，安装软件的rpm包
使用软件安装包命令：
> **查看当前系统已经安装的rpm软件：rpm -qa**
> **过滤查看：rpm -qa|grep 软件名**
> **卸载rpm：rpm -e 软件名**
> **安装rpm包需准备好rpm包：rpm -ivh rpm包**
> 注：如果安装rpm时没有包依赖的rpm则会安装失败
## Linux中YUM包的管理
是一种基于rpm的软件管理工具：
这个工具解决了rpm的依赖问题，安装rpm时会自动下载rpm依赖的包
> 查看已安装的rpm列表：yum list
> 安装rpm：yum install 软件名
> 查看当前系统中已安装的rpm软件：yum list installed|grep 软件名
> 卸载rpm软件包：yum remove 软件名
## Linux中安装配置jdk环境变量
首先从官网下载linux版本的jdk8：[oracle官网](https://www.oracle.com/java/technologies/downloads/#java8)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5718385f6e4a472d84d3d8107450df70.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**使用Xftp工具将下载好的tar.gz包传到linux系统根目录下的opt目录下**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c85e1198e0f145d293e06285921baba8.png)
使用Xshell转到opt目录下cd /opt
进行解压：tar -zxvf jdk-8u311-linux-x64.tar.gz
此时opt目录下会生成一个jdk文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/6febafb01a5040bfaebc571b76ab8369.png)
**然后配置环境变量：**
找到etc目录下：cd /etc
打开目录下的profile文件：vim profile
在这一行之后![在这里插入图片描述](https://img-blog.csdnimg.cn/d02fe52d9f7446368ac9588821ce0c2b.png)

**添加以下内容：**
jdk文件目录根据自己下载的目录和名称输入
```java
JAVA_HOME=/opt/jdk1.8.0_311
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME PATH CLASSPATH
```
重启linux系统后输入java -version
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ec64b511eb74999a47b345229ef43d2.png)
**配置完成**
## Linux中安装配置tomcat服务器
首先官网下载tomcat：[tomcat官网](https://tomcat.apache.org/download-10.cgi)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d321a99b020f4252ba746731095ff8d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
同样的方法丢到opt目录下
![在这里插入图片描述](https://img-blog.csdnimg.cn/0ede3988549e46b88257986e3ee5dd89.png)
进入opt目录cd /opt
解压：tar -zxvf apache-tomcat-10.0.12.tar.gz
![在这里插入图片描述](https://img-blog.csdnimg.cn/2262b312430b4737a0b2c06b9001852c.png)
启动需要进入解压完成的目录：
cd apache-tomcat-10.0.12/bin
![在这里插入图片描述](https://img-blog.csdnimg.cn/54acd91a541c46d3abb74902fa58326a.png)
可执行文件输入：./startup.sh
查看服务：ps -ef|grep tomcat
![在这里插入图片描述](https://img-blog.csdnimg.cn/d378e1c9a56f4b8aa50b60ee6caf9e41.png)
可以在linux中的火狐浏览器输入网址http://127.0.0.1:8080
![在这里插入图片描述](https://img-blog.csdnimg.cn/6d57a94b45024926921e45828aeffd87.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
如果想在windows中访问linux的tomcat服务器需要linux关闭防火墙：
命令：systemctl stop firewalld
然后windows的浏览器输入地址（自己linux系统的ip地址）：http://192.168.73.132:8080
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e0ce25b719f458f9a04b35e4f2ba1e3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
**安装完成**
## Linux中安装mysql
首先检查linux系统中是否安装mariadb数据库，mariadb数据库是和mysql数据库冲突的，如果安装了需要卸载：
检查是否安装：yum list installed|grep mariadb
卸载：yum remove mariadb
官网下载linux版mysql：[oracle官网](https://dev.mysql.com/downloads/mysql/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/65b912abdc2b43559f7059850e6c8a6c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
用Xftp放到opt目录下
进入目录：cd /opt
解压文件：tar xvJf mysql-8.0.27-linux-glibc2.12-x86_64.tar.xz
![在这里插入图片描述](https://img-blog.csdnimg.cn/a800400ade384123837f51ffb5f8f5ec.png)
**给文件重命名：mv mysql-8.0.27-linux-glibc2.12-x86_64 mysql**

**创建data数据库包**
data是用来存放数据库文件的，数据库的表文件都放在data目录下
linux的mysql没有默认的data目录，需要手动创建

> **进入mysql目录：cd mysql**
> **创建目录：mkdir data**

**创建用来管理mysqld命令的linux用户**

> **先创建组：groupadd mysql**
> **创建专门用来执行mysql的用户：useradd -g mysql mysql**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c15385ecf6f34dcfa85e536933c87b9b.png)
> **先进入mysql的bin目录：cd**
> **初始化mysql(设置默认数据库)：**
> **./mysqld --initialize --user=mysql --datadir=/opt/mysql/data --basedir=/opt/mysql**
> **初始化完之后会显示：**
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f640769eb69942c587cac1616033f7e4.png)
**将最后一行root@localhost:&rfsJEOIl0#S
这一段的内容保存一下[这是mysql初始化的密码]**

> **设置安全功能：./mysql_ssl_rsa_setup --datadir=/opt/mysql/data**
> **修改mysql安装目录的权限(需要在opt目录下执行)：chown -R mysql:mysql /opt/mysql/**
> **给mysql目录授权：chmod 777 /opt/mysql/**

**启动mysql服务：**

> 进入bin目录：cd /opt/mysql/bin 
> 启动服务：./mysqld_safe & 
> 加个&是让服务后台运行
> 查看mysql进程：ps -ef|grep mysql
![在这里插入图片描述](https://img-blog.csdnimg.cn/ac6989f8a0be4528889a5c4f0ee7dd9d.png)

**打开客户端mysql**
>**进入bin目录：cd /opt/mysql/bin**
>**输入./mysql -uroot -p**
>**密码就是上面root@localhost后面的这就是密码：&rfsJEOIl0#S**
>![在这里插入图片描述](https://img-blog.csdnimg.cn/e1c83c76b0e2488b9fdfc4dfb16bdeaa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

>**进入之后输入修改密码:alter user '用户名'@'localhost' identified by '新密码'**
>**alter user 'root'@'localhost' identified by '123465'**
>**之后就可以正常使用mysql了**

**解决mysql报错**
两个报错统计：
**mysql时报错：mysql: error while loading shared libraries: libncurses.so.5: cannot open shared object fil**
以上报错用Xftp打开目录usr-lib64搜索libncurses
![在这里插入图片描述](https://img-blog.csdnimg.cn/65041a371b26482f9a078ba68402270d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
将快捷方式libncurses.so.6改成libncurses.so.5就可以
**mysql: error while loading shared libraries: libtinfo.so.5**
以上报错需要用Xftp进入到usr-lib64目录下搜索libtinfo
![在这里插入图片描述](https://img-blog.csdnimg.cn/0ee45413ed9a40088190780c9026ca0b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
将快捷方式libtinfo.so.6改成libtinfo.so.5即可上图是已更改
**关闭mysql服务**
关闭mysql需要进入/opt/mysql/bin目录下

> **输入命令：./mysqladmin -uroot -p shutdown**
> **输入完之后需要输入mysql的密码即可关闭**

**从windows连接linux的mysql数据库**
首先进入linux的mysql数据库客户端：

> **关闭防火墙：systemctl stop firewalld**
> **进入mysql的bin目录：cd /opt/mysql/bin**
> **启动mysql服务：./mysqld_safe &**
> **登陆：./mysql -uroot -p密码**
> **创建远程连接的用户：create user '用户名'@'%' identified by '密码';**
> 比如：create user 'root'@'%' identified by '123456';
> **赋予权限：grant all on *.* to '用户名'@'%' with grant option;**
> grant后面的all表示所有权限，也可以自己指定权限
> 其中*.* 的第一个*表示所有数据库名，第二个*表示所有的数据库表；
> 用户名@'%' %表示ip地址，%也可以指定具体的ip地址
![在这里插入图片描述](https://img-blog.csdnimg.cn/04ec24f98d7641d2ae5794a4a85a7792.png)

**打开windows的Navicat for Mysql（连接mysql的工具，其他工具也可以）**
![在这里插入图片描述](https://img-blog.csdnimg.cn/83699ad610024562a175d9c11c64a676.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/de518245abee42eea3617247f0f36b05.png)
**完成**
