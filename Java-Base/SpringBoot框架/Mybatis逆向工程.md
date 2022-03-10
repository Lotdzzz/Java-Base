> Mybatis逆向工程会根据数据库的表数据，自动生成dao层的接口，mapper文件

> 在SpringBoot中添加Mybatis自动生成代码插件
```html
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.6</version>
    <configuration>
    	<!--configurationFile标签内是逆向工程的配置文件，直接放在和src目录平级的目录即可-->
        <configurationFile>generatorConfig.xml</configurationFile>
        <overwrite>true</overwrite>
        <verbose>true</verbose>
    </configuration>
</plugin>
```
> generatorConfig.xml内容
```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <!-- 指定连接数据库的JDBC驱动包所在位置，指定到你本机的完整路径 -->
    <classPathEntry location="C:\Program Files (x86)\MySQL\Connector J 8.0\mysql-connector-java-8.0.26.jar"/>

    <!-- 配置table表信息内容体，targetRuntime指定采用MyBatis3的版本 -->
    <context id="tables" targetRuntime="MyBatis3">

        <!-- 抑制生成注释，由于生成的注释都是英文的，可以不让它生成 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>

        <!-- 配置数据库连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/ccxg"
                        userId="root"
                        password="123456">
        </jdbcConnection>

        <!-- 生成domain类，targetPackage指定domain类的包名， targetProject指定生成的domain放在哪个工程下面-->
        <javaModelGenerator targetPackage="com.java.domain"
                            targetProject="D:\javaNode\test\java-mybatis\src\main\java">
            <property name="enableSubPackages" value="false" />
            <property name="trimStrings" value="false" />
        </javaModelGenerator>

        <!-- 生成MyBatis的Mapper.xml文件，targetPackage指定mapper.xml文件的包名， targetProject指定生成的mapper.xml放在哪个工程下面 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

        <!-- 生成MyBatis的Mapper接口类文件,targetPackage指定Mapper接口类的包名， targetProject指定生成的Mapper接口放在哪个工程下面 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.java.dao" targetProject="src/main/java">
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>

        <!-- 数据库表名及对应的Java模型类名 指定要生成的表 -->
        <table tableName="tbl_user" domainObjectName="User"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>

    </context>

</generatorConfiguration>
```

> 如果开头的http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd爆红
![在这里插入图片描述](https://img-blog.csdnimg.cn/2c320d344a8849c4835550d614d30397.png)
打开IDEA的file-settings
![在这里插入图片描述](https://img-blog.csdnimg.cn/4115fbb847f742d1932196bfb1c17477.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> 将http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd复制到浏览器的地址栏中下载
> 然后添加时第一行填写http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd第二行添加下载后文件所在地址
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/e0e4f376094a4895ae8cf4c4f70ff6f5.png)

> 在右边项目点击插件的生成操作
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f0da9b0cfdb24da3bef27f5360041b76.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)


