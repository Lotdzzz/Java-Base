@[TOC](目录)
# FastDFS介绍
> 传统文件管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/14e789d868974e66904432444ea6a029.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_17,color_FFFFFF,t_70,g_se,x_16)
> 分布式文件系统

![在这里插入图片描述](https://img-blog.csdnimg.cn/1d70cd07546346eebbfab6c5f8570322.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d54a904aee2647adb517c3bda6d8c7ca.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)

> FastDFS是一个分布式文件系统（是被C语言开发的）
> github地址：[FastDFS](https://github.com/happyfish100)
> ****
> FastDFS是由客户端（代码），服务器端组成
> 服务端由两个部分组成，跟踪器（tracker），存储节点（storage）
> 跟踪器主要做调度工作，在内存中记录集群中存储节点的状态信息
> 存储节点用于存储文件，包括文件和文件属性，都保存到存储服务器磁盘上
> 完成文件管理，文件同步，访问等
# FastDFS安装
> Linux系统中确认是否安装了GCC，libevent，libevent-devel
> 查看是否安装命令：
> yum list installed | grep gcc
> yum list installed | grep libevent
> yum list installed | grep libevent-devel
> 安装命令：
> yum install gcc libevent libevent-devel -y
> 到官网下载以下：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/cdd91ad05f6f489c956b220e20a23cbf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d6e6fad8b974446bd4e8309d0d4138c.png)
> 发送到Linux系统中
![在这里插入图片描述](https://img-blog.csdnimg.cn/9a18e563fd1a48d6bf51652c56797399.png)
> 安装公共函数库：tar -zxvf libfastcommon-1.0.54.tar.gz
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/7af359e08100449693de991a7801e9a1.png)
> 到目录中输入：./make.sh
> 编译完输入：./make.sh install
> 返回上级目录解压FastDFS：tar -zxvf fastdfs
> 进入目录执行编译：./make.sh
> 编译完成：./make.sh install
> ****
> 在/etc/fdfs/中有四个文件
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/6e2685a94f0f41818f509f2573cf7188.png)
> 存放的是fastdfs配置文件的
> 将安装的fastdfs中的conf目录下的http.conf和mime.types拷贝到/etc/fdfs目录中
> cp http.conf /etc/fdfs/
> cp mime.types /etc/fdfs/
> ****
> 配置文件修改
> 将etc的fdfs目录下的storage.conf.sample和tracker.conf.sample修改扩展名为conf
> tracker配置文件下的base_path=/opt/fastdfs/tracker（必须确保文件存在）
> storage配置文件下的base_path=/opt/fastdfs/storage
> storage配置文件下的store_path0 = /opt/fastdfs/storage/files
> storage配置文件下的tracker_server = 192.168.73.132:22122（修改为自己linux的ip）
> 注：client.conf也需要修改
> ****
> 创建文件夹
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/9b29abd632e44fe3ae2075c5b621aaec.png)
> mkdir /opt/fastdfs/storage/files
> ****
> 启动服务：
> 在fdfs目录下指定配置文件名：fdfs_trackerd /etc/fdfs/tracker.conf
> fdfs_storaged /etc/fdfs/storage.conf
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/57b1fcb23f804f8da09cd887ae127bda.png)
> 在files/data目录下会创建256*256个（65535）文件夹来存储数据
> ****
> 关闭fastdfs服务：fdfs_trackerd stop
> 关闭fastdfs服务：fdfs_storaged stop
> ****
> 重启：fdfs_trackerd restart
> fdfs_storaged restart
# FastDFS使用
> 测试上传
> 命令：fdfs_test client.conf upload 文件名
> 上传之后会显示：
```c
This is FastDFS client test program v6.07

Copyright (C) 2008, Happy Fish / YuQing

FastDFS may be copied only under the terms of the GNU General
Public License V3, which may be found in the FastDFS source kit.
Please visit the FastDFS Home Page http://www.fastken.com/ 
for more detail.

[2022-01-07 18:35:12] DEBUG - base_path=/opt/fastdfs/client, connect_timeout=5, network_timeout=60, tracker_server_count=1, anti_steal_token=0, anti_steal_secret_key length=0, use_connection_pool=0, g_connection_pool_max_idle_time=3600s, use_storage_id=0, storage server id count: 0

tracker_query_storage_store_list_without_group: 
	server 1. group_name=, ip_addr=192.168.73.132, port=23000

#group_name属于组名
group_name=group1, ip_addr=192.168.73.132, port=23000
storage_upload_by_filename

#M00是第一块磁盘 00 00是files文件内的00的文件夹
#M00表示是storage.conf中的store_path0=的路径加上files

#remote_filename表示远程文件名，下载时用到

group_name=group1, remote_filename=M00/00/00/wKhJhGHYF2CAbacYAAAADLCV5eM801.txt
source ip address: 192.168.73.132
file timestamp=2022-01-07 18:35:12
file size=12
file crc32=2962613731
example file url: http://192.168.73.132/group1/M00/00/00/wKhJhGHYF2CAbacYAAAADLCV5eM801.txt
storage_upload_slave_by_filename
group_name=group1, remote_filename=M00/00/00/wKhJhGHYF2CAbacYAAAADLCV5eM801_big.txt
source ip address: 192.168.73.132
file timestamp=2022-01-07 18:35:12
file size=12
file crc32=2962613731
example file url: http://192.168.73.132/group1/M00/00/00/wKhJhGHYF2CAbacYAAAADLCV5eM801_big.txt
```
> 存储的文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/cce49594f2024624a5e4a14b1ad540e4.png)
> _big表示是备份文件，-m表示属性文件

