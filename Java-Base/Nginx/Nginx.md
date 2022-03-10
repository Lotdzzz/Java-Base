@[TOC](目录)
# Nginx介绍

> Nginx是一个高性能的Web服务器和反向代理服务器
> Nginx只能做静态资源的操作
> 可以做到资源的动静分离
> 可以分配多个tomcat服务器的请求，负载均衡
> 有静态代理的功能
> 可以做虚拟主机场景
> Nginx默认端口：80
# Nginx安装与基本命令
> Nginx官方网站：[Nginx下载](https://nginx.org/en/download.html)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0cac8cbb8a7b4a1e9d02d4691f85b621.png)
> 将下载包.tar.gz放到linx环境下
> 解压：tar -zxvf nginx-1.20.2.tar.gz
> 执行命令：yum -y install pcre-devel
> 执行命令：yum -y install openssl openssl-devel
> 进入到nginx安装目录下执行：./configure
> 执行命令编译：make
> 执行命令：make install
> 此时会在指定目录下生成nginx目录
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/7970fe9d25034099906656c490e736e1.png)
> conf目录——配置文件目录（conf内的nginx.conf是nginx的主配置文件）
> html目录——欢迎首页面和错误信息页面
> logs目录——日志文件
> sbin目录——启动nginx的文件
> ****
> 启动Nginx命令（到sbin目录下）：./nginx
> 指定配置文件启动Nginx（配置文件要是绝对路径）：./nginx -c /opt/nginx/conf/nginx.conf
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/2ccc625fff524b3d94693e7ea9a6b465.png)
> 关闭防火墙：systemctl stop firewalld
> 从windows浏览器访问
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/e527a12af8bb4490a19ff814430cdc71.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ****
> 关闭Nginx命令：kill -TERM nginx的pid
> 另一种方式：kill -QUIT pid
> ****
> 重启nginx：./nginx -s reload
> ****
> 配置检查（检查nginx配置文件是否正确能用）：./nginx -c /opt/nginx/conf/nginx.conf -t
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/8f4b39c5713844e8a54d57b78a248257.png)
# Nginx配置文件
> 配置文件位置conf/nginx.conf

```c
#配置worker进程运行用户，nobody也是一个linux用户，一般用于启动程序，没有密码
#定义Nginx运行的用户和用户组
#user  nobody;

#nginx进程数，建议设置为等于CPU总核心数,默认为1。
worker_processes  1;

#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#进程pid文件,指定nginx进程运行文件存放地址
#pid        logs/nginx.pid;


# 工作模式与连接数上限
# 这个指令是指当一个nginx进程打开的最多文件描述符数目
# worker_connections最大值：65535。
#这是因为nginx调度时分配请求到进程并不是那么的均衡，所以假如填写10240，总并发量达到3-4 万时就有进程可能超过10240了，这时会返回502错误。
events {
    worker_connections  1024;
}

#设定http服务器，利用它的反向代理功能提供负载均衡支持
http {

	#文件扩展名与文件类型映射表的文件
    include       mime.types;
    
    #默认文件类型
    default_type  application/octet-stream;
    
	#配置日志文件的格式
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    
	#配置access_log日志及存放路径，并使用main的日志格式
    #access_log  logs/access.log  main;

    sendfile        on;		#开启高效文件传输模式
    #tcp_nopush     on;		#防止网络阻塞

    #keepalive_timeout  0;
    keepalive_timeout  65;		#长连接超时时间，单位是秒

    #gzip  on;		#开启gzip压缩输出

	#配置虚拟主机
    server {
    	#这两个保证唯一性就可以
        listen       80;     #配置监听端口，多个主机端口可以一样，但服务名不能相同
        server_name  localhost;		#配置服务名，多个主机服务名可以相同，但端口号不能相同

        #charset koi8-r;	#配置字符集

        #access_log  logs/host.access.log  main;	#配置本虚拟机的访问日志

		#请求拦截，/表示地址栏中的根路径（localhost:80），会被location拦截
        location / {
        	#root表示本地磁盘的网站根路径，默认为nginx目录下的html目录
            root   html;
            #配置首页文件的名称
            index  index.html index.htm;
        }
        
        #精准拦截，当出现访问（根路径/50x.html）的时候会把请求转到html目录中
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }

}
```
# Nginx静态网站部署
> Nginx是一个Http的web服务器，可以将服务器上的静态文件通过http协议返回给浏览器客户端

