JDK官方网站Oracle：[JDK8的安装](https://www.oracle.com/java/technologies/downloads/#java8)

打开网站往下翻找到以下页面

![img](https://img-blog.csdnimg.cn/985a70de224042649cc5ecd5462056ca.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

点击Java8[比较流行的版本]

点击下载会弹出

![img](https://img-blog.csdnimg.cn/67e1d1385c7549bbb29890989ed863cb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/415f7eccdf994ccd886e2dcd8f24a941.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

按照步骤一步步走即可

安装完之后需要配置环境变量

找到系统的环境变量：点击设置

![img](https://img-blog.csdnimg.cn/7b26c27e0f904dec9e34ca70ad722dbe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

左边栏找到![img](https://img-blog.csdnimg.cn/44286c7703304463afa34878eed710c7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)点击右边的![img](https://img-blog.csdnimg.cn/71e518065f2846028581d894db2b274a.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/94469b4cd39846dfa980952dbfb4a794.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

打开后会有用户环境变量和系统环境变量我们看系统环境变量右下角![img](https://img-blog.csdnimg.cn/19284ed139c34672ba62e614d2aa3eca.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

变量值选择您下载的JDK安装目录 这里是默认安装路径C盘下programFiles的java的jdk

点击确定

![img](https://img-blog.csdnimg.cn/96c391868ada44bbad2e1b2eaf1c5f7b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

然后找到系统环境变量的Path

![img](https://img-blog.csdnimg.cn/4c855193a9984ae7ab55ab321bc1868b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

打开点新建输入%JAVA_HOME%/bin

![img](https://img-blog.csdnimg.cn/16936549475d45a283d9f392fe721245.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

到这里配置完成 测试一下按Win+R键

输入java -version

![img](https://img-blog.csdnimg.cn/c34794ffd0b745558ab406d20987a8f2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

显示类似信息表示配置成功

到这里Java的环境变量配置完成