> 测试下载到当前目录下
> 命令：fdfs_test client.conf download group1 M00/00/00/wKhJhGHYF2CAbacYAAAADLCV5eM801.txt
> 下载成功提示
```c
This is FastDFS client test program v6.07

Copyright (C) 2008, Happy Fish / YuQing

FastDFS may be copied only under the terms of the GNU General
Public License V3, which may be found in the FastDFS source kit.
Please visit the FastDFS Home Page http://www.fastken.com/ 
for more detail.

[2022-01-07 18:47:03] DEBUG - base_path=/opt/fastdfs/client, connect_timeout=5, network_timeout=60, tracker_server_count=1, anti_steal_token=0, anti_steal_secret_key length=0, use_connection_pool=0, g_connection_pool_max_idle_time=3600s, use_storage_id=0, storage server id count: 0

storage=192.168.73.132:23000
download file success, file size=12, file save to wKhJhGHYF2CAbacYAAAADLCV5eM801.txt
```
> 删除文件命令：
> fdfs_test client.conf group1 M00/00/00/wKhJhGHYF2CAbacYAAAADLCV5eM801.txt
> 删除后只剩备份文件

> 安装Nginx插件
> 上传包
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f35c03c08cb04f70a49e1a5822e69aaa.png)
> 解压：unzip fastdfs-nginx-module-master.zip
> 进入到之前安装的Nginx-版本号的目录下
> ./configure --prefix=安装到的路径 --add-module=插件位置
> 命令：./configure --prefix=/opt/software/nginx-fdfs --add-module=/opt/software/fastdfs-nginx-module-master/src
> 命令：make install
> ****
> 在插件模块目录下的mod_fastdfs.conf
> 修改base_path=/opt/fastdfs/nginx_fdfs
> tracker_server=192.168.73.132:22122
> url_have_group_name = true
> store_path0=/opt/fastdfs/storage/files
> 将这个文件上传到/etc/fdfs目录
> 创建配置中的目录/opt/fastdfs/nginx_fdfs
> ****
> 到nginx_fdfs目录中编辑nginx.conf文件
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

		location ~ /group[1-9]/M0[0-9] {
			ngx_fastdfs_module;
		}
```

> 启动Nginx（nginx_dfds的sbin目录下）：
> ./nginx -c /opt/software/nginx-fdfs/conf/nginx.conf

> 扩展模块执行流程
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0806dbb25676419b8028455e7e7cdb65.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
# Web工程操作FastDFS
> 创建项目添加依赖选项：
> Spring Boot DevTools
> Spring Web
> Thymeleaf
> Mybatis Framework
> MySql Driver

![在这里插入图片描述](https://img-blog.csdnimg.cn/d069b46bd47a4900aaf1d454f7aae531.png)
> 从github下载源码进行编译成jar包
> 打包后引入依赖
```html
<dependency>
    <groupId>org.csource</groupId>
    <artifactId>fastdfs-client-java</artifactId>
    <version>1.28-SNAPSHOT</version>