> 修改nginx.conf配置文件增加访问路径
```c
server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        location /crm {
            root   /opt/project;
            index  index.html;
        }
        
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
***文件目录结构（/opt/project）（location /crm）：***
![在这里插入图片描述](https://img-blog.csdnimg.cn/222fdf8603304a86932c518a32ba7a60.png)
***修改nginx.conf之后需要重载Nginx：./nginx -s reload***
# Nginx负载均衡
> 如果浏览器访问tomcat会出现请求上限，过量的请求需要等待
> 而负载均衡是配置多个相同的tomcat，tomcat的入口由nginx管理

> nginx.conf配置文件（假如部署多个tomcat服务器）
```c
#upstream www.填写的域名.com
upstream www.crm.com {

	#tomcat的服务器ip+端口
	#被拦截的请求会转发到这里，而请求会被分配到以下的服务器上，按照个数的顺序分配
	#这种处理方式被称为：轮询策略，像for循环一样分配请求，要求：服务器的处理速度性能要相同

	server 192.168.73.132:8080;
	server 192.168.73.132:8081;
	server 192.168.73.132:8082;
	server 192.168.73.132:8083;
}

server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        location /crm {
        	#www.域名.com，域名可以自定义
            proxy_pass http://www.crm.com;
        }
        
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
> 权重模式
```c
#upstream www.填写的域名.com
upstream www.crm.com {

	#tomcat的服务器ip+端口
	#权重模式下，每个请求按照一定比例分配，并不是第一个服务器发五个请求第二个发两个请求
	#分配数量会根据weight值而变化，用于后端服务器性能不均衡的情况

	server 192.168.73.132:8080 weitht=5;
	server 192.168.73.132:8081 weitht=2;
	server 192.168.73.132:8082 weitht=3;
	server 192.168.73.132:8083 weitht=3;
}
```
> 负载均衡策略——ip_hash
> ip_hash是ip锁定，每个请求按照访问的ip的hash值分配，这样每个客户端会固定访问一个服务器
> 可以解决session丢失问题（如果轮询分配的话，登陆时算一个请求会被分配到不同的服务器引起session丢失）
```c
#upstream www.填写的域名.com
upstream www.crm.com {

	#添加哈希运算，如果是哈希运算那么一定有哈希碰撞问题（一个客户端可能会永远在一个服务器内，如果多个ip都固定，导致宕机）
	ip_hash;

	#tomcat的服务器ip+端口
	server 192.168.73.132:8080;
	server 192.168.73.132:8081;
	server 192.168.73.132:8082;
	server 192.168.73.132:8083;
}
```
> 最少连接——least_conn
```c
#upstream www.填写的域名.com
upstream www.crm.com {
	
	#请求会被转发到连接数最少的服务器上
	least_conn;

	#tomcat的服务器ip+端口
	server 192.168.73.132:8080;
	server 192.168.73.132:8081;
	server 192.168.73.132:8082;
	server 192.168.73.132:8083;
}
```
> 其他配置
```c
upstream www.crm.com {

	server 192.168.73.132:8080;
	server 192.168.73.132:8081;
	server 192.168.73.132:8082;
	server 192.168.73.132:8083;
	
	#当其他服务器都停止或down之后才会访问这个服务器，比如一些服务器更新的时候会停止其他服务器
	server 192.168.73.132:8084 backup;
}
```
```c
upstream www.crm.com {

	server 192.168.73.132:8080;
	server 192.168.73.132:8081;
	server 192.168.73.132:8082;
	server 192.168.73.132:8083 down; #down表示这个服务器不参与负载均衡
}
```
# Nginx静态代理
> Nginx静态代理就是把服务器中的静态资源抽离出，由Nginx管理

