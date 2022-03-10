@[TOC](目录)
# 1：SpringBoot介绍
> SpringBoot是Spring全家桶的一个框架
> 主要作用是让Spring和SpringMVC的使用更方便，核心是IOC容器
> ****
> 特点：
> 1：可以创建Spring应用
> 2：内嵌tomcat，jetty，Undertow服务器，不需要手动配置
> 3：提供了starter起步依赖，比如mybatis框架，可以直接使用starter起步依赖创建
> 4：配置Spring和第三方库，自动配置（把Spring中的，第三方库中的对象创建好，放到容器中）
> 5：提供了健康检查，统计，外部化配置
> 6：不用生成代码，不使用xml做配置
# 2：SpringBoot使用
***创建SpringBoot项目***
![在这里插入图片描述](https://img-blog.csdnimg.cn/c55085e5e470433f9ab85d63f130614b.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/e31d7f3084db40daa6301cc4f395c68f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ccc1be29b0914444bd96cfb268b6eeb8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a47bf68e1b0e4fe0aa47a97068206b2c.png)
***SpringBoot项目在生成之后，在src下的javaspringboot目录下有个主启动类***
![在这里插入图片描述](https://img-blog.csdnimg.cn/1149bf908cfa4a0f99b87c890e5717d0.png)
```java
/*
复合注解：@SpringBootApplication
通过这个注解表示这个类和主方法是整个项目的启动方式

复合注解的功能包括以下注解的所有功能：
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan

通过这些注解可以扫描项目内的@Controller，@Service等注解
*/
@SpringBootApplication
public class JavaSpringBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(JavaSpringBootApplication.class, args);
    }
}
```
***注：主类是分层次的，如果在主类以上的层的类是不会参与项目的，必须是同级或下级***
## 2.2：SpringBoot配置文件

> 配置文件名为application
> 具有两种格式：properties和yml格式
> ****
> 配置文件可以配置项目内的功能
> 比如服务器的端口号，上下文等
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/7c8cdef9eb3d4bce8d4ef786f6ff974b.png)
以yml写的格式（注：分号： 后有个空格）：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6c958a180ab249d1a6cc03252bdea2a9.png)
## 2.3：SpringBoot多环境配置
> SpringBoot可以拥有多个配置文件：命名规范为：application-环境标识.properties
> 但每次启动项目只能读取一个配置文件的信息
> 可以在主配置文件内指定要读取的配置文件名

