@[TOC](目录)
# Docker介绍

> Docker容器引擎是基于虚拟花技术上升级的一个操作系统级虚拟化
> 是运行在一个系统上不同进程，并将这些进程封装在一个容器内
> 被称之为容器技术（Docker是其中之一）
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d79fd2eed2954ba5a4f7b5bca058ba85.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
# Docker安装
> 查看Docker是否安装：yum list installed | grep docker
> 安装依赖：yum install yum-utils
> 安装资源：yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
> 安装资源：dnf install https://mirrors.aliyun.com/docker-ce/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.13-3.1.el7.x86_64.rpm
> ****
> 安装docker命令：yum install --allowerasing docker-ce
> 卸载docker命令：yum remove docker
> 查看安装：docker -v

> docker启动服务命令：systemctl start docker 或者 service docker start
> docker关闭服务命令：systemctl start docker 或者 service docker stop
> docker重启服务命令：systemctl restart docker 或者 service docker restart
> 查看docker运行状态：systemctl status docker
> 查看docker系统信息：docker info
# Docker使用
> docker不是容器，而是容器管理的引擎

> docker运行机制（得到服务----下载镜像----启动该镜像的容器----容器运行下载的镜像程序）
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/b2c5240ad0214b5f8375313f60e32882.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

> 搜索镜像命令：docker search 镜像名（tomcat）
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/5ca6f4c9d72e43c8a5a6e72bead26deb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> 下载镜像：docker pull 镜像名tomcat
> 运行镜像：docker run tomcat -d
> 显示本地镜像：docker images

> 启动镜像程序后需要和本地系统做映射
> 停止容器：docker ps（这个命令之后会有容器的names）
> 停止：docker stop 容器的names
> 本地系统映射：docker -d -p 8080:8080 tomcat
> 作用是用tomcat的8080映射到本地的8080端口让外网可以访问

> 进入容器内命令：docker exec -it 容器的names bash
# Docker核心组件
> Docker分为三个核心要素：镜像，容器，仓库

## 命令操作

> 镜像是一个只读的模板，可以用来创建Docker容器
> 一个镜像只包含一个完整的centos操作系统环境，这个环境只安装了用户需求的应用程序
> ****
> 镜像的组成结构
> 镜像是由许多层文件系统叠加构成的
> 最下面是一个引导文件系统的bootfs
> 第二层是一个root文件系统
> root文件系统通常是某种操作系统，centos等
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/4a965d03169b42c883cd344ffc414b5f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_19,color_FFFFFF,t_70,g_se,x_16)

> 镜像的操作命令
> 下载镜像：docker pull 镜像名:版本号
> 查看已下载的镜像：docker images 【镜像名】
> 运行镜像：docker run 镜像名
> 删除镜像：docker rmi 镜像名
> 删除容器：docker rm 容器names
> 查看容器信息：docker inspect 容器names
> 停用全部容器：docker stop ${docker ps -q}
> 删除全部容器：docker rm ${docker ps -aq}
> 停用并删除所有容器：docker stop ${docker ps -q} & docker rm ${docker ps -aq}

> 安装使用Mysql
> docker pull mysql 
> docker run -p 3306:3306 -e MYSQL_DATABASE=database -e MYSQL_ROOT_PASSWORD=123456 -d mysql
> -p指定端口 mysql端口3306映射本机端口3306
> -e指定环境变量 DATABASE表示创建数据库
> ROOT_PASSWORD表示root账户的密码设置为123456
> -d 后台启动mysql
> 进入mysql容器内：docker exec -it 容器names bash
> 登陆mysql：mysql -uroot -p123456

> 将linux的文件移动到容器内：docker cp linux内的文件的路径文件名 容器names:容器内的路径目录名
# 自定义镜像
> Dockerfile用于构建docker镜像，Dockerfile文件是由多行命令语句组成
> ****
> Dockerfile分为四部分
> 基础镜像信息
> 维护者信息
> 镜像操作信息
> 容器启动时执行命令

> 自定义jdk镜像
```bash
FROM centos:latest
MAINTAINER MyOS
ADD jdk-8u321-linux-x64.tar.gz /usr/local/
ENV JAVA_HOME=/usr/local/jdk1.8.0_321
ENV PATH=$JAVA_HOME/bin:$PATH
ENV CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
CMD java -version
```
> FROM（表示继承的镜像）
MAINTAINER（指定维护者，作者）
ADD （将一个文件复制到容器内的路径目录中）
ENV（配置环境变量）
EXPOSE（告诉docker服务容器暴露的端口号比如tomcat的8080，在启动时需要-p映射端口）
EUN（在当前镜像基础上执行指定命令，并提交为新的镜像，当命令较长可以用\来换行）
CMD（启动容器时执行命令，只能执行一个cmd命令）
> ****
> 构成镜像：docker build -t MyOS .
> -t给镜像起的名字
> .表示当前目录下有个Dockerfile文件

> 阿里云镜像仓库地址：[dev.aliyun.com](https://www.aliyun.com/product/acr?spm=5176.23056729.J_2110863300.6.3dcc3f06vXRXiu)
# Docker部署SpringBoot项目
> 在自己的当前的目录下有SpringBoot项目的jar包

> 使用自定义镜像的方法
```bash
FROM java
MAINTAINER spring-java
ADD spring-1.8.0.jar /opt
RUN chmod +x/opt/spring-1.8.0.jar
CMD java -jar /opt/spring-1.8.0.jar
```

> 保存新镜像
> 让镜像持久化
> 命令：docker commit 镜像id 新镜像的名字
> 让容器内的数据得到保存