</dependency>
```
> 上传文件时Thymeleaf模板要求
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <!--上传文件的请求方式必须是post 必须写enctype为multipart/form-data以方便用二进制流传输数据-->
    <form th:action="@{/upload}" method="post" enctype="multipart/form-data">
        编号<span th:text="1"></span><br>
        姓名<span th:text="ccxg"></span><br>
        性别<span th:text="女"></span><br>
        <input type="file" name="myFile"/><br>
        <input type="submit">
    </form>
</body>
</html>
```
> resources目录创建配置文件fastdfs.conf（配置tracker的ip与端口号）
```c
tracker_server=192.168.73.132:22122
```
> 文件上传操作（纯演示）
```java
@Controller
public class FileController {

    @RequestMapping(value = "/upload")
    public String upload(MultipartFile myFile, Model model) throws IOException {
        //获取文件信息
        System.out.println(myFile.getName());
        //获取文件字节数组
        System.out.println(myFile.getBytes());

        StorageServer ss = null;
        TrackerServer ts = null;
        try {
            //读取FastDFS的配置文件用于将所有的tracker的地址读取到内存中
            ClientGlobal.init("fastdfs.conf");
            TrackerClient tc = new TrackerClient();
            ts = tc.getTrackerServer();
            ss = tc.getStoreStorage(ts);
            //定义Storage的客户端对象，需要使用这个对象来完成具体的文件上传下载和删除
            StorageClient sc = new StorageClient(ts,ss);
            //上传文件
            //参数1是需要上传的文件的字节数组
            //参数2为需要上传的文件的扩展名
            //参数3为文件的属性文件通常不上传
            //返回一个String[]数组，数组中第一个元素是文件所在的组名group，第二个元素是文件的远程路径名
            //获取文件的扩展名
            String fileExtName = myFile.getOriginalFilename().substring(myFile.getOriginalFilename().lastIndexOf(".")+1);
            String[] result = sc.upload_file(myFile.getBytes(),fileExtName,null);
            System.out.println(result[0]);
            System.out.println(result[1]);
        } catch (MyException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2261d47595644d09bb0be4c9690b7522.png)
> 文件下载（通过请求可以让浏览器下载文件）
```java
@Controller
public class FileController {