> 代理方式：在配置文件中设置静态资源的后缀和路径（使用正则表达式）
```c
server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

		#使用正则表达式，内容是被代理的静态资源的后缀名
        location ~ .*\.(js|css|html|htm|gif|jpg)$ {
        	#被代理后nginx管理的静态资源位置
        	root /opt/static
        }

		#使用正则表达式，内容是被代理的静态资源的路径
        location ~ .*/(css|html|images) {
        	#被代理后nginx管理的静态资源位置
        	root /opt/static
        }
        
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
# Nginx动静分离
> Nginx的负载均衡和静态代理结合在一起，可以实现动静分离
> 动态资源：如jsp等由tomcat服务器处理
> 静态资源：如图片,css,js等由nginx服务器完成

> 准备负载均衡
```c
upstream www.crm.com {
	ip_hash;
	server 192.168.73.132:8080;
	server 192.168.73.132:8081;
}

upstream static.crm.com {
	server 192.168.73.132:81;
	server 192.168.73.132:82;
}

server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

		#配置负载均衡
        location /crm {
            proxy_pass http://www.crm.com;
        }

		#配置动静分离
		location ~ .*/(css|html|images) {
			proxy_pass http://static.crm.com;
		}
        
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
> 准备另外的nginx，准备动静分离，修改端口
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/5e6bd69a79314c5c9c920696073d9c2d.png)
```c
server {
        listen       81;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }
        
        location ~ .*/(css|html|images) {
        	root /opt/static;
        }
        
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
# Nginx虚拟主机
> 把一台服务器划分多个虚拟的服务器，可以一台服务器当多个服务器使用
> 从而可以配置多个网站

> 配置方式：基于域名
```c
    upstream www.first.com {
        server 192.168.73.132:8080;
    }

    upstream www.two.com {
        server 192.168.73.132:8081;
    }

    server {
        listen       80;
        server_name  www.first.com;
   
        location / {
            proxy_pass http://www.first.com;
        }
     }

    server {
        listen       80;
        server_name  www.two.com;
   
        location / {
            proxy_pass http://www.two.com;
        }
     }
```

> 在windows中配置域名
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/274333ccb1c34b719040129b6747f7cc.png)
```c
添加：
192.168.73.132 www.first.com
192.168.73.132 www.two.com
```
# Session丢失问题概述
> Session存储过程
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/95210a10a6f841c78eb13a3188f449af.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> Session丢失原因

![在这里插入图片描述](https://img-blog.csdnimg.cn/ed9f5fa793bb4e0f91b14db334ac9fb6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

> 关键问题在JVM中，SessionID会不停的让浏览器更新更换，从而导致服务器内的Session找不到对应的SessionID
# SpringSession解决丢失
> 解决方案，使用Redis实现SessionID的共享

![在这里插入图片描述](https://img-blog.csdnimg.cn/d3ad7ffbecfd49f2a3d23a8fa38abf0d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> SpringBoot集成SpringSession
> 创建SpringBoot项目，选中依赖：Spring Web，Spring Session，Spring DataRedis（Access+Driver）

> 依赖
```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
```
> 在application配置文件中配置SpringSession
```c
server.port=8080

#配置Redis
spring.redis.port=6379
spring.redis.host=192.168.73.132

#设置SpringSession声明周期 30m表示30分钟
server.servlet.session.timeout=30m
#指定Cookie的存放路径为根路径，实现同域名不同项目的session共享
server.servlet.session.cookie.path=/
#指定Cookie的存放域名，用于实现同根域名不同二级子域名的session共享
server.servlet.session.cookie.domain=project.com
```