***创建开发，上线，测试环境配置文件***
![在这里插入图片描述](https://img-blog.csdnimg.cn/a1b40005b74f495583f9bf06f7d2bcc3.png)
***在主配置文件（application.yml）中指定读取的配置文件***
![在这里插入图片描述](https://img-blog.csdnimg.cn/76f50366b8c04ada998723d01014a675.png)
## 2.4：SpringBoot自定义配置
### 2.4.1：@Value注解
***通过这个注解可以把类中的属性的值写在配置文件中被读取***
![在这里插入图片描述](https://img-blog.csdnimg.cn/bb9ec4cb6a304a71993a26d12853bb5f.png)
可以在**容器内**的类里给属性赋值
```java
@Controller
public class Controller {
    @Value("${student.name}")
    private String name;
    @Value("${student.id}")
    private String id;
}
```
### 2.4.2：@ConfigurationProperties注解
> 和@Value注解差不多，可以将配置文件内的值赋值到类的属性内（也需要是容器内的对象类）

![在这里插入图片描述](https://img-blog.csdnimg.cn/bb9ec4cb6a304a71993a26d12853bb5f.png)
```java
//这个注解的参数前缀prefix是配置文件中的属性的前缀
//必须设置前缀
@ConfigurationProperties(prefix = "student")
@Component
public class Student {
    //然后会根据配置文件的后缀名自动寻找同名的属性赋值
    private String name;
    private String id;
}
```
***如果注解页面报红添加依赖***
```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```
## 2.5：SpringBoot中使用容器对象
> 如果想使用容器就需要接收主类里的run方法返回值
```java
@SpringBootApplication
public class JavaSpringBootApplication {

    public static void main(String[] args) {
        //接收容器
        ConfigurableApplicationContext ctx = SpringApplication.run(JavaSpringBootApplication.class, args);
        //从容器中获取对象
        //此时的ctx就相当于ApplicationContext中容器的对象
        String[] names = ctx.getBeanDefinitionNames();
        for (int i = 0; i < names.length; i++) {
            System.out.println(names[i]);
        }
    }
}
```
## 2.6：CommandLineRunner接口，ApplicationRunner接口
> 两个接口的作用一样
> 两个接口都有个run方法，执行时间是在容器对象创建好后执行run
> 可以完成自定义在容器对象创建好后的操作
> 使用：主类继承CommandLineRunner接口或ApplicationRunner接口
```java
@SpringBootApplication
public class JavaSpringBootApplication implements CommandLineRunner {

    public static void main(String[] args) {
        SpringApplication.run(JavaSpringBootApplication.class, args);
        //此时主方法内调用的run就是以下的run方法
    }

    @Override
    public void run(String... args) throws Exception {
       	//执行操作，此时容器内的对象已经创建完毕，可以对容器内的对象进行操作
    }
}
```
# 3：SpringBoot和Web组件
## 3.1：SpringBoot中使用拦截器
***创建拦截器***
```java
public class Interceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截器执行");
        return true;
    }
}
```
***此时的拦截器还不在容器中，需要添加到容器内***
> 而SpringBoot中添加拦截器需要一个类作为配置文件并继承WebMvcConfigurer接口

```java
//加入@Configuration注解表示这个类作为配置文件使用
//实现接口WebMvcConfigurer
//并实现接口的方法addInterceptors
@Configuration
public class WebMVCConfig implements WebMvcConfigurer {
    @Override//有Override注解不需要@Bean注解，会自动加入到容器中
    public void addInterceptors(InterceptorRegistry registry) {
        String[] path = {"/user/*"};//创建需要拦截的路径,以字符串形式,他可以是数组也可以是List<String>集合
        String excludePath = "/user/login";//创建哪些路径可以通过拦截器，可以是字符串和List<String>集合
        /*
		使用方法内的参数registry，他是一个InterceptorRegistry类
		添加拦截器方法：
		> addPathPatterns方法表示需要拦截的地址，可以是List集合和字符串
		> excludePathPatterns方法表示不需要拦截的地址，可以是List集合和字符串
		这两个方法的返回值都是InterceptorRegistration，因此可以写成以下形式
		*/
        registry.addInterceptor(new Interceptor()).addPathPatterns(path).excludePathPatterns(excludePath);
    }
}
```
## 3.2:SpringBoot中使用Servlet
***创建Servlet***
```java
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter pw = resp.getWriter();
        pw.println("执行Servlet");
        pw.flush();
        pw.close();
    }
}
```
***将Servlet添加到容器内***

> 创建类作为配置文件
```java
@Configuration
public class ServletConfig {
    @Bean
    //方法返回值必须是ServletRegistrationBean
    public ServletRegistrationBean servletRegistrationBean(){
        //这个方法需要两个参数，第一个参数是Servlet对象，第二个参数是URL地址
        //只要访问url就可以访问到这个servlet对象
        ServletRegistrationBean bean = new ServletRegistrationBean(new MyServlet(),"/myServlet");
        return bean;
    }
}
```
## 3.3：SpringBoot中使用Filter过滤器
***创建过滤器***
```java
public class MyFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("过滤器启动");
        filterChain.doFilter(servletRequest,servletResponse);
    }
}
```
***将过滤器添加到容器内***
> 创建类作为配置文件
```java
@Configuration
public class FilterConfig {
    @Bean
    //返回值必须是FilterRegistrationBean
    public FilterRegistrationBean filterRegistrationBean(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new MyFilter());//添加过滤器的类
        bean.addUrlPatterns("/user/*");//设置过滤的地址
        return bean;
    }
}
```
***框架的过滤器***
> 在使用doPost方式时会出现乱码的情况，可以通过框架提供的过滤器解决，也可以用自己的过滤器
> 
在配置文件application.yml中添加代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/d2f3efa433914bebbd8b74c5ca45ade6.png)
encoding.enabled=false的意思是使用过滤器中的编码方式而不是默认的
创建类作为配置文件将框架提供的过滤器添加到容器中：
```java
@Configuration
public class FilterConfig {
    @Bean
    public FilterRegistrationBean filterRegistrationBean(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        //使用框架中的过滤器类
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        //设置编码方式
        filter.setEncoding("utf-8");
        //指定request，response都使用encoding的值
        filter.setForceEncoding(true);
        bean.setFilter(filter);
        bean.addUrlPatterns("/*");//设置过滤的地址
        return bean;
    }
}
```
***配合文件修改字符集编码***
> 可以在application.yml直接修改所有目录的编码方式

***在application.yml配置文件添加***
![在这里插入图片描述](https://img-blog.csdnimg.cn/e1de14a3f031421188cd3da0c4117bb1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_17,color_FFFFFF,t_70,g_se,x_16)
# 4：ORM（SpringBoot+Mybatis）操作数据库
> SpringBoot中创建Mybatis直接添加起步依赖，mysql驱动等
```html
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.0</version>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```
> 在application配置文件中设置连接数据库的源
```html
#项目端口和上下文
server.port=8080
server.servlet.context-path=/project
#driver
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#url，参数部分useUnicode表示是否使用编码，characterEncoding设置编码，serverTimezone设置时区增加8小时，因为是国外的时间，如果使用国内时间需要增加8小时
spring.datasource.url=jdbc:mysql://localhost:3306/ccxg?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456
```
> @Mapper注解：这个注解是可以将dao层的接口加入到容器中被扫描到
```java
@Mapper
public interface UserDao {
    List<User> selectUser();
}
```
> 如果dao层的接口太多可以在主类上加入注解@MapperScan（可以导入多个目录下的dao）
```java
@SpringBootApplication
//MapperScan可以导入多个目录@MapperScan(basePackages = {"com.java.dao","com.crm.dao"})
@MapperScan(basePackages = {"com.java.dao"})
public class JavaProjectSpringbootApplication {
    public static void main(String[] args) {
        SpringApplication.run(JavaProjectSpringbootApplication.class, args);
    }
}
```
> 在SpringBoot中mapper文件和接口文件是可以分开的
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/be82e0cb379c471694bb58d830adcfd7.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/432aa316e0d740759716ac29051c1bc5.png)
> 分开之后需要在application配置文件中导入mapper文件位置
```xml
#mapper文件位置
mybatis.mapper-locations=classpath:mapper/*.xml

#日志
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

> 如果运行时找不到application配置文件不在类路径下可以加入插件
```html
<resources>
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.*</include>
        </includes>
    </resource>
</resources>
```
# 5：SpringBoot中的事务控制
***在Service层添加事务注解***
```java
@Service
public class UserServiceImpl implements UserService {
    @Resource
    private UserDao userDao;
    //添加事务注解，如果有运行时异常，回滚事务
    @Transactional
    @Override
    public List<User> selectUser() {
        //抛出运行时异常，测试事务
        int count = 10/0;
        return userDao.selectUser();
    }
}
```
> 在主类添加注解@EnableTransactionManagement表示启动事务管理器
```java
@SpringBootApplication
@MapperScan(basePackages = {"com.java.dao"})
@EnableTransactionManagement
public class JavaProjectSpringbootApplication {
    public static void main(String[] args) {
        SpringApplication.run(JavaProjectSpringbootApplication.class, args);
    }
}
```
# 6：接口架构风格-RESTful
> RESTful（表现层状态转移）架构：
> ***普通访问地址：http://localhost:8080/project/createUser?name=zs&password=123***
> ***RESTful架构风格：http://localhost:8080/project/createUser/zs/123***
> ****
> 可以指定Servlet和Controller中的处理请求的方式：get，post，put，delete等
> ****
> 普通的访问是一个名词对应一个操作比如createUser对应insert操作，deleteUser对应delete操作
> 而RESTful风格则是一个名词对应多个操作比如User对应所有CRUD操作
## 6.1：RESTful风格的注解
> @PathVariable作用：从url中获取数据
> @GetMapping：支持get请求方式相当于：@RequestMapping(method = RequestMethod.GET)
> @PostMapping：支持post请求：@RequestMapping(method = RequestMethod.POST)
> @PutMapping：支持put请求：@RequestMapping(method = RequestMethod.PUT)
> @DeleteMapping：支持delete请求：@RequestMapping(method = RequestMethod.DELETE)
> @RestController：复合注解，是@Controller和@ResponseBody组合
> 在类上面加入@RestController注解表示所有方法都是@ResponseBody返回数据
>****
>***注：URL中的地址一定要是唯一值，比如@PostMapping(value = "/user/{id}/{name}/{sex}")个数类型必须不同***
```java
//@RestController表示所有方法具有@ResponseBody注解
@RestController
public class UserController {

    @Resource
    private UserService userService;

    @GetMapping(value = "/user")//http://localhost:8080/project/user
    public List<User> selectUser(){
        return userService.selectUser();
    }

    @PostMapping(value = "/user/{id}/{name}/{sex}")//http://localhost:8080/project/user/1/zs/男
    //@PathVariable注解表示获取url中的值，而PostMapping中用占位符表示
    public int insertUser(@PathVariable(value = "id") Integer id,@PathVariable(value = "name") String name,@PathVariable(value = "sex") String sex){
        return userService.intserUser(id,name,sex);
    }

    @PutMapping(value = "/user/{id}/{name}/{sex}")//http://localhost:8080/project/user/1/ls/女
    public int updateUser(@PathVariable(value = "id") Integer id,@PathVariable(value = "name") String name,@PathVariable(value = "sex") String sex){
        return userService.updateUser(id,name,sex);
    }

    @DeleteMapping(value = "/user/{id}")//http://localhost:8080/project/user/1
    public int deleteUser(@PathVariable(value = "id") Integer id){
        return userService.deleteUser(id);
    }

}
```
## 6.2：在页面中支持put与delete请求方式

> 框架中可以将post请求转为相应的put或delete请求
> 在application配置文件中开启HiddenHttpMethodFilter过滤器
> 在页面发送请求时，需要包含_method参数，它的值是put或delete，这个请求要用post方式发送
```java
#启用过滤器
spring.mvc.hiddenmethod.filter.enabled=true
```
# 7：SpringBoot集成Redis
> 加入Redis起步依赖
```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
> 在application配置文件中添加redis配置信息
```html
#指定redis的host ip password等
spring.redis.host=192.168.73.132
spring.redis.port=6379
```
> redis使用方式
```java
@Controller
public class UserController {

    /*
    * 注入RedisTemplate
    * RedisTemplate<泛型可以是String,String或者Object,Object>
    * 也可以不写泛型
    */
    @Resource
    private RedisTemplate redisTemplate;

    @RequestMapping (value = "/redis")
    @ResponseBody
    public String getRedis(){
        //获取string类型对象
        ValueOperations string = redisTemplate.opsForValue();
        //获取list集合对象
        ListOperations listOperations = redisTemplate.opsForList();
        //获取set集合对象
        SetOperations setOperations = redisTemplate.opsForSet();
        //获取哈希类型对象
        HashOperations hashOperations = redisTemplate.opsForHash();
        //获取zset类型对象
        ZSetOperations zSetOperations = redisTemplate.opsForZSet();

        //redis执行的命令 添加命令
        string.set("key","value");
        //redis执行的命令 获取数据
        Object key = string.get("key");
        return key+"";
    }

}
```
> 通过RedisTemplate处理的数据都是被序列化的数据："\xac\xed\x00\x05t\x00\x03key"
> 如果想要可观的数据可以使用StringRedisTemplate是作为string处理，是string的序列化
> 数据被序列化是为了方便跨平台传输数据，转成序列化二进制是不需要担心编码问题的
> RedisTemplate默认的序列化是jdk的
> 

> 设置RedisTemplate序列化
```java
	/*
    设置RedisTemplate的序列化，可以单独设置key的序列化，也可以设置value的序列化，也可以同时设置
    * */
    public void setString(){
    
    	//StringRedisSerializer为string的序列化
    	
        //setKeySerializer方法设置key的序列化
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //setValueSerializer设置value的序列化
        redisTemplate.setValueSerializer(new StringRedisSerializer());
    }
```
> 设置java对象序列化版本号
> 在idea的settings中找到以下
> 在普通java类实现Serializable接口然后鼠标放在类名按alt+回车![在这里插入图片描述](https://img-blog.csdnimg.cn/49eb73ccd74047888bef5b1a17fe2c6b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
public class User implements Serializable {
    
    private static final long serialVersionUID = 8844840639320734470L;
    
    private Integer id;
    private String name;
    private String sex;

```

> 使用json序列化
```java
/*
    * json序列化，把java对象序列化
    * */
    @RequestMapping(value = "/json")
    @ResponseBody
    public String setJson(){
        User user = new User();
        user.setName("zs");
        user.setId(1);
        user.setSex("男");
        //使用redisTemplate将key进行string的序列化
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //将value执行json序列化参数是序列化的java对象，注java对象需要有序列化版本号
        redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer(User.class));
        redisTemplate.opsForValue().set("user",user);
        return "";
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4a28c2ed3c8548cfa495a41799f84107.png)
# 8：SpringBoot集成Dubbo
> 创建公共项目（普通maven项目即可）
![在这里插入图片描述](https://img-blog.csdnimg.cn/ef8ef09926114d8284c96beb16c53321.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_16,color_FFFFFF,t_70,g_se,x_16)

> 创建提供服务者，并添加公共项目的依赖和dubbo的依赖
```html
<!--公共项目的依赖-->
<dependency>
    <groupId>com.java</groupId>
    <artifactId>java-interface-api</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
<!--Dubbo起步依赖-->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.8</version>
</dependency>
<!--zookeeper注册中心依赖-->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-dependencies-zookeeper</artifactId>
    <version>2.7.8</version>
    <type>pom</type>
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
> 提供者实现公共项目的接口实现类业务
```java
/*
* 使用dubbo中的注解暴露提供者
* @DubboService(interfaceClass = 业务接口.class,version = "版本号")
* */
@DubboService(interfaceClass = UserService.class,version = "1.0.0")
public class UserServiceImpl implements UserService {
    @Override
    public User selectUserById(Integer id) {
        User user = new User();
        user.setName("zs");
        user.setSex("男");
        user.setId(1);
        return user;
    }
}
```
> 在配置文件中添加
```java
#配置服务名称
spring.application.name=UserService-provider
#配置扫描的包
dubbo.scan.base-packages=com.java.service
#注册中心
dubbo.registry.address=zookeeper://localhost:2181
```
> 在主启动类上添加注解@EnableDubbo或者@EnableDubboConfig

> 创建消费者项目，加入dubbo起步依赖，zookeeper依赖，提供者依赖
```html
<dependency>
    <groupId>com.java</groupId>
    <artifactId>java-project-provider</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>

<dependency>
   <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.8</version>
</dependency>

<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-dependencies-zookeeper</artifactId>
    <version>2.7.8</version>
    <type>pom</type>
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
> 消费者使用注册中心的订阅的业务
```java
@Controller
public class UserController {

    //使用DubboReference注解 参数是业务接口，版本号
    @DubboReference(interfaceClass = UserService.class,version = "1.0")
    private UserService userService;

    @RequestMapping(value = "/user")
    @ResponseBody
    public String selectUserById(){
        return userService.selectUserById(null).toString();
    }
}
```
> 主类添加注解@EnableDubbo
```java
@SpringBootApplication
@EnableDubbo
public class JavaProjectConsumerApplication {

    public static void main(String[] args) {
        SpringApplication.run(JavaProjectConsumerApplication.class, args);
    }

}
```

> 配置文件中添加注册中心信息
```java
server.port=8081
server.servlet.context-path=/project
#配置服务名称
spring.application.name=consumer-application
#注册中心
dubbo.registry.address=zookeeper://localhost:2181
```
# 9：打成war包后指定使用外部tomcat
> 主启动类继承SpringBootServletInitializer实现SpringApplicationBuilder方法
> 使用方法内的builder参数的sources方法，参数是主启动类.class
```java
@SpringBootApplication
public class JavaProjectConsumerApplication extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(JavaProjectConsumerApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(JavaProjectConsumerApplication.class);
    }
}
```
> 这样打包之后就可以使用外部的tomcat启动项目