    @RequestMapping(value = "/download")
    /*
    * 完成浏览器文件下载
    * 返回值是ResponseEntity响应实体，这个类是框架的类
    * 这个对象包含响应状态码，以及响应头，响应体
    * 这个响应数据可以是html代码 js 字符串 文件流等
    * */
    public ResponseEntity<byte[]> download(){

        StorageServer ss = null;
        TrackerServer ts = null;
        byte[] result = null;
        try {
            ClientGlobal.init("fastdfs.conf");
            TrackerClient tc = new TrackerClient();
            ts = tc.getTrackerServer();
            ss = tc.getStoreStorage(ts);
            StorageClient sc = new StorageClient(ts,ss);

            //下载文件的方法
            //参数1 组名
            //参数2 远程文件名
            //返回值为字节数组byte[]
            result = sc.download_file("group1","M00/00/00/wKhJhGHZHm2AEBoMAAAAC0oXsVY358.txt");

        } catch (MyException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        //创建响应实体的对象，spring会将这个对象返回浏览器当做响应信息
        //参数1 为 响应时的具体数据
        //参数2 为 响应时的头文件信息
        //参数3 为 响应时的状态码
        HttpHeaders httpHeaders = new HttpHeaders();//获取响应头信息
        httpHeaders.setContentType(MediaType.APPLICATION_OCTET_STREAM);//设置响应类型为文件类型
        httpHeaders.setContentLength(11);//设置文件大小（字节）
        httpHeaders.setContentDispositionFormData("attachment","helloworld.txt");//设置下载时的默认文件名，第一个固定参数，第二个为文件名+后缀名
        ResponseEntity<byte[]> responseBody = new ResponseEntity<byte[]>(result,httpHeaders,HttpStatus.OK);
        return responseBody;
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b09400340fb4b9baddb94df56187e2c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> 文件删除操作
```java
@RequestMapping(value = "/delete")
    public void delete(){
        StorageServer ss = null;
        TrackerServer ts = null;
        try {
            ClientGlobal.init("fastdfs.conf");
            TrackerClient tc = new TrackerClient();
            ts = tc.getTrackerServer();
            ss = tc.getStoreStorage(ts);
            StorageClient sc = new StorageClient(ts,ss);
            //删除文件
            //delete_file方法只需要指定组名和远程文件名即可
            int count = sc.delete_file("group1","wKhJhGHYF2CAbacYAAAADLCV5eM801_big.txt");
            System.out.println(count);
        } catch (MyException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
# 集群环境搭建
```bash
FastDFS分布式文件系统集群环境搭建-操作步骤手册

搭建一个FastDFS分布式文件系统集群，推荐至少部署6个服务器节点；
================================搭建FastDFS的集群==============================
第一步：安装6个迷你版的Linux，迷你版Linux没有图形界面，占用磁盘及资源小，企业里面使用的Linux都是没有图形界面的Linux；

第二步：由于迷你版Linux缺少一些常用的工具库，操作起来不方便，推荐安装如下的工具库：
1、安装lrzsz， yum install lrzsz -y
2、安装wget, yum install wget -y
4、安装vim， yum install vim -y
5、安装unzip，yum install unzip -y
6、安装ifconfig，yum install net-tools -y

yum install lrzsz wget vim unzip net-tools -y

7、安装nginx及fastdfs需要的库依赖：
   yum install gcc perl openssl openssl-devel pcre pcre-devel zlib zlib-devel libevent libevent-devel -y

第三步 安装fastdfs 
   1、 上传fastdfs的安装包和libfastcommon的安装包
   2、 解压libfastcommon 安装libfastcommon
   3、 解压fastdfs 安装fastdfs
   4、 拷贝fastdfs目录中的http.conf和mime.types到/etc/fdfs 目录中
注：6台机器全部执行这些操作


第四步：部署两个tracker server服务器，需要做的工作:

    修改两个tracker服务器的配置文件：
    tracker.conf: 修改一个地方：
    base_path=/opt/fastdfs/tracker   #设置tracker的数据文件和日志目录（需预先创建）
    启动tracker服务器 fdfs_trackerd /etc/fdfs/tracker.conf


第五步 修改两个组中的4台storage中storage.conf文件
    第一组group1的第一个storage server（修改storage.conf配置文件）：
    group_name=group1   #组名，根据实际情况修改，值为 group1 或 group2
    base_path=/opt/fastdfs/storage   #设置storage的日志目录（需预先创建）
    store_path0=/opt/fastdfs/storage/files    #存储路径
    tracker_server=192.168.171.135:22122  #tracker服务器的IP地址以及端口号
    tracker_server=192.168.171.136:22122

    第二组group2的第一个storage server（修改storage.conf配置文件）：
    group_name=group2   #组名，根据实际情况修改，值为 group1 或 group2
    base_path=/opt/fastdfs/storage   #设置storage的日志目录（需预先创建）
    store_path0=/opt/fastdfs/storage/files    #存储路径
    tracker_server=192.168.171.135:22122  #tracker服务器的IP地址以及端口号
    tracker_server=192.168.171.136:22122
   
    启动storage服务器
    使用之前的Java代码测试FastDFS的6台机器是否可以上传文件
注意：FastDFS默认是带有负载均衡策略的可以在tracker的2台机器中修改tracker.conf文件
    store_lookup=1

    0 随机存放策略
    1 指定组
    2 选择磁盘空间的优先存放 默认值

    修改后重启服务
    fdfs_trackerd /etc/fdfs/tracker.conf restart


======================使用Nginx进行负载均衡==============================


第六步 安装 nginx ，使用nginx 对fastdfs 进行负载均衡 
    上传 nginx-1.12.2.tar.gz以及 nginx的fastdfs扩展模块安装包fastdfs-nginx-module-master.zip
    添加nginx的安装依赖
       yum install gcc openssl openssl-devel pcre pcre-devel zlib zlib-devel -y
    解压nginx
       tar -zxvf  nginx-1.12.2.tar.gz
    解压fastdfs扩展模块
       unzip fastdfs-nginx-module-master.zip
    配置nginx的安装信息
       2台tracker服务器的配置信息（不需要fastdfs模块）
         ./configure --prefix=/usr/local/nginx_fdfs
       4台storage服务器其的配置信息（需要使用fastdfs模块）
         ./configure --prefix=/usr/local/nginx_fdfs --add-module=/root/fastdfs-nginx-module-master/src
    编译并安装nginx
       ./make
       ./make install

    4台storage的服务器需要拷贝mod_fastdfs文件
    将/root/fastdfs-nginx-module-master/src目录下的mod_fastdfs.conf文件拷贝到 /etc/fdfs/目录下，这样才能正常启动Nginx；



第七步 配置tracker 的两台机器的nginx
    进入安装目录
    cd /usr/local/nginx_fdfs

    添加一个location 对请求进行拦截，配置一个正则规则 拦截fastdfs的文件路径， 并将请求转发到其余的4台storage服务器(修改 conf目录下nginx.conf 文件)
    #nginx拦截请求路径：
    location ~ /group[1-9]/M0[0-9] {   
        proxy_pass http://fastdfs_group_server; 
    }



    添加一个upstream 执行服务的IP为 另外的4台stroage 的地址
    #部署配置nginx负载均衡:
    upstream fastdfs_group_server {  
        server 192.168.171.137:80;  
        server 192.168.171.138:80;
        server 192.168.171.139:80;  
        server 192.168.171.140:80;  
    }


第八步 配置另外4台storage的nginx添加http访问的请求路径拦截
    进入安装目录
    cd /usr/local/nginx_fdfs
    添加一个location 对请求进行拦截，配置一个正则规则 拦截fastdfs的文件路径，使用fastdfs的nginx模块转发请求(修改 conf目录下nginx.conf 文件)
    #nginx拦截请求路径：
    location ~ /group[1-9]/M0[0-9] {   
        ngx_fastdfs_module;
    }



第九步 分别修改4台storage服务器的mod_fasfdfs.conf文件（/etc/fdfs/mod_fastdfs.conf）
    #修改基本路径，并在指定路径创建对应文件夹
    base_path=/opt/fastdfs/nginx_mod #保存日志目录
    #指定两台tracker服务器的ip和端口
    tracker_server=192.168.171.135:22122  #tracker服务器的IP地址以及端口号
    tracker_server=192.168.171.136:22122
    #指定storage服务器的端口号
    storage_server_port=23000 #通常情况不需要修改
    #指定当前的storage服务器所属的组名 （当前案例03和04为group1 05和06为group2）
    group_name=group1  #当前服务器的group名
    #指定url路径中是否包含组名 （当前案例url包含组名）
    url_have_group_name=true     #文件url中是否有group名
    store_path_count=1           #存储路径个数，需要和store_path个数匹配（一般不用改）
    store_path0=/opt/fastdfs/storage/files    #存储路径
    #指定组个数，根据实际配置决定，（当前案例拥有2个组group1和group2）
    group_count = 2                   #设置组的个数



    在末尾增加2个组的具体信息：
    [group1]
    group_name=group1
    storage_server_port=23000
    store_path_count=1
    store_path0=/opt/fastdfs/storage/files

    [group2]
    group_name=group2
    storage_server_port=23000
    store_path_count=1
    store_path0=/opt/fastdfs/storage/files

    第一个组的第二个storage按照相同的步骤操作；

    另外一个组的两个storage也按照相同的步骤操作；

    #测试nginx的配置文件是否正确（测试全部6台服务器）
       /usr/local/nginx_fdfs/sbin/nginx -c /usr/local/nginx_fdfs/conf/nginx.conf -t
    #启动nginx服务器(全部6台服务器)
      /usr/local/nginx_fdfs/sbin/nginx -c /usr/local/nginx_fdfs/conf/nginx.conf




测试：使用浏览器分别访问 6台 服务器中的fastdfs文件


第十步：部署前端用户访问入口服务器，即访问192.168.230.128上的Nginx，该Nginx负载均衡到后端2个tracker server；
    配置nginx.conf文件
    location ~ /group[1-9]/M0[0-9] {   
        proxy_pass http://fastdfs_group_server; 
    }



    添加一个upstream 执行服务的IP为 2台tracker 的地址
    #部署配置nginx负载均衡:
    upstream fastdfs_group_server {  
        server 192.168.171.135:80;  
        server 192.168.171.136:80; 
    }

测试：使用浏览器访问128（唯一入口的nginx服务器）服务器中的fastdfs文件
注意：由于之前128的nginx中可能拥有静态资源拦截会导致访问不到文件，这时可以注释或删除这些静态资源拦截




==============================补充资料============================================
最后，为了让服务能正常连接tracker，请关闭所有机器的防火墙：
systemctl status firewalld   查看防火墙状态
systemctl disable firewalld  禁用开机启动防火墙
systemctl stop firewalld     停止防火墙
systemctl  restart  network  重启网络
systemctl  start network     启动网络
systemctl  stop  network     停止网络

可能安装的linux（无图形的）没有开启网卡服务，可以修改/etc/sysconfig/network-scripts 下的网卡配置文件设置 ONBOOT=yse 
表示开机启动网卡，然后启动网络服务即可

Keepalived当主nginx出现故障后会自动切换到备用nginx服务器的一款软件 通常由运维人员进行使用
```

