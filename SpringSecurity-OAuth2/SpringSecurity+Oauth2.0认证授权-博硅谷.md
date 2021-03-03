> 视频资源：https://www.bilibili.com/video/BV14z4y1d7eY
>
> 参考文档：https://blog.csdn.net/qq_38697437/article/details/106129153



> 文档资源
>
> 第1章 [Spring Security 基本概念](https://zhuanlan.zhihu.com/p/136003528)
>
> 第2章[基于Session的认证方式](https://zhuanlan.zhihu.com/p/136003528)
>
> 第3章 [Spring Security快速上手](https://zhuanlan.zhihu.com/p/136108776)
>
> 第4章 [Spring Security应用详解](https://zhuanlan.zhihu.com/p/136473553)
>
>    4.1 [Spring Security工作原理](https://zhuanlan.zhihu.com/p/136627658)
>
>   4.2 [Spring Security认证流程](https://zhuanlan.zhihu.com/p/136636352)
>
>   4.3 [Spring Security授权流程](https://zhuanlan.zhihu.com/p/136736820)
>
> 第5章 [分布式系统认证方案 OAuth2.0](https://zhuanlan.zhihu.com/p/137835878)
>
> 第6章 [JWT令牌](https://zhuanlan.zhihu.com/p/137897376)
>
> 第7章 [Spring Security 实现分布式系统授权](https://zhuanlan.zhihu.com/p/137903094)
>
> 

## 1、基本概念

### 1.1 什么是认证

进入互联网时代，大家每天都在刷收，常用的软件有微信、支付宝、头条、下面那微信举例说明相关的基本概念，在初次使用微信前需要注册成为微信用户，然后输入账号和密码即可登录微信，输入账号和密码登录微信的过程就是认证。

系统为什么要认证？

认证是为了保护系统的隐私数据与资源，用户的合法 方可访问该系统的资源。

**认证：**用户认证就是判断一个用户的身份是否合法的过程，用户去访问系统资源时，系统要求验证用户的身份信息，身份合法才可以继续访问，不合法则拒绝访问。常用的用户身份认证方式有：用户名密码登录，二维码登录，手机短信认证，指纹认证、刷脸认证等方式。

### 1.2 什么是会话

用户认证通过后，为了避免用户的每次操作都进行认证可以将用户的信息保存在会话中，会哈就是系统为了保持当前用户的登录状态所提供的机制，常见的有基于session的方式，基于token方式。

**基于session的认证方式**，如下图：

它的交互流程是：用户认证成功后，在服务端生产用户相关的数据保存在session（当前会话）中， 发给客户端的session_id存放到客户端的cookie中，这样用户客户端请求时，带上session_id就可以验证服务daunt是否存在session数据，以此完成用户的合法校验，当用户





**基于token方式**，如下图：

它的交互流程是，用户认证成功后，服务端生产一个token发给客户端，客户端可以放到cookie或者localStory

### 1.3 什么是授权

### 1.4 RBAC

如何实现授权，

1.4.1 基于角色的访问控制

1.4.2 基于资源的访问控制

RBAC(Resource  based  access  control)，基于资源的访问控制。资源在系统中是不变的，比如资源有：类中的方法，页面中的按钮。

```java
if(user.hasPermission('用户报表查看（权限标识符）')){
	//系统资源内容
	//用户报表查看
}
```



## 2、基于Session的认证方式

### 2.1 认证流程

用户认证成功后，**在服务端生成**用户相关的数据保存在session(当前会话)中，**发给客户端的 sesssion_id 存放到 cookie 中**，这样用户客户端**请求时带上 session_id** 就可以验证服务器端是否存在 session 数 据，以此完成用户的合法校验，当用户退出系统或session过期销毁时,客户端的session_id也就无效了。

夏天是session认证方式的流程图：

![img](https://img-blog.csdnimg.cn/20200817171649769.png)



基于Session的认证机制由Servlet规范定制，Servlet容器已经实现，用户通过HttpSession的操作方法即可实现，下面是HttpSession的相关的操作API

| 方法                                        | 含义                    |
| ------------------------------------------- | ----------------------- |
| HttpSession getSession(Boolean create )     | 获取当前HttpSession对象 |
| void setAttribute(String name,Object value) | 向session中存放对象     |
| void removeAttribute(String name)           | 移除session对象         |
| void invalidate()                           | 使得HttpSession失效     |
| 略... ...                                   |                         |



### 2.2 创建工程

​	本案例工程使用maven进行构建，使用SprignMVC、Servlet3.0实现。

​	Servlet3.0 支持使用Java类的方式实现配置

2.2.1 创建maven工程

​	工程名称 springsecurity-springmvc

com.beyondsoft.security

引入依赖

```xml
<packageing>war</packageing>	

```

```java
pacakge config;

@Configuration
@ComponentScan(backPackges="com.beyondsoft.spring", exclu)
public class ApplicationConfig{
    
}
```



##### 2.2.2 容器配置

#### 2.4 实现会话功能



#### 2.5 实现授权功能

interceptor.SimpleAuthenticationInterceptor

#### 2.6 小结



## 3、Spring Security快速上手



### 3.1 Spring Security介绍



### 3.2 Spring Security搭建工程

security-spring-security



```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.beyondsoft</groupId>
    <artifactId>security-spring-security</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <dependencies>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.8</version>
        </dependency>
        <!-- 引入spring security依赖-->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>5.1.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>5.1.5.RELEASE</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>springsecurity-springmvc</finalName>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.tomcat.maven</groupId>
                    <artifactId>common-tomcat-maven-plugin</artifactId>
                    <version>2.2</version>
                </plugin>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                    </configuration>
                </plugin>
                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <configuration>
                        <encoding>utf-8</encoding>
                        <useDefaultDelimiters>true</useDefaultDelimiters>
                        <resources>
                            <resource>
                                <directory>src/main/resources</directory>
                                <includes>
                                    <include>**/*.xml</include>
                                </includes>
                            </resource>
                            <resource>
                                <directory>src/main/java</directory>
                                <includes>
                                    <include>**/*.xml</include>
                                </includes>
                            </resource>
                        </resources>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```

#### 3.2.2 Spring容器配置

同security-springmvc

```java
@Configuration
@ComponentScan(basePackages = "com.beyondsoft.security.springmvc"
        ,excludeFilters ={@ComponentScan.Filter(type= FilterType.ANNOTATION,value = Controller.class)})
public class ApplicationConfig {
    //在此配置除了Controller的其他bean，比如：数据连接池、事务管理器、业务bean等。
}

```

#### 3.23. Servlet Context配置

同security-springmvc，不同之处是：spring-security 提供了认证、授权、拦截的功能 不需要自己再定义拦截器了

```java
@Configuration //就相当于springmvc.xml文件
@EnableWebMvc
@ComponentScan(basePackages = "com.beyondsoft.security.springmvc"
        ,includeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class)})
public class WebConfig  implements WebMvcConfigurer {

    //视图解析器
    @Bean
    public InternalResourceViewResolver viewResolver(){
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setPrefix("/WEB-INF/view/");
        viewResolver.setSuffix(".jsp");
        return viewResolver;
    }

    @Override
    public void addViewControllers(ViewControllerRegistry registry){
        registry.addViewController("/").setViewName("login");
    }
}
```

spring-security 提供了认证、授权、拦截的功能 不需要自己再定义拦截器了

#### 3.2.4 加载Spring容器

在init包下定Spring容器初始化类SpringApplicationInitalizer，此类实现WebApplicationInitializer接口，Spring容器启动时加载WebApplicationInitalizer接口的所有实现类。

```java
public class SpringApplicationInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    //spring容器，相当于加载applicationContext.xml
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{ApplicationConfig.class};
    }

    //servletContext，相当于加载springmvc.xml
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{WebConfig.class};
    }

    //url-mapping
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

### 3.3 认证

#### 3.3.1 认证页面

SpringSecurity默认提供了认证页面，不需要额外开发。

![img](http://img-03.proxy.5ce.com/view/image?&type=2&guid=d3d6bfb5-1e2f-eb11-8da9-e4434bdf6706&url=https://pic3.zhimg.com/v2-80184ea448ae02d73715fdc969f5ed6e_b.jpg)

#### 3.3.2.安全配置

Spring Security提供了用户名密码登录、退出、会话管理等认证功能，只需要配置即可使用。

**1) 在config包下定义WebSecurityConfig**，安全配置的内容包括：用户信息、密码编码器、安全拦截机制。

```java
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    //定义用户用户信息服务（查询用户信息）
    @Bean
    @Override
    public UserDetailsService userDetailsService(){
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(User.withUsername("zhagnsan").password("123").authorities("p1").build());
        manager.createUser(User.withUsername("lisi").password("456").authorities("p2").build());
        return manager;
    }
    
    //密码编码器(用来比对传入的密码和数据库密码的比对方式)
    @Bean
    public PasswordEncoder passwordEncoder(){
        return NoOpPasswordEncoder.getInstance();  //不需要对输入的密码进行编码或者加密，只进行字符串比较
    }
    
    //配置拦截截止(最关键最关键)
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/r/**").authenticated()   //所有/r/**的请求必须认证通过
                .anyRequest().permitAll()       //除了/r/**的请求 其他请求都可以访问
                .and()
                .formLogin()   //运行表单登录
                .successForwardUrl("/login-success");//自定义登录成功的页面地址
    }
}
```

在 userDetailsService()方法中，我们返回了一个UserDetailsService给spring容器，Spring Security会使用它来获取用户信息。我们暂时使用InMemoryUserDetailsManager实现类，并在其中分别创建了zhangsan、lisi两个用户，并设置密码和权限。

而在configure()中，我们通过HttpSecurity设置了安全拦截规则，其中包含了以下内容：

（1）url匹配/r/**的资源，经过认证后才能访问。

（2）其他url完全开放。

（3）支持form表单认证，认证成功后转向/login-success。

关于HttpSecurity的配置清单请参考附录 HttpSecurity。

**2) 加载 WebSecurityConfig**

修改SpringApplicationInitializer的getRootConfigClasses()方法，添加WebSecurityConfig.class：

```java
@Override
protected Class<?>[] getRootConfigClasses() {
    return new Class[]{ApplicationConfig.class, WebSecurityConfig.class};
}
```

#### 3.3.3 SpringSecurity初始化

Spring Security初始化，这里有两种情况

- 若当前环境没有使用 Spring或Spring MVC，则需要将 WebSecurityConfig(Spring Security配置类) 传入超类，以确保获取配置，并创建spring context。
- 相反，若当前环境已经使用 spring，我们应该在现有的springContext中注册Spring Security(上一步已经做将WebSecurityConfig加载至rootcontext)，此方法可以什么都不做。

在init包下定义SpringSecurityApplicationInitializer：

```java
public class SpringSecurityApplicationInitializer
        extends AbstractSecurityWebApplicationInitializer {
    public SpringSecurityApplicationInitializer() {
        //super(WebSecurityConfig.class);
    }
}
```

#### 3.2.4 默认根路径请求

在WebConfig.java中添加默认请求根路径跳转到/login，此url为spring security提供：

```java
// 默认Url根路径跳转到/login，此url为spring security提供
@Override
public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController("/").setViewName("redirect:/login");
}
```

spring security默认提供的登录页面。

#### 3.2.5 认证成功页面

在安全配置中，认证成功将跳转到/login-success，代码如下：

```java
    // 配置安全拦截机制 
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/r/**").authenticated()   //所有/r/**的请求必须认证通过
                .anyRequest().permitAll()       //除了/r/**的请求 其他请求都可以访问
                .and()
                .formLogin()   //运行表单登录
                .successForwardUrl("/login-success");//自定义登录成功的页面地址
    }
```

spring security支持form表单认证，认证成功后转向/login-success。

在LoginController中定义/login-success:

```java
@RestController
public class LoginController {

    @RequestMapping(value = "/login-success",produces = "text/plain;charset=utf-8")
    public String loginSuccess(){
        return "登录成功";
    }
}
```

#### 3.2.6 测试



```
clean tomcat7:run
```

**（1）启动项目，访问http://localhost:8080/security-spring-security/路径地址**

![img](http://img-03.proxy.5ce.com/view/image?&type=2&guid=d3d6bfb5-1e2f-eb11-8da9-e4434bdf6706&url=https://pic3.zhimg.com/v2-372b0223723eb65dbb8c26bee6ef1d4e_b.jpg)



**（2）登录**

1、输入错误的用户名、密码

![img](http://img-01.proxy.5ce.com/view/image?&type=2&guid=d3d6bfb5-1e2f-eb11-8da9-e4434bdf6706&url=https://pic3.zhimg.com/v2-ab0dd1c499f8b230d0d6a21ced5b011e_b.jpg)

2、输入正确的用户名、密码，登录成功

**（3）退出**

1、请求/logout退出

![img](http://img-02.proxy.5ce.com/view/image?&type=2&guid=d3d6bfb5-1e2f-eb11-8da9-e4434bdf6706&url=https://pic4.zhimg.com/v2-47fb1a53acb940f66cbb4787ad2104a3_b.jpg)

2、退出 后再访问资源自动跳转到登录页面

### 3.4 授权

实现授权需要对用户的访问进行拦截校验，校验用户的权限是否可以操作指定的资源，Spring Security默认提供授权实现方法。

在LoginController添加/r/r1或/r/r2

```java

/** 
 * 测试资源1
 * @return
 */
@GetMapping(value = "/r/r1",produces = {"text/plain;charset=UTF‐8"})
public String r1(){
    return " 访问资源1";
}
 
/**
 * 测试资源2
 * @return
 */
@GetMapping(value = "/r/r2",produces = {"text/plain;charset=UTF‐8"})
public String r2(){
    return " 访问资源2";
}
```

在安全配置类 WebSecurityConfig.java中配置授权规则：

```java
.antMatchers("/r/r1").hasAuthority("p1")                                       
.antMatchers("/r/r2").hasAuthority("p2")
```

antMatchers("/r/r1").hasAuthority("p1")表示：访问/r/r1资源的 url需要拥有p1权限。

.antMatchers("/r/r2").hasAuthority("p2")表示：访问/r/r2资源的 url需要拥有p2权限。

完整的WebSecurityConfig方法如下：

```java
    //配置拦截机制(最关键最关键)
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/r/r1").hasAuthority("p1")
                .antMatchers("/r/r2").hasAuthority("p2")
                .antMatchers("/r/**").authenticated()   //所有/r/**的请求必须认证通过
                .anyRequest().permitAll()       //除了/r/**的请求 其他请求都可以访问
                .and()
                .formLogin()   //运行表单登录
                .successForwardUrl("/login-success");//自定义登录成功的页面地址
    }
```

测试：

1、登录成功

2、访问/r/r1和/r/r2，有权限时则正常访问，否则返回403（拒绝访问）

### 3.5 小结

通过快速上手，咱们使用Spring Security实现了认证和授权，Spring Security提供了基于账号和密码的认证方式，通过安全配置即可实现请求拦截，授权功能，Spring Security能完成的不仅仅是这些。

## 4、Spring Security应用详解

### 4.1 集成SpringBoot

#### 4.1.1 Spring Boot 介绍

Spring Boot是一套Spring的快速开发框架，基于Spring 4.0设计，使用Spring Boot开发可以避免一些繁琐的工程搭建和配置，同时它集成了大量的常用框架，快速导入依赖包，避免依赖包的冲突。基本上常用的开发框架都支持Spring Boot开发，例如：MyBatis、Dubbo等，Spring 家族更是如此，例如：Spring cloud、Spring mvc、Spring security等，使用Spring Boot开发可以大大得高生产率，所以Spring Boot的使用率非常高。

本章节讲解如何通过Spring Boot开发Spring Security应用，Spring Boot提供spring-boot-starter-security用于开发Spring Security应用。

#### 4.1.2 创建maven工程

1）创建maven工程 security-spring-boot，工程结构如下：

![img](https://pic3.zhimg.com/80/v2-c2b3a54d1523ebb5415657c92c021c36_720w.jpg)

2）引入以下依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-
4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.beyondsoft.security</groupId>
    <artifactId>security-springboot</artifactId>
    <version>1.0-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
    </parent>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding> 
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    <dependencies>
        <!-- 以下是>spring boot依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 以下是>spring security依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <!-- 以下是jsp依赖-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <scope>provided</scope>
        </dependency>
        <!--jsp页面使用jstl标签 -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>
        <!--用于编译jsp -->
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <scope>provided</scope>
        </dependency>
         <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.0</version>
          </dependency>
    </dependencies>
    <build>
        <finalName>security-springboot</finalName>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.tomcat.maven</groupId>
                    <artifactId>tomcat7-maven-plugin</artifactId> 
                    <version>2.2</version>
                </plugin>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                    </configuration>
                </plugin>
                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <configuration>
                        <encoding>utf-8</encoding>
                        <useDefaultDelimiters>true</useDefaultDelimiters>
                        <resources>
                            <resource>
                                <directory>src/main/resources</directory>
                                <filtering>true</filtering>
                                <includes>
                                    <include>**/*</include>
                                </includes>
                            </resource>
                            <resource>
                                <directory>src/main/java</directory>
                                <includes>
                                    <include>**/*.xml</include>
                                </includes>
                            </resource>
                        </resources>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```

#### 4.1.3 spring 容器配置

SpringBoot工程启动会自动扫描启动类所在包下的所有Bean，加载到spring容器。

1）Spring Boot配置文件

[在resources下添加application.properties](https://link.zhihu.com/?target=http%3A//xn--resourcesapplication-4k45al78d8zx6g9j.properties/)，内容如下：

```properties
server.port=8080
server.servlet.context-path=/security-springboot
spring.application.name = security-springboot
```

2 ）Spring Boot 启动类

```java
@SpringBootApplication
public class SecuritySpringBootApp {
    public static void main(String[] args) {
        SpringApplication.run(SecuritySpringBootApp.class, args);
    }
}
```

#### 4.1.4 Servlet Context配置

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    //默认Url根路径跳转到/login，此url为spring security提供
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("redirect:/login");
    }
}
```

视图解析器配置在application.properties中

```properties
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

#### 4.1.5 安全配置

由于Spring boot starter自动装配机制，这里无需使用@EnableWebSecurity，WebSecurityConfig内容如下

```java
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
   //内容跟Spring security入门程序一致
    //配置用户信息服务
    @Override
    @Bean
    public UserDetailsService userDetailsService() {
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();

        manager.createUser(User.withUsername("zhangsan").password("123").authorities("p1").build());
        manager.createUser(User.withUsername("lisi").password("456").authorities("p2").build());
        return manager;
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return NoOpPasswordEncoder.getInstance();
    }

    //配置安全拦截机制
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/r/r1").hasAuthority("p1")
                .antMatchers("/r/r2").hasAuthority("p2")
                .antMatchers("/r/**").authenticated()
                .anyRequest().permitAll()
                .and()
                .formLogin().successForwardUrl("/login-success");
    }
}
```

#### 4.1.6 测试

LoginController的内容同同Spring security入门程序。

```java
@RestController
public class LoginController {
  //内容略..跟Spring security入门程序保持一致
    @RequestMapping(value = "/login-success",produces = {"text/plain;charset=UTF-8"})
    public String loginSuccess(){
        return " 登录成功";
    }
    /**
     * 测试资源1
     * @return
     */
    @GetMapping(value = "/r/r1",produces = {"text/plain;charset=UTF-8"})
    public String r1(){
        return " 访问资源1";
    }

    /**
     * 测试资源2
     * @return
     */
    @GetMapping(value = "/r/r2",produces = {"text/plain;charset=UTF-8"})
    public String r2(){
        return " 访问资源2";
    }
}
```

测试过程：

1、测试认证

2、测试退出

3、测试授权

### 4.2 工作原理

#### 4.2.1 结构总览

Spring Security所解决的问题就是安全访问控制，而安全访问控制功能其实就是对所有进入系统的请求进行拦截，校验每个请求是否能够访问它所期望的资源。根据前边知识的学习，可以通过Filter或AOP等技术来实现，SpringSecurity对Web资源的保护是靠Filter实现的，所以从这个Filter来入手，逐步深入Spring Security原理。

当初始化Spring Security时，会创建一个名为 SpringSecurityFilterChain 的Servlet过滤器，类型为org.springframework.security.web.FilterChainProxy，它实现了javax.servlet.Filter，因此外部的请求会经过此类，下图是Spring Security过虑器链结构图：

![img](https://pic3.zhimg.com/80/v2-8aa06cb7144dbab6290467fc918abfda_720w.jpg)

FilterChainProxy 是一个代理，真正起作用的是FilterChainProxy中SecurityFilterChain所包含的各个Filter，同时这些Filter作为Bean被Spring管理，它们是Spring Security核心，各有各的职责，但他们并不直接处理用户的认证，也不直接处理用户的授权，而是把它们交给了认证管理器（AuthenticationManager）和决策管理器（AccessDecisionManager）进行处理，下图是FilterChainProxy相关类的UML图示。

![img](https://pic4.zhimg.com/80/v2-7103c8a68e1eb429ed149df4b0fd900f_720w.jpg)

spring Security功能的实现主要是由一系列过滤器链相互配合完成。

![img](https://pic3.zhimg.com/80/v2-b8f4aac780e4ff35eb36e2ceabeb77ba_720w.jpg)

下面介绍过滤器链中主要的几个过滤器及其作用：

**SecurityContextPersistenceFilter** 这个Filter是整个拦截过程的入口和出口（也就是第一个和最后一个拦截器），会在请求开始时从配置好的 SecurityContextRepository 中获取 SecurityContext，然后把它设置给SecurityContextHolder。在请求完成后将 SecurityContextHolder 持有的 SecurityContext 再保存到配置好的 SecurityContextRepository，同时清除 securityContextHolder 所持有的SecurityContext；

**UsernamePasswordAuthenticationFilter** 用于处理来自表单提交的认证。该表单必须提供对应的用户名和密码，其内部还有登录成功或失败后进行处理的AuthenticationSuccessHandler 和AuthenticationFailureHandler，这些都可以根据需求做相关改变；

**FilterSecurityInterceptor** 是用于保护web资源的，使用AccessDecisionManager对当前用户进行授权访问，前面已经详细介绍过了；

**ExceptionTranslationFilter** 能够捕获来自 FilterChain 所有的异常，并进行处理。但是它只会处理两类异常：AuthenticationException 和 AccessDeniedException，其它的异常它会继续抛出。

#### 4.2.2 认证流程

##### 4.2.2.1 认证流程

![img](https://pic3.zhimg.com/80/v2-131f392f77b86a2b57a0c044c43f5dd2_720w.jpg)

让我们仔细分析认证过程：

1. 用户提交用户名、密码被SecurityFilterChain中的 UsernamePasswordAuthenticationFilter 过滤器获取到，封装为请求Authentication，通常情况下是UsernamePasswordAuthenticationToken这个实现类。

2. 然后过滤器将Authentication提交至认证管理器（AuthenticationManager）进行认证

3. 认证成功后， AuthenticationManager 身份管理器返回一个被填充满了信息的（包括上面提到的权限信息，身份信息，细节信息，但密码通常会被移除） Authentication 实例。

4. SecurityContextHolder 安全上下文容器将第3步填充了信息的 Authentication ，通过

SecurityContextHolder.getContext().setAuthentication(…)方法，设置到其中。可以看出AuthenticationManager接口（认证管理器）是认证相关的核心接口，也是发起认证的出发点，它的实现类为ProviderManager。而Spring Security支持多种认证方式，因此ProviderManager维护着一个List<AuthenticationProvider> 列表，存放多种认证方式，最终实际的认证工作是由AuthenticationProvider完成的。咱们知道web表单的对应的AuthenticationProvider实现类为DaoAuthenticationProvider，它的内部又维护着一个UserDetailsService负责UserDetails的获取。最终AuthenticationProvider将UserDetails填充至Authentication。

认证核心组件的大体关系如下：

![](https://pic2.zhimg.com/80/v2-52484a42fdffbe0e2994fa39dfd3d8b5_720w.jpg)

##### 4.3.2.2 AuthenticationProvider

通过前面的**Spring Security认证流程**我们得知，认证管理器（AuthenticationManager）委托AuthenticationProvider完成认证工作。

AuthenticationProvider是一个接口，定义如下：

```java
public interface AuthenticationProvider {
    Authentication authenticate(Authentication authentication) throws AuthenticationException;
    boolean supports(Class<?> var1);
}
```

**authenticate**()方法定义了**认证的实现过程**，它的参数是一个Authentication，里面包含了登录用户所提交的用户、密码等。而返回值也是一个Authentication，这个Authentication则是在认证成功后，将用户的权限及其他信息重新组装后生成。

Spring Security中维护着一个 List<AuthenticationProvider> 列表，存放多种认证方式，不同的认证方式使用不同的AuthenticationProvider。如使用用户名密码登录时，使用AuthenticationProvider1，短信登录时使用AuthenticationProvider2等等这样的例子很多。

每个AuthenticationProvider需要实现**supports**（）方法来表明自己支持的认证方式，如我们使用表单方式认证，在提交请求时Spring Security会生成UsernamePasswordAuthenticationToken，它是一个Authentication，里面

封装着用户提交的用户名、密码信息。而对应的，哪个AuthenticationProvider来处理它？

我们在**DaoAuthenticationProvider**的基类AbstractUserDetailsAuthenticationProvider发现以下代码：

```java
public boolean supports(Class<?> authentication) {
    return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication);
}
```

**也就是说当web表单提交用户名密码时，Spring Security由DaoAuthenticationProvider处理。**

最后，我们来看一下 **Authentication**(认证信息)的结构，它是一个接口，我们之前提到的

UsernamePasswordAuthenticationToken就是它的实现之一：

```java
public interface Authentication extends Principal, Serializable {         （1）
    Collection<? extends GrantedAuthority> getAuthorities();              （2）
   
   Object getCredentials();   （3）                                                           
    Object getDetails();                                                  （4）
    Object getPrincipal();                                                （5）
    boolean isAuthenticated();                                           
    void setAuthenticated(boolean var1) throws IllegalArgumentException;
}
```

（1）Authentication是spring security包中的接口，直接继承自Principal类，而Principal是位于 [java.security](https://link.zhihu.com/?target=http%3A//java.security/)包中的。它是表示着一个抽象主体身份，任何主体都有一个名称，因此包含一个getName()方法。

（2）getAuthorities()，权限信息列表，默认是GrantedAuthority接口的一些实现类，通常是代表权限信息的一系列字符串。

（3）getCredentials()，凭证信息，用户输入的密码字符串，在认证过后通常会被移除，用于保障安全。

（4）getDetails()，细节信息，web应用中的实现接口通常为 WebAuthenticationDetails，它记录了访问者的ip地址和sessionId的值。

（5）getPrincipal()，身份信息，大部分情况下返回的是UserDetails接口的实现类，UserDetails代表用户的详细信息，那从Authentication中取出来的UserDetails就是当前登录用户信息，它也是框架中的常用接口之一。

##### 4.3.2.3 UserDetailsService

1）认识UserDetailsService

现在咱们现在知道DaoAuthenticationProvider处理了web表单的认证逻辑，认证成功后既得到一个Authentication(UsernamePasswordAuthenticationToken实现)，里面包含了身份信息（Principal）。这个身份信息就是一个 Object ，大多数情况下它可以被强转为UserDetails对象。

DaoAuthenticationProvider中包含了一个UserDetailsService实例，它负责根据用户名提取用户信息UserDetails(包含密码)，而后DaoAuthenticationProvider会去对比UserDetailsService提取的用户密码与用户提交的密码是否匹配作为认证成功的关键依据，因此可以通过将自定义的 UserDetailsService 公开为spring bean来定义自定义身份验证。

```java
public interface UserDetailsService {   
UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;    
}
```

很多人把 DaoAuthenticationProvider和UserDetailsService的职责搞混淆，其实UserDetailsService只负责从特定的地方（通常是数据库）加载用户信息，仅此而已。而DaoAuthenticationProvider的职责更大，它完成完整的认证流程，同时会把UserDetails填充至Authentication。

上面一直提到UserDetails是用户信息，咱们看一下它的真面目：

```java
public interface UserDetails extends Serializable {
   Collection<? extends GrantedAuthority> getAuthorities();
   String getPassword();
   String getUsername();
   boolean isAccountNonExpired();
   boolean isAccountNonLocked();
   boolean isCredentialsNonExpired();
   boolean isEnabled();
}
```

它和Authentication接口很类似，比如它们都拥有username，authorities。Authentication的getCredentials()与UserDetails中的getPassword()需要被区分对待，前者是用户提交的密码凭证，后者是用户实际存储的密码，认证其实就是对这两者的比对。Authentication中的getAuthorities()实际是由UserDetails的getAuthorities()传递而形成的。还记得Authentication接口中的getDetails()方法吗？其中的UserDetails用户详细信息便是经过了

AuthenticationProvider认证之后被填充的。

通过实现UserDetailsService和UserDetails，我们可以完成对用户信息获取方式以及用户信息字段的扩展。Spring Security提供的InMemoryUserDetailsManager(内存认证)，JdbcUserDetailsManager(jdbc认证)就是UserDetailsService的实现类，主要区别无非就是从内存还是从数据库加载用户。

2）测试

自定义UserDetailsService

```java
@Service 
public class SpringDataUserDetailsService implements UserDetailsService {
    //根据 账号查询用户信息
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //登录账号
        System.out.println("username="+username);
        //根据账号去数据库查询...
        //这里暂时使用静态数据
        UserDetails userDetails =
User.withUsername(username).password("123").authorities("p1").build();
        return userDetails;
    }
}
```

屏蔽安全配置类中 UserDetailsService的定义

```java
/*    @Bean 
    public UserDetailsService userDetailsService()  {
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
       
manager.createUser(User.withUsername("zhangsan").password("123").authorities("p1").build());
        manager.createUser(User.withUsername("lisi").password("456").authorities("p2").build());
        return manager;
    }*/
```

重启工程，请求认证，SpringDataUserDetailsService的loadUserByUsername方法被调用 ，查询用户信息。

##### 4.3.2.4.PasswordEncoder

1）认识PasswordEncoder

DaoAuthenticationProvider认证处理器通过UserDetailsService获取到UserDetails后，它是如何与请求Authentication中的密码做对比呢？

在这里Spring Security为了适应多种多样的加密类型，又做了抽象，DaoAuthenticationProvider通过PasswordEncoder接口的matches方法进行密码的对比，而具体的密码对比细节取决于实现：

```java
public interface PasswordEncoder {
    String encode(CharSequence var1);
    boolean matches(CharSequence var1, String var2);
    default boolean upgradeEncoding(String encodedPassword) {
        return false;
    }
}
```

而Spring Security提供很多内置的PasswordEncoder，能够开箱即用，使用某种PasswordEncoder只需要进行如下声明即可，如下：

```java
@Bean 
public PasswordEncoder passwordEncoder() {
    return  NoOpPasswordEncoder.getInstance();
}
```

NoOpPasswordEncoder采用字符串匹配方法，不对密码进行加密比较处理，密码比较流程如下：

1、用户输入密码（明文 ）

2、DaoAuthenticationProvider获取UserDetails（其中存储了用户的正确密码）

3、DaoAuthenticationProvider使用PasswordEncoder对输入的密码和正确的密码进行校验，密码一致则校验通过，否则校验失败。

NoOpPasswordEncoder 的校验规则拿 输入的密码和UserDetails中的正确密码进行字符串比较，字符串内容一致则校验通过，否则 校验失败。

实际项目中推荐使用BCryptPasswordEncoder, Pbkdf2PasswordEncoder, SCryptPasswordEncoder等，感兴趣的大家可以看看这些PasswordEncoder的具体实现。

2）使用BCryptPasswordEncoder

1、配置BCryptPasswordEncoder

在安全配置类中定义：

```java
@Bean 
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

测试发现认证失败，提示：Encoded password does not look like BCrypt。

原因：

由于UserDetails中存储的是原始密码（比如：123），它不是BCrypt格式。

跟踪 DaoAuthenticationProvider第33行代码查看 userDetails中的内容 ，跟踪第38行代码查看PasswordEncoder的类型。

2、测试BCrypt

通过下边的代码测试BCrypt加密及校验的方法

添加依赖：

```xml
<dependency> 
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

编写测试方法：

```java
@RunWith(SpringRunner.class) 
public class TestBCrypt {
    @Test
    public void test1(){
        //对原始密码加密
        String hashpw = BCrypt.hashpw("123",BCrypt.gensalt());
        System.out.println(hashpw);
        //校验原始密码和BCrypt密码是否一致
        boolean checkpw = BCrypt.checkpw("123",
"$2a$10$NlBC84MVb7F95EXYTXwLneXgCca6/GipyWR5NHm8K0203bSQMLpvm");
        System.out.println(checkpw);
    }
}
```

3、修改安全配置类

将UserDetails中的原始密码修改为BCrypt格式

```java
manager.createUser(User.withUsername("zhangsan").password("$2a$10$1b5mIkehqv5c4KRrX9bUj.A4Y2hug3I
GCnMCL5i4RpQrYV12xNKye").authorities("p1").build());
```

实际项目中存储在数据库中的密码并不是原始密码，都是经过加密处理的密码。

#### 4.2.3.授权流程

##### 4.2.3.1 授权流程

通过**快速上手**我们知道，Spring Security可以通过 http.authorizeRequests() 对web请求进行授权保护。SpringSecurity使用标准Filter建立了对web请求的拦截，最终实现对资源的授权访问。

Spring Security的授权流程如下：

![img](https://pic1.zhimg.com/80/v2-ad7705045a6d7e208ef87ec10e7851f8_720w.jpg)



分析授权流程：

1. **拦截请求**，已认证用户访问受保护的web资源将被SecurityFilterChain中的 FilterSecurityInterceptor 的子类拦截。

2. **获取资源访问策略**，FilterSecurityInterceptor会从 SecurityMetadataSource 的子类

DefaultFilterInvocationSecurityMetadataSource 获取要访问当前资源所需要的权限

Collection<ConfigAttribute> 。

SecurityMetadataSource其实就是读取访问策略的抽象，而读取的内容，其实就是我们配置的访问规则， 读取访问策略如：

```java
http
.authorizeRequests()                                                               
        .antMatchers("/r/r1").hasAuthority("p1")                                     
        .antMatchers("/r/r2").hasAuthority("p2")
        ...
```

\3. 最后，FilterSecurityInterceptor会调用 AccessDecisionManager 进行授权决策，若决策通过，则允许访问资源，否则将禁止访问。

AccessDecisionManager（访问决策管理器）的核心接口如下:

```java
public interface AccessDecisionManager {
   /**  
   * 通过传递的参数来决定用户是否有访问对应受保护资源的权限  
   */  
    void decide(Authentication authentication , Object object, Collection<ConfigAttribute>
configAttributes ) throws AccessDeniedException, InsufficientAuthenticationException;
 //略..    
}
```

这里着重说明一下decide的参数：

authentication：要访问资源的访问者的身份

object：要访问的受保护资源，web请求对应FilterInvocation

configAttributes：是受保护资源的访问策略，通过SecurityMetadataSource获取。

**decide接口就是用来鉴定当前用户是否有访问对应受保护资源的权限。**

##### 4.2.3.2 授权决策

AccessDecisionManager采用**投票**的方式来确定是否能够访问受保护资源。

![](https://pic1.zhimg.com/80/v2-1d5be950ada9cde77d8cc85f48101540_720w.jpg)

通过上图可以看出，AccessDecisionManager中包含的一系列AccessDecisionVoter将会被用来对Authentication是否有权访问受保护对象进行投票，AccessDecisionManager根据投票结果，做出最终决策。

AccessDecisionVoter是一个接口，其中定义有三个方法，具体结构如下所示。

```java
public interface AccessDecisionVoter<S> {
    int ACCESS_GRANTED = 1;
    int ACCESS_ABSTAIN = 0;
    int ACCESS_DENIED = -1;
    boolean supports(ConfigAttribute var1);
    boolean supports(Class<?> var1);
    int vote(Authentication var1, S var2, Collection<ConfigAttribute> var3);
}
```

vote()方法的返回结果会是AccessDecisionVoter中定义的三个常量之一。ACCESS_GRANTED表示同意，ACCESS_DENIED表示拒绝，ACCESS_ABSTAIN表示弃权。如果一个AccessDecisionVoter不能判定当前Authentication是否拥有访问对应受保护对象的权限，则其vote()方法的返回值应当为弃权ACCESS_ABSTAIN。

Spring Security内置了三个基于投票的AccessDecisionManager实现类如下，它们分别是**AffirmativeBased**、**ConsensusBased**和**UnanimousBased**，。

**AffirmativeBased**的逻辑是：

（1）只要有AccessDecisionVoter的投票为ACCESS_GRANTED则同意用户进行访问；

（2）如果全部弃权也表示通过；

（3）如果没有一个人投赞成票，但是有人投反对票，则将抛出AccessDeniedException。

Spring security默认使用的是AffirmativeBased。

**ConsensusBased**的逻辑是：

（1）如果赞成票多于反对票则表示通过。

（2）反过来，如果反对票多于赞成票则将抛出AccessDeniedException。

（3）如果赞成票与反对票相同且不等于0，并且属性allowIfEqualGrantedDeniedDecisions的值为true，则表示通过，否则将抛出异常AccessDeniedException。参数allowIfEqualGrantedDeniedDecisions的值默认为true。

（4）如果所有的AccessDecisionVoter都弃权了，则将视参数allowIfAllAbstainDecisions的值而定，如果该值为true则表示通过，否则将抛出异常AccessDeniedException。参数allowIfAllAbstainDecisions的值默认为false。

**UnanimousBased**的逻辑与另外两种实现有点不一样，另外两种会一次性把受保护对象的配置属性全部传递给AccessDecisionVoter进行投票，而UnanimousBased会一次只传递一个ConfigAttribute给AccessDecisionVoter进行投票。这也就意味着如果我们的AccessDecisionVoter的逻辑是只要传递进来的ConfigAttribute中有一个能够匹配则投赞成票，但是放到UnanimousBased中其投票结果就不一定是赞成了。

UnanimousBased的逻辑具体来说是这样的：

（1）如果受保护对象配置的某一个ConfigAttribute被任意的AccessDecisionVoter反对了，则将抛出AccessDeniedException。

（2）如果没有反对票，但是有赞成票，则表示通过。

（3）如果全部弃权了，则将视参数allowIfAllAbstainDecisions的值而定，true则通过，false则抛出AccessDeniedException。

Spring Security也内置一些投票者实现类如**RoleVoter**、**AuthenticatedVoter**和**WebExpressionVoter**等，可以自行查阅资料进行学习。

### 4.3 自定义认证

Spring Security提供了非常好的认证扩展方法，比如：快速上手中将用户信息存储到内存中，实际开发中用户信息通常在数据库，Spring security可以实现从数据库读取用户信息，Spring security还支持多种授权方法。

#### 4.3.1 自定义登录页面

在**快速上手**中，你可能会想知道登录页面从哪里来的？因为我们并没有提供任何的HTML或JSP文件。SpringSecurity的默认配置没有明确设定一个登录页面的URL，因此Spring Security会根据启用的功能自动生成一个登录页面URL，并使用默认URL处理登录的提交内容，登录后跳转的到默认URL等等。尽管自动生成的登录页面很方便快速启动和运行，但大多数应用程序都希望定义自己的登录页面。

##### 4.3.1.1 认证页面

将security-springmvc工程的login.jsp拷贝到security-springboot下，目录保持一致。

![img](https://pic2.zhimg.com/80/v2-30d6c13b4133c8f0a278dfff107dfded_720w.jpg)

##### 4.3.1.2 配置认证页面

在WebConfig.java中配置认证页面地址：

```java
// 默认Url根路径跳转到/login，此url为spring security提供
@Override
public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController("/").setViewName("redirect:/login-view");
    registry.addViewController("/login-view").setViewName("login");
}
```

##### **4.3.1.3 安全配置**

在WebSecurityConfig中配置表章登录信息：

```java
// 配置安全拦截机制
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
            .authorizeRequests()
            .antMatchers("/r/**").authenticated()              
            .anyRequest().permitAll()                          
            .and()
            .formLogin()                 (1)
          .loginPage("/login-view")        (2)
          .loginProcessingUrl("/login")      (3)
          .successForwardUrl("/login-success")   (4)
          .permitAll();
}
```

（1）允许表单登录

（2）指定我们自己的登录页,spring security以重定向方式跳转到/login-view

（3）指定登录处理的URL，也就是用户名、密码表单提交的目的路径

（4）指定登录成功后的跳转URL

（5）我们必须允许所有用户访问我们的登录页（例如为验证的用户），这个 formLogin().permitAll() 方法允许任意用户访问基于表单登录的所有的URL。

##### **4.3.1.4 测试**

当用户没有认证时访问系统的资源会重定向到login-view页面

![img](https://pic1.zhimg.com/80/v2-2940d876ca04000228b315401ce3d560_720w.jpg)

输入账号和密码，点击登录,报错：

![img](https://pic3.zhimg.com/80/v2-1c657fedd3eabb21c9be175dcd094eda_720w.jpg)

问题解决：

spring security为防止CSRF（Cross-site request forgery跨站请求伪造）的发生，限制了除了get以外的大多数方法。

解决方法1：

屏蔽CSRF控制，即spring security不再限制CSRF。

配置WebSecurityConfig

```java
@Override 
protected void configure(HttpSecurity http) throws Exception {
    http.csrf().disable()  //屏蔽CSRF控制，即spring security不再限制CSRF
            ...
}
```

本案例采用方法1

解决方法2：

在login.jsp页面添加一个token，spring security会验证token，如果token合法则可以继续请求。修改login.jsp

```jsp
<form action="login" method="post"> 
    <input type="hidden"  name="${_csrf.parameterName}"   value="${_csrf.token}"/>
   ...
</form>
```

#### 4.3.2 连接数据库认证

前边的例子我们是将用户信息存储在内存中，实际项目中用户信息存储在数据库中，本节实现从数据库读取用户信息。根据前边对认证流程研究，只需要重新定义UserDetailService即可实现根据用户账号查询数据库。

##### 4.3.2.1 创建数据库

创建user_db数据库

```sql
CREATE DATABASE `user_db` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
```

创建t_user表

```sql
CREATE TABLE `t_user` ( 
  `id` bigint(20) NOT NULL COMMENT '用户id',
  `username` varchar(64) NOT NULL,
  `password` varchar(64) NOT NULL,
  `fullname` varchar(255) NOT NULL COMMENT '用户姓名',
  `mobile` varchar(11) DEFAULT NULL COMMENT '手机号',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC
```

##### **4.3.2.2 代码实现**

1）定义dataSource

在application.properties配置

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/user_db 
spring.datasource.username=root
spring.datasource.password=mysql
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

2）添加依赖

```xml
<dependency> 
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

3）定义Dao

定义模型类型，在 model包定义UserDto：

```java
@Data 
public class UserDto {
    private String id;
    private String username;
    private String password;
    private String fullname;
    private String mobile;
}
```

在Dao包定义UserDao：

```java
@Repository 
public class UserDao {
    @Autowired
    JdbcTemplate jdbcTemplate;
    public UserDto getUserByUsername(String username){
        String sql ="select id,username,password,fullname from t_user where username = ?";
        List<UserDto> list = jdbcTemplate.query(sql, new Object[]{username}, new
BeanPropertyRowMapper<>(UserDto.class));
        if(list == null && list.size() <= 0){
            return null;
        }
        return list.get(0);
    }
}
```

##### **4.3.2.3 定义UserDetailService**

在service包下定义SpringDataUserDetailsService：

```java
@Service 
public class SpringDataUserDetailsService implements UserDetailsService {
    @Autowired
    UserDao userDao;
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //登录账号
        System.out.println("username="+username);
        //根据账号去数据库查询...
        UserDto user = userDao.getUserByUsername(username);
        if(user == null){
            return null;
        }
        // 这里暂时使用静态数据
        UserDetails userDetails =
User.withUsername(user.getFullname()).password(user.getPassword()).authorities("p1").build();
        return userDetails;
    }
}
```

##### **4.3.2.4 测试**

输入账号和密码请求认证，跟踪代码。

##### **4.3.2.5 使用BCryptPasswordEncoder**

按照我们前边讲的PasswordEncoder的使用方法，使用BCryptPasswordEncoder需要完成如下工作：

1、在安全配置类中定义BCryptPasswordEncoder

```java
@Bean 
public PasswordEncoder passwordEncoder() {
    return  new BCryptPasswordEncoder();
}
```

2、UserDetails中的密码存储BCrypt格式

前边实现了从数据库查询用户信息，所以数据库中的密码应该存储BCrypt格式

![img](https://pic4.zhimg.com/80/v2-d723b3fb74afcc6ebe9c8dd2a0e8aa83_720w.png)

### 4.4 会话

用户认证通过后，为了避免用户的每次操作都进行认证可将用户的信息保存在会话中。spring security提供会话管理，认证通过后将身份信息放入SecurityContextHolder上下文，SecurityContext与当前线程进行绑定，方便获取用户身份。

#### 4.4.1.获取用户身份

编写LoginController，实现/r/r1、/r/r2的测试资源，并修改loginSuccess方法，注意getUsername方法，SpringSecurity获取当前登录用户信息的方法为SecurityContextHolder.getContext().getAuthentication()

```java
@RestController
public class LoginController {
    /**
     * 用户登录成功
     * @return
     */
    @RequestMapping(value = "/login-success",produces = {"text/plain;charset=UTF-8"})
    public String loginSuccess(){
        String username = getUsername();
        return username + " 登录成功";
    }
   /**
     * 获取当前登录用户名
     * @return
     */
    private String getUsername(){
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        if(!authentication.isAuthenticated()){
            return null;
        }
        Object principal = authentication.getPrincipal();
        String username = null;
        if (principal instanceof org.springframework.security.core.userdetails.UserDetails) {
            username =
((org.springframework.security.core.userdetails.UserDetails)principal).getUsername();
        } else {
            username = principal.toString();
        }
        return username;
    }
    /**
     * 测试资源1
     * @return
     */
    @GetMapping(value = "/r/r1",produces = {"text/plain;charset=UTF-8"})
    public String r1(){
        String username = getUsername();
        return username + " 访问资源1";
    }
    /**
     * 测试资源2
     * @return
     */
    @GetMapping(value = "/r/r2",produces = {"text/plain;charset=UTF-8"})
    public String r2(){
        String username = getUsername();
        return username + " 访问资源2";
    }
}
```

测试

登录前访问资源

被重定向至登录页面。

登录后访问资源

成功访问资源，如下：

![img](https://pic2.zhimg.com/80/v2-beea5431aba8a62b647fd01389fce459_720w.png)

#### 4.4.2会话控制

我们可以通过以下选项准确控制会话何时创建以及Spring Security如何与之交互：

![img](https://pic1.zhimg.com/80/v2-2aca20d190e76197b9ca17f24b57201c_720w.jpg)

通过以下配置方式对该选项进行配置：

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
   http.sessionManagement()
        .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
}
```

默认情况下，Spring Security会为每个登录成功的用户会新建一个Session，就是**ifRequired** 。

若选用**never**，则指示Spring Security对登录成功的用户不创建Session了，但若你的应用程序在某地方新建了session，那么Spring Security会用它的。

若使用**stateless**，则说明Spring Security对登录成功的用户不会创建Session了，你的应用程序也不会允许新建session。并且它会暗示不使用cookie，所以每个请求都需要重新进行身份验证。这种无状态架构适用于REST API及其无状态认证机制。

##### 会话超时

可以再sevlet容器中设置Session的超时时间，如下设置Session有效期为3600s；

spring boot 配置文件：

```text
server.servlet.session.timeout=3600s
```

session超时之后，可以通过Spring Security 设置跳转的路径。

```text
http.sessionManagement()
    .expiredUrl("/login-view?error=EXPIRED_SESSION")
    .invalidSessionUrl("/login-view?error=INVALID_SESSION");
```

expired指session过期，invalidSession指传入的sessionid无效。

##### **安全会话cookie**

我们可以使用httpOnly和secure标签来保护我们的会话cookie：

- **httpOnly** ：如果为true，那么浏览器脚本将无法访问cookie
- **secure** ：如果为true，则cookie将仅通过HTTPS连接发送

spring boot 配置文件：

```text
server.servlet.session.cookie.http-only=true
server.servlet.session.cookie.secure=true
```

### 4.6 退出

Spring security默认实现了logout退出，访问/logout，果然不出所料，退出功能Spring也替我们做好了。

![img](https://pic2.zhimg.com/80/v2-2eb7e2bf792c06a19c18f2858d8555d9_720w.jpg)

点击“Log Out”退出 成功。

退出 后访问其它url判断是否成功退出。

这里也可以自定义退出成功的页面：

在WebSecurityConfig的protected void configure(HttpSecurity http)中配置：

```text
.and() 
.logout()
.logoutUrl("/logout")
.logoutSuccessUrl("/login-view?logout");
```

当退出操作出发时，将发生：

- 使 HTTP Session 无效
- 清除 SecurityContextHolder
- 跳转到 /login -view?logout

但是，类似于配置登录功能，咱们可以进一步自定义退出功能：

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests()
       //...      
            .and()
            .logout()                                                (1)
            .logoutUrl("/logout")                                    (2)
            .logoutSuccessUrl("/login-view?logout")                  (3)
            .logoutSuccessHandler(logoutSuccessHandler)              (4)
            .addLogoutHandler(logoutHandler)                         (5)      
            .invalidateHttpSession(true);                            (6)
               
}
```

（1）提供系统退出支持，使用 WebSecurityConfigurerAdapter 会自动被应用

（2）设置触发退出操作的URL (默认是 /logout ).

（3）退出之后跳转的URL。默认是 /login?logout 。

（4）定制的 LogoutSuccessHandler ，用于实现用户退出成功时的处理。如果指定了这个选项那么logoutSuccessUrl() 的设置会被忽略。

（5）添加一个 LogoutHandler ，用于实现用户退出时的清理工作.默认 SecurityContextLogoutHandler 会被添加为最后一个 LogoutHandler 。

（6）指定是否在退出时让 HttpSession 无效。 默认设置为 **true**。

**注意：如果让logout在GET请求下生效，必须关闭防止CSRF攻击csrf().disable()。如果开启了CSRF，必须使用post方式请求/logout**

**logoutHandler**：

一般来说， LogoutHandler 的实现类被用来执行必要的清理，因而他们不应该抛出异常。

下面是Spring Security提供的一些实现：

- PersistentTokenBasedRememberMeServices 基于持久化token的RememberMe功能的相关清理
- TokenBasedRememberMeService 基于token的RememberMe功能的相关清理
- CookieClearingLogoutHandler 退出时Cookie的相关清理
- CsrfLogoutHandler 负责在退出时移除csrfToken
- SecurityContextLogoutHandler 退出时SecurityContext的相关清理

链式API提供了调用相应的 LogoutHandler 实现的快捷方式，比如deleteCookies()。

### 4.7 授权

#### **4.7.1 概述**

授权的方式包括 web授权和方法授权，web授权是通过 url拦截进行授权，方法授权是通过 方法拦截进行授权。他们都会调用accessDecisionManager进行授权决策，若为web授权则拦截器为FilterSecurityInterceptor；若为方法授权则拦截器为MethodSecurityInterceptor。如果同时通过web授权和方法授权则先执行web授权，再执行方法授权，最后决策通过，则允许访问资源，否则将禁止访问。

类关系如下：

![img](https://pic2.zhimg.com/80/v2-ea3249cce876629bd5da84421484fd39_720w.jpg)

#### 4.7.2.准备环境

##### **4.7.2.1 数据库环境**

在t_user数据库创建如下表：

角色表：

```sql
CREATE TABLE `t_role` ( 
  `id` varchar(32) NOT NULL,
  `role_name` varchar(255) DEFAULT NULL,
  `description` varchar(255) DEFAULT NULL,
  `create_time` datetime DEFAULT NULL,
  `update_time` datetime DEFAULT NULL,
  `status` char(1) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `unique_role_name` (`role_name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
insert  into `t_role`(`id`,`role_name`,`description`,`create_time`,`update_time`,`status`) values
('1','管理员',NULL,NULL,NULL,'');
```

用户角色关系表：

```sql
CREATE TABLE `t_user_role` ( 
  `user_id` varchar(32) NOT NULL,
  `role_id` varchar(32) NOT NULL,
  `create_time` datetime DEFAULT NULL,
  `creator` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`user_id`,`role_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
insert  into `t_user_role`(`user_id`,`role_id`,`create_time`,`creator`) values
('1','1',NULL,NULL);
```

权限表：

```sql
CREATE TABLE `t_permission` ( 
  `id` varchar(32) NOT NULL,
  `code` varchar(32) NOT NULL COMMENT '权限标识符',
  `description` varchar(64) DEFAULT NULL COMMENT '描述',
  `url` varchar(128) DEFAULT NULL COMMENT '请求地址',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
insert  into `t_permission`(`id`,`code`,`description`,`url`) values ('1','p1','测试资源
1','/r/r1'),('2','p3','测试资源2','/r/r2');
```

角色权限关系表：

```sql
CREATE TABLE `t_role_permission` ( 
  `role_id` varchar(32) NOT NULL,
  `permission_id` varchar(32) NOT NULL,
  PRIMARY KEY (`role_id`,`permission_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
insert  into `t_role_permission`(`role_id`,`permission_id`) values ('1','1'),('1','2');
```

##### **4.7.2.2 修改UserDetailService**

1、修改dao接口

在UserDao中添加：

```java
// 根据用户id查询用户权限
public List<String> findPermissionsByUserId(String userId){
    String sql="SELECT * FROM t_permission WHERE id IN(\n" +
            "SELECT permission_id FROM t_role_permission WHERE role_id IN(\n" +
            "\tSELECT role_id FROM t_user_role WHERE user_id = ? \n" +
            ")\n" +
            ")";
    List<PermissionDto> list = jdbcTemplate.query(sql, new Object[]{userId}, new
BeanPropertyRowMapper<>(PermissionDto.class));
    List<String> permissions = new ArrayList<>();
    list.iterator().forEachRemaining(c->permissions.add(c.getCode()));
    return permissions;
}
```

2、修改UserDetailService

实现从数据库读取权限

```java
@Override 
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
    //登录账号
    System.out.println("username="+username);
    //根据账号去数据库查询...
    UserDto user = userDao.getUserByUsername(username);
    if(user == null){
        return null;
    }
    //查询用户权限
    List<String> permissions = userDao.findPermissionsByUserId(user.getId());
    String[] perarray = new String[permissions.size()];
    permissions.toArray(perarray);
    //创建userDetails
    UserDetails userDetails =
User.withUsername(user.getFullname()).password(user.getPassword()).authorities(perarray).build();
    return userDetails;
}
```

##### **4.7.2.web授权**

在上面例子中我们完成了认证拦截，并对/r/**下的某些资源进行简单的授权保护，但是我们想进行灵活的授权控制该怎么做呢？通过给 http.authorizeRequests() 添加多个子节点来定制需求到我们的URL，如下代码：

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
            .authorizeRequests()                                                           (1)
            .antMatchers("/r/r1").hasAuthority("p1")                                       (2)
            .antMatchers("/r/r2").hasAuthority("p2")                                       (3)
            .antMatchers("/r/r3").access("hasAuthority('p1') and hasAuthority('p2')")      (4)
            .antMatchers("/r/**").authenticated()                                          (5)
            .anyRequest().permitAll()                                                      (6)  
            .and()      
            .formLogin()
        // ...      
}
```

（1） http.authorizeRequests() 方法有多个子节点，每个macher按照他们的声明顺序执行。

（2）指定"/r/r1"URL，拥有p1权限能够访问

（3）指定"/r/r2"URL，拥有p2权限能够访问

（4）指定了"/r/r3"URL，同时拥有p1和p2权限才能够访问

（5）指定了除了r1、r2、r3之外"/r/**"资源，同时通过身份认证就能够访问，这里使用SpEL（Spring Expression Language）表达式。。

（6）剩余的尚未匹配的资源，不做保护。

**注意：**

**规则的顺序是重要的,更具体的规则应该先写**.现在以/ admin开始的所有内容都需要具有ADMIN角色的身份验证用户,即使是/ admin / login路径(因为/ admin / login已经被/ admin / **规则匹配,因此第二个规则被忽略).

```text
.antMatchers("/admin/**").hasRole("ADMIN") 
.antMatchers("/admin/login").permitAll()
```

因此,登录页面的规则应该在/ admin / **规则之前.例如.

```text
.antMatchers("/admin/login").permitAll() 
.antMatchers("/admin/**").hasRole("ADMIN")
```

保护URL常用的方法有：

**authenticated**() 保护URL，需要用户登录

**permitAll**() 指定URL无需保护，一般应用与静态资源文件

**hasRole**(String role) 限制单个角色访问，角色将被增加 “ROLE_” .所以”ADMIN” 将和 “ROLE_ADMIN”进行比较.

**hasAuthority**(String authority) 限制单个权限访问

**hasAnyRole**(String… roles)允许多个角色访问.

**hasAnyAuthority**(String… authorities) 允许多个权限访问.

**access**(String attribute) 该方法使用 SpEL表达式, 所以可以创建复杂的限制.

**hasIpAddress**(String ipaddressExpression) 限制IP地址或子网

#### **4.7.3.方法授权**

现在我们已经掌握了使用如何使用 http.authorizeRequests() 对web资源进行授权保护，从Spring Security2.0版本开始，它支持服务层方法的安全性的支持。本节学习@PreAuthorize,@PostAuthorize, @Secured三类注解。

我们可以在任何 @Configuration 实例上使用 @EnableGlobalMethodSecurity 注释来启用基于注解的安全性。

以下内容将启用Spring Security的 @Secured 注释。

```java
@EnableGlobalMethodSecurity(securedEnabled = true) 
public class MethodSecurityConfig {// ...}
```

然后向方法（在类或接口上）添加注解就会限制对该方法的访问。 Spring Security的原生注释支持为该方法定义了一组属性。 这些将被传递给AccessDecisionManager以供它作出实际的决定：

```text
public interface BankService {
@Secured("IS_AUTHENTICATED_ANONYMOUSLY")
public Account readAccount(Long id);
@Secured("IS_AUTHENTICATED_ANONYMOUSLY")
public Account[] findAccounts();
@Secured("ROLE_TELLER")
public Account post(Account account, double amount);
}
```

以上配置标明readAccount、findAccounts方法可匿名访问，底层使用WebExpressionVoter投票器，可从AffirmativeBased第23行代码跟踪。。

post方法需要有TELLER角色才能访问，底层使用RoleVoter投票器。

使用如下代码可启用prePost注解的支持

```java
@EnableGlobalMethodSecurity(prePostEnabled = true) 
public class MethodSecurityConfig {
// ...
}
```

相应Java代码如下：

```java
public interface BankService {
@PreAuthorize("isAnonymous()")
public Account readAccount(Long id);
@PreAuthorize("isAnonymous()")
public Account[] findAccounts();
@PreAuthorize("hasAuthority('p_transfer') and hasAuthority('p_read_account')")
public Account post(Account account, double amount);
}
```

以上配置标明readAccount、findAccounts方法可匿名访问，post方法需要同时拥有p_transfer和p_read_account权限才能访问，底层使用WebExpressionVoter投票器，可从AffirmativeBased第23行代码跟踪。



## 5、分布式系统认证方案



## 6、OAuth2.0

### 6.1 OAuth2.0介绍

OAuth（开放授权）是一个开放标准，允许用户授权第三方应用访问他们存储在另外的服务提供者上的信息，而不需要将用户名和密码提供给第三方应用或分享他们数据的所有内容。OAuth2.0是OAuth协议的延续版本，但不向后兼容OAuth 1.0即完全废止了OAuth1.0。很多大公司如Google，Yahoo，Microsoft等都提供了OAUTH认证服务，这些都足以说明OAUTH标准逐渐成为开放资源授权的标准。

Oauth协议目前发展到2.0版本，1.0版本过于复杂，2.0版本已得到广泛应用。

参考：[oAuth_百度百科](https://link.zhihu.com/?target=https%3A//baike.baidu.com/item/oAuth/7153134%3Ffr%3Daladdin)

Oauth 协议：[https://tools.ietf.org/html/rfc6749](https://link.zhihu.com/?target=https%3A//tools.ietf.org/html/rfc6749)

下边分析一个 Oauth2认证的例子，通过例子去理解OAuth2.0协议的认证流程，本例子是某个客户端网站使用微信认证的过程，这个过程的简要描述如下：

用户借助微信认证登录客户端网站，用户就不用单独在客户端注册用户，怎么样算认证成功吗？客户端网站需要成功从微信获取用户的身份信息则认为用户认证成功，那如何从微信获取用户的身份信息？用户信息的拥有者是用户本人，微信需要经过用户的同意方可为客户端网站生成令牌，客户端网站拿此令牌方可从微信获取用户的信息。

1、客户端请求第三方授权

用户进入客户端的登录页面，点击微信的图标以微信账号登录系统，用户是自己在微信里信息的资源拥有者。

![img](https://pic1.zhimg.com/80/v2-89058e1e5b048b5da57306471b675b0c_720w.jpg)



点击“微信”出现一个二维码，此时用户扫描二维码，开始给客户端网站授权。

[https://open.weixin.qq.com/connect/confirm?uuid=081HotoNCFsqdaOu](https://link.zhihu.com/?target=https%3A//open.weixin.qq.com/connect/confirm%3Fuuid%3D081HotoNCFsqdaOu) (二维码自动识别)

2、资源拥有者同意给客户端授权

资源拥有者扫描二维码表示资源拥有者同意给客户端授权，微信会对资源拥有者的身份进行验证， 验证通过后，微信会询问用户是否给授权客户端网站访问自己的微信数据，用户点击“确认登录”表示同意授权，微信认证服务器会颁发一个授权码，并重定向到客户端的网站。

![img](https://pic3.zhimg.com/v2-2154fbda03f941407ea6dc5bf24a0b16_r.jpg)

3、客户端获取到授权码，请求认证服务器申请令牌:

此过程用户看不到，客户端应用程序请求认证服务器，请求携带授权码。

4、认证服务器向客户端响应令牌:

微信认证服务器验证了客户端请求的授权码，如果合法则给客户端颁发令牌，令牌是客户端访问资源的通行证。此交互过程用户看不到，当客户端拿到令牌后，用户在客户端网站看到已经登录成功。

5、客户端请求资源服务器的资源

客户端携带令牌访问资源服务器的资源。

客户端网站携带令牌请求访问微信服务器获取用户的基本信息。

6、资源服务器返回受保护资源

资源服务器校验令牌的合法性，如果合法则向用户响应资源信息内容。

以上认证授权详细的执行流程如下：


![img](https://pic1.zhimg.com/80/v2-42012141d4365664ebfb588158546ec0_720w.jpg)

通过上边的例子我们大概了解了OAauth2.0的认证过程，下边我们看OAuth2.0认证流程：

引自OAauth2.0协议rfc6749 [The OAuth 2.0 Authorization Framework](https://link.zhihu.com/?target=https%3A//tools.ietf.org/html/rfc6749)


![img](https://pic2.zhimg.com/80/v2-29a12b0b101bfc25ed96657950ca5ae9_720w.jpg)

OAauth2.0包括以下角色：

1、客户端

本身不存储资源，需要通过资源拥有者的授权去请求资源服务器的资源，比如：Android客户端、Web客户端（浏览器端）、微信客户端等。

2、资源拥有者

通常为用户，也可以是应用程序，即该资源的拥有者。

3、授权服务器（也称认证服务器）

用于服务提供商对资源拥有的身份进行认证、对访问资源进行授权，认证成功后会给客户端发放令牌（access_token），作为客户端访问资源服务器的凭据。本例为微信的认证服务器。

4、资源服务器

存储资源的服务器，本例子为微信存储的用户信息。

现在还有一个问题，服务提供商能允许随便一个**客户端**就接入到它的**授权服务器**吗？答案是否定的，服务提供商会给准入的接入方一个身份，用于接入时的凭据:

**client_id**：客户端标识 

**client_secret**：客户端秘钥

因此，准确来说，**授权服务器**对两种OAuth2.0中的两个角色进行认证授权，分别是资源**拥有者、客户端**。



### 6.2 Spring Cloud Security OAuth2

#### 6.2.1 环境介绍

Spring-Security-OAuth2是对OAuth2的一种实现，并且跟我们之前学习的Spring-Security相辅相成，与Spring Cloud体系的集成也非常便利，接下来，我们需要对它进行学习，最终使用它来实现我们设计的分布式认证授权解决方案。

OAuth2.0的服务提供方涵盖两个服务，即授权服务 (Authorization Server，也叫认证服务) 和资源服务 (Resource Server)，使用 Spring Security OAuth2 的时候你可以选择把它们在同一个应用程序中实现，也可以选择建立使用同一个授权服务的多个资源服务。

**授权服务（认证服务Authorization Server）** 应包含对接入端以及登入用户的合法性进行验证并颁发token等功能，对令牌的请求端点由 Spring MVC 控制器进行实现，下面是配置一个认证服务必须要实现的endpoints：

- **AuthorizationEndpoint** 服务于认证请求。默认 URL： /oauth/authorize 。
- **TokenEndpoint** 服务于访问令牌的请求。默认 URL： /oauth/token 。

**资源服务** **(Resource Server)**  应包含对资源的保护功能，对非法请求进行拦截，对请求中token进行解析鉴权等，下面的过滤器用于实现 OAuth 2.0 资源服务：

- OAuth2AuthenticationProcessingFilter 用来对请求给出的身份令牌解析鉴权。

本教程分别创建uaa授权服务（也可叫认证服务）和order订单资源服务。

![img](https://pic3.zhimg.com/80/v2-3f47ea460a9c228924968e6941c8390a_720w.jpg)

认证流程如下：

1、客户端请求UAA授权服务进行认证。

2、认证通过后由UAA颁发令牌。

3、客户端携带令牌Token请求资源服务。

4 、资源服务校验令牌的合法性，合法即返回资源信息。

#### 6.2.2 环境搭建

##### 6.2.2.1 父工程

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifictId>spring-boot-starter-parent</artifictId>
    <version>2.1.3.RELEASE</version>
</parent>
<dependencyManagement>
    <dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Greenwich.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
    </dependency>
    <dependency>
    	<groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.interceptor</groupId>
        <artifactId>javax.interceptor-api</artifactId>
        <version>1.2</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.47</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.0</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-jwt</artifactId>
        <version>1.0.10.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.security.oauth.boot</groupId>
        <artifactId>spring-security-oauth2-autoconfigure</artifactId>
        <version>2.1.3.RELEASE</version>
    </dependency>
    </dependencies>
</dependencyManagement>
```

##### 6.2.2.2 创建UAA授权服务工程

1、创建distributed-security-uaa认证服务

```xml
<project>
    <dependency>
	    <groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
	</dependency>

    <dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-openfeign</artifactId>
	</dependency>
	<dependency>
		<groupId>com.netflix.hystrix</groupId>
		<artifactId>hystrix-javanica</artifactId>
	</dependency>
    <dependency>
        <groupId>org.springframework.retry</groupId>
        <artifactId>spring-retry</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-freemarker</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring-data-commons</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-oauth2</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-jwt</artifactId>
    </dependency>
</project>
```

2、启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableHystrix
@EnableFeignClients(basePackages={"com.xx.security.distribute.uaa"})
public class UaaServer{
    public static  void main(String[] args){
        SpringApplication.run(UaaServer.class, args);
    }
}
```

3、配置文件 application.properties

```properties
spring.application.name=uaa-service
server.port=53020
spring.main.allow-bean-definition-overriding = true

logging.level.root = debug
logging.level.org.springframework.web = info

spring.http.encoding.enabled = true
spring.http.encoding.charset = UTF-8
spring.http.encoding.force = true

server.tomcat.remote_ip_header = x-forwarded-for
server.tomcat.protocol_header = x-forwarded-proto
server.use-forward-headers = true
server.servlet.context-path = /uaa

spring.freemarker.enabled = true
spring.freemarker.suffix = .html
spring.freemarker.request-context-attribute = rc
spring.freemarker.content-type = text/html
spring.freemarker.charset = UTF-8
spring.mvc.throw-exception-if-no-handler-found = true
spring.resources.add-mappings = false
spring.datasource.url = jdbc:mysql://localhost:3306/user_db?useUnicode=true
spring.datasource.username = root
spring.datasource.password = mysql
spring.datasource.driver-class-name = com.mysql.jdbc.Driver
#eureka.client.serviceUrl.defaultZone = http://localhost:53000/eureka/
#eureka.instance.preferIpAddress = true
#eureka.instance.instance-id = ${spring.application.name}:${spring.cloud.client.ip-address}:${spring.application.instance_id:${server.port}}
management.endpoints.web.exposure.include = refresh,health,info,env
feign.hystrix.enabled = true
feign.compression.request.enabled = true
feign.compression.request.mime-types[0] = text/xml
feign.compression.request.mime-types[1] = application/xml
feign.compression.request.mime-types[2] = application/json
feign.compression.request.min-request-size = 2048
feign.compression.response.enabled = true
```

##### 6.2.2.3 创建Order资源服务工程

1、导入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-
4.0.0.xsd">
    <parent>
        <artifactId>distributed-security</artifactId>
        <groupId>com.lw.security</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>distributed-security-order</artifactId> 
    <dependencies>
        <!--<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-oauth2</artifactId>
        </dependency>
        <dependency>
            <groupId>javax.interceptor</groupId>
            <artifactId>javax.interceptor-api</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
</project>
```

2、工程结构

![img](https://pic2.zhimg.com/80/v2-de99972446c8f95175f4c0b56f38c2a9_720w.jpg)

2、启动类

```java
@SpringBootApplication
public class ServerOrderApplication {
    public static void main(String[] args) {
        SpringApplication.run(ServerOrderApplication.class, args);
    }
}

```

3、配置文件

[在resources中创建application.properties](https://link.zhihu.com/?target=http%3A//xn--resourcesapplication-j545am19cnr0aeu5b.properties/)

```properties
spring.application.name=order-service
server.port=53021
spring.main.allow-bean-definition-overriding = true

logging.level.root = debug
logging.level.org.springframework.web = info

spring.http.encoding.enabled = true
spring.http.encoding.charset = UTF-8
spring.http.encoding.force = true

server.tomcat.remote_ip_header = x-forwarded-for
server.tomcat.protocol_header = x-forwarded-proto
server.use-forward-headers = true
server.servlet.context-path = /order

spring.freemarker.enabled = true
spring.freemarker.suffix = .html
spring.freemarker.request-context-attribute = rc
spring.freemarker.content-type = text/html
spring.freemarker.charset = UTF-8
spring.mvc.throw-exception-if-no-handler-found = true
spring.resources.add-mappings = false

#eureka.client.serviceUrl.defaultZone = http://localhost:53000/eureka/
#eureka.instance.preferIpAddress = true
#eureka.instance.instance-id = ${spring.application.name}:${spring.cloud.client.ip-address}:${spring.application.instance_id:${server.port}}
management.endpoints.web.exposure.include = refresh,health,info,env

feign.hystrix.enabled = true
feign.compression.request.enabled = true
feign.compression.request.mime-types[0] = text/xml
feign.compression.request.mime-types[1] = application/xml
feign.compression.request.mime-types[2] = application/json
feign.compression.request.min-request-size = 2048
feign.compression.response.enabled = true
```

#### 6.2.3 授权服务器配置

##### 6.2.3.1 EnableAuthorizationServer

可以用@EnableAuthorizationServer注解并继承AuthorizationServerConfigureAdapter来配置OAuth2.0授权服务器

在config包下创建AuthorizationServer

```java
@Configuration
@EnableAuthorizationServer
public class AuthorizationServer extends {
    AuthorizationSererConfigureAdapter{
        
    }
}
```

AuthorizationServerConfigureAdapter 要求配置一下几个类，这几个类是由Spring创建的独立的配置对象，它们会被`Spring`传入`AuthorizationServerConfigurer`中进行配置。

```java
public class AuthorizationServerConfigurerAdapter implements AuthorizationServerConfigurer { 
	public AuthorizationServerConfigurerAdapter(){}
	public void configure(AuthorizationServerSecurityConfigurer security){}
	public void configure(ClientDetailsServiceConfigurer clients) {}
	public void configure(AuthorizationServerEndpointsConfigurer endpoints){}
}
```

- **ClientDetailsServiceConfigurer**:用来配置客户端详情服务（`ClientDetailsService`），客户端详情信息在这里进行初始化，你能够把客户端详情信息写死在这里或者是通过数据库来存储调取详情信息
- **AuthorizationServerEndpointsConfigurer**:用来配置令牌（`token`）的访问端点和令牌服务(`token services`)
- **AuthorizationServerSecurityConfigurer**:用来配置令牌端点的安全约束

##### 6.2.3.2 配置客户端详细信息

```java
@Configuration
@EnableAuthorizationServer
pubic class AuthorizationServer extends AuthorizationServerConfigurerAdapter {
    //配置客户端详细信息服务
    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        clients.inMemory()	//是由内存方式
                .withClient("c1")	//client_id
                .secret(new BCryptPasswordEncoder().encode("secret"))
                .resourceIds("res1")
                .authorizedGrantTypes("authorization_code","password","client_credentials","implicit","refresh_token")  //该client允许的授权类型 五种授权类型
                .scopes("all") //允许的授权范围
                .autoApprove(false)  //false 跳转到授权页面
                .redirectUris("http://www.baidu.com");
    }
}
```

##### 6.2.3.3 管理令牌

AuthorizationServerTokenService接口定义了一些操作使得你可以对令牌进行一些必要的管理，令牌可以被用来加载身份信息，里面包含了这个令牌的相关权限。

自己可以创建 AuthorizationServerTokenServices 这个接口的实现，需要继承DefaultTokenServices，里面包含了一些有用实现，你可以使用它来修改令牌的格式和令牌的存储。默认的，当它尝试创建一个令牌的时候，是使用随机值来进行填充的，除了持久化令牌是委托一个 TokenStore 接口来实现以外，这个类几乎帮你做了所有的事情。并且 TokenStore 这个接口有一个默认的实现，它就是 InMemoryTokenStore ，如其命名，所有的令牌是被保存在了内存中。除了使用这个类以外，你还可以使用一些其他的预定义实现，下面有几个版本，它们都实现了TokenStore接口：

- **InMemoryTokenStore：**这个版本的实现是被默认采用的，它可以完美的工作在单服务器上（即访问并发量压力不大的情况下，并且它在失败的时候不会进行备份），大多数的项目都可以使用这个版本的实现来进行尝试，你可以在开发的时候使用它来进行管理，因为不会被保存到磁盘中，所以更易于调试。
- **JdbcTokenStore：**这是一个基于JDBC的实现版本，令牌会被保存进关系型数据库。使用这个版本的实现时，你可以在不同的服务器之间共享令牌信息，使用这个版本的时候请注意把"spring-jdbc"这个依赖加入到你的classpath当中。
- **JwtTokenStore：**这个版本的全称是 JSON Web Token（JWT），它可以把令牌相关的数据进行编码（因此对于后端服务来说，它不需要进行存储，这将是一个重大优势），但是它有一个缺点，那就是撤销一个已经授权令牌将会非常困难，所以它通常用来处理一个生命周期较短的令牌以及撤销刷新令牌（refresh_token）。另外一个缺点就是这个令牌占用的空间会比较大，如果你加入了比较多用户凭证信息。JwtTokenStore 不会保存任何数据，但是它在转换令牌值以及授权信息方面与 DefaultTokenServices 所扮演的角色是一样的。

**如何配置令牌服务**

1、定义TokenConfig

在config包下定义TokenConfig，我暂时使用InMemoryTokenStore，生成一个普通的令牌

```java
@Configuration
public class TokenConfig {

    //令牌存储策略 暂时使用内存方式
    @Bean
    public TokenStore tokenStore(){
        return  new InMemoryTokenStore();
    }
}
```

2、定义AuthorizationServerTokenServices

在AuthorizationServer中定义AuthorizationServerTokenServices

```java

    @Autowired
    private TokenStore tokenStore;

    @Autowired
    private ClientDetailsService clientDetailsService;

    @Bean
    public AuthorizationServerTokenServices tokenServices(){
        DefaultTokenServices services = new DefaultTokenServices();
        services.setClientDetailsService(clientDetailsService);
        services.setSupportRefreshToken(true);//是否产生刷新令牌
        services.setTokenStore(tokenStore);
        services.setAccessTokenValiditySeconds(7200); //令牌默认有效期2小时
        services.setRefreshTokenValiditySeconds(259200); //刷新令牌有效期三天
        return services;
    }
```

##### 6.2.3.4 令牌访问端点配置

AuthorizationServerEndpointsConfigurer这个对象的实例可以完成令牌服务以及令牌endpoint配置

###### 配置授权类型（Grant Types）

AuthorizationServerEndpointsConfigurer通过设定以下属性决定支持的 **授权类型（GrantTypes）**

- **authenticationManager：**认证管理器，当你选择了资源所有者密码（password）授权类型的时候，请设置这个属性注入一个 AuthenticationManager 对象。
- **userDetailsService：**如果你设置了这个属性的话，那说明你有一个自己的 UserDetailsService 接口的实现，或者你可以把这个东西设置到全局域上面去（例如 GlobalAuthenticationManagerConfigurer 这个配置对象），当你设置了这个之后，那么 "refresh_token" 即刷新令牌授权类型模式的流程中就会包含一个检查，用来确保这个账号是否仍然有效，假如说你禁用了这个账户的话。
- **authorizationCodeServices：**这个属性是用来设置授权码服务的（即 AuthorizationCodeServices 的实例对象），主要用于 "authorization_code" 授权码类型模式。
- **implicitGrantService：**这个属性用于设置隐式授权模式，用来管理隐式授权模式的状态。
- **tokenGranter：**当你设置了这个属性（即 TokenGranter 接口实现），那么授权将会交由你来完全掌控，并且会忽略掉上面的这几个属性，这个属性一般是用作拓展用途的，即标准的四种授权模式已经满足不了你的需求的时候，才会考虑使用这个。

###### 配置授权端点的URL（Endpoint URLs）:

AuthorizationServerEndpointsConfigurer这个对象有一个叫做pathMapping()的方法用来配置端点URL链接，它有两个参数：

- 第一个参数：String类型，这个端点URL的默认链接。
- 第二个参数：String类型，你要进行替换的URL链接。

以上的参数都将以 "/" 字符为开始的字符串，框架的默认URL链接如下列表，可以作为这个 pathMapping() 方法的第一个参数：

- /oauth/authorize：授权端点。
- /oauth/token：令牌端点。
- /oauth/confirm_access：用户确认授权提交端点。
- /oauth/error：授权服务错误信息端点。
- /oauth/check_token：用于资源服务访问的令牌解析端点。
- /oauth/token_key：提供公有密匙的端点，如果你使用JWT令牌的话。

需要注意的是授权端点这个URL应该被Spring Security保护起来只供授权用户访问。

在AuthorizationServer配置令牌访问端点

```java
@Autowired
private AuthorizationCodeServices authorizationCodeServices;
    
@Autowired
private AuthenticationManager authenticationManager;

@Override
public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
        endpoints.authenticationManager(authenticationManager) //如果采用密码模式 需要配置此项
                .authorizationCodeServices(authorizationCodeServices) //授权码模式
                .tokenServices(tokenServices()) //令牌管理服务 不管什么模式都需要
                .allowedTokenEndpointRequestMethods(HttpMethod.POST);
}
```

##### 6.2.3.5 令牌端点的安全约束

**AuthorizationServerSecurityConfigurer：**用来配置令牌端点（Token Endpoint）的安全约束，在AuthorizationServer中配置如下

```java
   //4 令牌端点的安全约束
    @Override
    public void configure(AuthorizationServerSecurityConfigurer security){
        security.tokenKeyAccess("permitAll()")	        //(1)
                .checkTokenAccess("permitAll()")		//(2)
                .allowFormAuthenticationForClients();	//(3)
    }
```

（1）tokenkey这个endpoint当使用jwtToken且使用非对称加密时，资源服务用于获取公钥而开放的，这里指这个endpoint完全公开

（2）checkToken这个endpoint完全公开

（3）允许表单验证

**授权服务配置总结：**

授权配置分为三大板块，可以关联记忆。

既然要完成认证，它首先得知道客户端信息从哪儿读取，一次要进行客户端详情配置。

既然要颁发token，那么必须得定义token的相关endpoint，以及tokern如何存取，以及客户端支持哪些类型的token。

既然要爆了除了一些endpoint，那么对这些endpoint可以定义一些安全上的约束等。

##### 6.2.3.6 web安全配置

WebSecurityConfig

```java
@Configuration
@EnableGlobalMethodSecurity(securedEnabled = true,prePostEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .authorizeRequests()
                .antMatchers("/r/**")
                .authenticated()
                .anyRequest().permitAll()
                .and()
                .formLogin()
                .loginPage("/login-view")   //允许登录表单
                .loginProcessingUrl("/login")   //登录页面
                .successForwardUrl("/login-success") //自定义登录成功的页面地址
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
                .and()
                .logout()
                .logoutUrl("/logout")
                .logoutSuccessUrl("/login-view?logout");
    }
}
```

#### 6.2.4 授权码模式

##### 6.2.4.1 授权码模式介绍

授权码模式交互图

![img](https://img-blog.csdnimg.cn/20200515143145439.png)

（1） 资源拥有者打开客户端，客户端要求资源拥有者给予授权，它将浏览器被重定向到授权服务器，重定向时，会附件客户端的身份信息。如：

```bash
http://localhost:53020/uaa/oauth/authorize?client_id=c1&response_type=code&scope=all&redirect_url=http://www.baidu.com
```


参数列表如下：

- client_id：客户端准入标识
- response_type：授权码模式固定为`code`
- scope：客户端权限
- redirect_uri：跳转uri，当授权码申请成功后会跳转到此地址，并在后边带上`code`参数（授权码）

（2）浏览器出现向授权服务器授权页面，之后将用户同意授权。

（3）授权服务器将授权码（`AuthorizationCode`）转经浏览器发送给`client`(通过`redirect_uri`)

（4）客户端拿着授权码向授权服务器索要访问`access_token`，请求如下：

```
http://localhost:53020/uaa/oauth/token? client_id=c1&client_secret=secret&grant_type=authorization_code&code=5PgfcD&redirect_uri=http://www.baidu.com
```


（5）授权服务器返回令牌(`access_token`)
这种模式是四种模式中最安全的一种模式。一般用于`client`是`Web`服务器端应用或第三方的原生`App`调用资源服务的时候。因为在这种模式中`access_token`不会经过浏览器或移动端的`App`，而是直接从服务端去交换，这样就最大限度的减小了令牌泄漏的风险。

##### 6.2.4.2 测试

使用postman测试

```bash
http://localhost:53020/uaa/oauth/token?client_id=c1&client_secret=secret&grant_type=authorization_code&code=5PgfcD&redirect_uri=http://www.baidu.com

http://localhost:53020/uaa/token?client_id=c1&client_secret=secret&grant_type=authorization_code&code=5PgfcD&redirect_uri=http://www.baidu.com
```

![img](https://pic2.zhimg.com/80/v2-0b2134ba79475280786faea322b9c951_720w.jpg)

然后输入模拟的账号和密码点登陆之后进入授权页面：

![img](https://pic4.zhimg.com/80/v2-b25c8e837cab757206f25ad4a18cf967_720w.jpg)

确认授权后，浏览器会重定向到指定路径（oauth_client_details表中的web_server_redirect_uri）并附加验证码?code=sc0N9W（每次不一样），最后使用该验证码获取token。

```text
POST http://localhost:53020/uaa/oauth/token 
```

![img](https://pic2.zhimg.com/80/v2-402532cb64b665f59d9d8d26ab097135_720w.jpg)


#### 6.2.5 简化模式

##### 6.2.5.1 简化模式介绍

简化模式交互图：

![img](https://img-blog.csdnimg.cn/20200515161306814.png)

**（1）资源拥有者打开客户端，客户端要求资源拥有者给予授权，它将浏览器被重定向到授权服务器，重定向时会附加客户端的身份信息。如：**

```text
http://localhost:53020/uaa/oauth/authorize?client_id=c1&response_type=token&scope=all&redirect_uri=http://www.baidu.com 
```

参数描述同**授权码模式** ，注意response_type=token，说明是简化模式。

**（2）浏览器出现向授权服务器授权页面，之后将用户同意授权。**

**（3）授权服务器将授权码将令牌（access_token）以Hash的形式存放在重定向uri的fargment中发送给浏览器。**

注：fragment 主要是用来标识 URI 所标识资源里的某个资源，在 URI 的末尾通过 （#）作为 fragment 的开头，其中 # 不属于 fragment 的值。如[https://domain/index#L18](https://link.zhihu.com/?target=https%3A//domain/index%23L18)这个 URI 中 L18 就是 fragment 的值。大家只需要知道js通过响应浏览器地址栏变化的方式能获取到fragment 就行了。

一般来说，简化模式用于没有服务器端的第三方单页面应用，因为没有服务器端就无法接收授权码。

##### 6.2.5.2 测试

浏览器访问认证页面：

```text
http://localhost:53020/uaa/oauth/authorize?client_id=c1&response_type=token&scope=all&redirect_uri=http://www.baidu.com
```

![img](https://pic2.zhimg.com/80/v2-1adfca2b4f1b9cedf7eb52d8f467f6f9_720w.jpg)

然后输入模拟的账号和密码点登陆之后进入授权页面：

![img](https://pic2.zhimg.com/80/v2-a5840784c515a08153a355f6e910b819_720w.jpg)

确认授权后，浏览器会重定向到指定路径（oauth_client_details表中的web_server_redirect_uri）并以Hash的形式存放在重定向uri的fargment中,如：

```text
http://www.baidu.com/receive#access_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0ZW5hbn...
```

#### 6.2.6 密码模式

##### 6.2.6.1 密码模式介绍

下图是密码模式交互图：

![img](https://pic3.zhimg.com/80/v2-a9544cf5e68c16209050fb48fae1d88a_720w.jpg)

**（1）资源拥有者将用户名、密码发送给客户端**

**（2）客户端拿着资源拥有者的用户名、密码向授权服务器请求令牌（access_token）**，请求如下：

```text
http://localhost:53020/uaa/oauth/token?client_id=c1&client_secret=secret&grant_type=password&username=zhangsan&password=123
```

![img](https://pic1.zhimg.com/80/v2-72ad959e3e6092cbae5a2ccda13280f0_720w.jpg)

参数列表如下：

- client_id ：客户端准入标识。
- client_secret ：客户端秘钥。
- grant_type ：授权类型，填写password表示密码模式
- username ：资源拥有者用户名。
- password ：资源拥有者密码。

**（3）授权服务器将令牌（access_token）发送给client**

这种模式十分简单，但是却意味着直接将用户敏感信息泄漏给了client，因此这就说明这种模式只能用于client是我们自己开发的情况下。因此密码模式一般用于我们自己开发的，第一方原生App或第一方单页面应用。

##### 6.2.6.2 测试

```text
POST http://localhost:53020/uaa/oauth/token 
```

请求参数：

![img](https://pic1.zhimg.com/80/v2-53a68ede5415bf063ee2cecf1ac59d68_720w.jpg)

#### 6.2.7 客户端模式

##### 6.2.7.1 客户端模式介绍

![img](https://pic4.zhimg.com/80/v2-c35691525dc1a7bdd188976eb6267abf_720w.jpg)

**（1）客户端向授权服务器发送自己的身份信息，并请求令牌（access_token）**

**（2）确认客户端身份无误后，将令牌（access_token）发送给client**，请求如下：

```text
http://localhost:53020/uaa/oauth/token?client_id=c1&client_secret=secret&grant_type=client_credentials 
```

参数列表如下：

- client_id ：客户端准入标识。
- client_secret ：客户端秘钥。
- grant_type ：授权类型，填写client_credentials表示客户端模式

这种模式是最方便但最不安全的模式。因此这就要求我们对client完全的信任，而client本身也是安全的。因此这种模式一般用来提供给我们完全信任的服务器端服务。比如，合作方系统对接，拉取一组用户信息。

##### 6.2.7.2 客户端模式测试

```text
POST http://localhost:53020/uaa/oauth/token 
```

请求参数：

![img](https://pic2.zhimg.com/80/v2-3d58960e538cb95f417a837852bcf47d_720w.jpg)

#### 6.2.8 资源服务测试

##### 6.2.7.1 资源服务器配置

@EnableResourceServer 注解到一个 @Configuration 配置类上，并且必须使用 ResourceServerConfigurer 这个配置对象来进行配置（可以选择继承自 ResourceServerConfigurerAdapter 然后覆写其中的方法，参数就是这个

对象的实例），下面是一些可以配置的属性：

ResourceServerSecurityConfigurer中主要包括：

- tokenServices ：ResourceServerTokenServices 类的实例，用来实现令牌服务。
- tokenStore ：TokenStore类的实例，指定令牌如何访问，与tokenServices配置可选
- resourceId ：这个资源服务的ID，这个属性是可选的，但是推荐设置并在授权服务中进行验证。
- 其他的拓展属性例如 tokenExtractor 令牌提取器用来提取请求中的令牌。

HttpSecurity配置这个与Spring Security类似：

- 请求匹配器，用来设置需要进行保护的资源路径，默认的情况下是保护资源服务的全部路径。
- 通过 http.authorizeRequests()来设置受保护资源的访问规则
- 其他的自定义权限保护规则通过 HttpSecurity 来进行配置。

@EnableResourceServer 注解自动增加了一个类型为 OAuth2AuthenticationProcessingFilter 的过滤器链

编写ResouceServerConfig：

```java
@Configuration
@EnableResourceServer
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class ResouceServerConfig extends ResourceServerConfigurerAdapter {
    public static final String RESOURCE_ID = "res1";
   
    @Override
    public void configure(ResourceServerSecurityConfigurer resources) {
        resources.resourceId(RESOURCE_ID)
                .tokenServices(tokenService())
                .stateless(true);
    }
    @Override
    public void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .antMatchers("/**").access("#oauth2.hasScope('all')")
                .and().csrf().disable()
                 .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
    }
}
```

##### 6.2.8.2 验证token

ResourceServerTokenServices 是组成授权服务的另一半，如果你的授权服务和资源服务在同一个应用程序上的话，你可以使用 DefaultTokenServices ，这样的话，你就不用考虑关于实现所有必要的接口的一致性问题。如果你的资源服务器是分离开的，那么你就必须要确保能够有匹配授权服务提供的 ResourceServerTokenServices，它知道如何对令牌进行解码。

令牌解析方法： 使用 DefaultTokenServices 在资源服务器本地配置令牌存储、解码、解析方式 使用RemoteTokenServices 资源服务器通过 HTTP 请求来解码令牌，每次都请求授权服务器端点 /oauth/check_token

使用授权服务的 /oauth/check_token 端点你需要在授权服务将这个端点暴露出去，以便资源服务可以进行访问，这在咱们授权服务配置中已经提到了，下面是一个例子,在这个例子中，我们在授权服务中配置了/oauth/check_token 和 /oauth/token_key 这两个端点：

```java
@Override
public void configure(AuthorizationServerSecurityConfigurer security) throws Exception {
    security
.tokenKeyAccess("permitAll()")// /oauth/token_key 安全配置                
.checkTokenAccess("permitAll()") // /oauth/check_token 安全配置                
}
```

在资源 服务配置RemoteTokenServices ，在ResouceServerConfig中配置：

```java
// 资源服务令牌解析服务
@Bean
public ResourceServerTokenServices tokenService() {
    //使用远程服务请求授权服务器校验token,必须指定校验token 的url、client_id，client_secret
    RemoteTokenServices service=new RemoteTokenServices();
    service.setCheckTokenEndpointUrl("http://localhost:53020/uaa/oauth/check_token");
    service.setClientId("c1");
    service.setClientSecret("secret");
    return service;
}
```

##### 6.2.8.3编写资源

在controller包下编写OrderController，此controller表示订单资源的访问类：

```java
@RestController 
public class OrderController {
    @GetMapping(value = "/r1")
    @PreAuthorize("hasAnyAuthority('p1')")
    public String r1(){
        return "访问资源1";
    }
}
```

##### 6.2.8.4 添加安全访问控制

```java
@Configuration 
@EnableGlobalMethodSecurity(securedEnabled = true,prePostEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    //安全拦截机制（最重要）
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .authorizeRequests()
//                .antMatchers("/r/r1").hasAuthority("p2")
//                .antMatchers("/r/r2").hasAuthority("p2")
                .antMatchers("/r/**").authenticated()//所有/r/**的请求必须认证通过
                .anyRequest().permitAll()//除了/r/**，其它的请求可以访问
                ;
    }
}
```

配置引导类OrderServer

```java
@SpringBootApplication
public class OrderServer {
    public static void main(String[] args) {
        SpringApplication.run(OrderServer.class, args);
    }
}
```

##### 6.2.8.5 测试

1、申请令牌

这里我们使用密码方式

![img](https://pic4.zhimg.com/80/v2-62b40960d1df47fa2b30ae58126f6d97_720w.jpg)

2、请求资源

按照oauth2.0协议要求，请求资源需要携带token，如下：

token的参数名称为：Authorization，值为：Bearer token值

![img](https://pic1.zhimg.com/80/v2-74f19730a8e00e9990a6ae2bec0ea93c_720w.jpg)

### 6.3 JWT令牌

#### 6.3.1  JWT介绍

通过上边的测试我们发现，当资源服务和授权服务不在一起时资源服务使用RemoteTokenServices 远程请求授权服务验证token，如果访问量较大将会影响系统的性能 。

解决上边问题：

令牌采用JWT格式即可解决上边的问题，用户认证通过会得到一个JWT令牌，JWT令牌中已经包括了用户相关的信息，客户端只需要携带JWT访问资源服务，资源服务根据事先约定的算法自行完成令牌校验，无需每次都请求认证服务完成授权。

**1、什么是JWT？**

JSON Web Token（JWT）是一个开放的行业标准（RFC 7519），它定义了一种简介的、自包含的协议格式，用于在通信双方传递json对象，传递的信息经过数字签名可以被验证和信任。JWT可以使用HMAC算法或使用RSA的公钥/私钥对来签名，防止被篡改。

官网：[JWT.IO](https://link.zhihu.com/?target=https%3A//jwt.io/)

标准： [JSON Web Token (JWT)](https://link.zhihu.com/?target=https%3A//tools.ietf.org/html/rfc7519)

JWT令牌的优点：

1）jwt基于json，非常方便解析。

2）可以在令牌中自定义丰富的内容，易扩展。

3）通过非对称加密算法及数字签名技术，JWT防止篡改，安全性高。

4）资源服务使用JWT可不依赖认证服务即可完成授权。

缺点：

１）JWT令牌较长，占存储空间比较大。

**2、JWT令牌结构**

通过学习JWT令牌结构为自定义jwt令牌打好基础。

JWT令牌由三部分组成，每部分中间使用点（.）分隔，比如：xxxxx.yyyyy.zzzzz

- Header

头部包括令牌的类型（即JWT）及使用的哈希算法（如HMAC SHA256或RSA）

一个例子如下：

下边是Header部分的内容

```json
{ 
  "alg": "HS256",
  "typ": "JWT"
}
```

将上边的内容使用Base64Url编码，得到一个字符串就是JWT令牌的第一部分。

- Payload

第二部分是负载，内容也是一个json对象，它是存放有效信息的地方，它可以存放jwt提供的现成字段，比如：iss（签发者）,exp（过期时间戳）, sub（面向的用户）等，也可自定义字段。

此部分不建议存放敏感信息，因为此部分可以解码还原原始内容。

最后将第二部分负载使用Base64Url编码，得到一个字符串就是JWT令牌的第二部分。

一个例子：

```text
{ 
  "sub": "1234567890",
  "name": "456",
  "admin": true
}
```

- Signature

第三部分是签名，此部分用于防止jwt内容被篡改。

这个部分使用base64url将前两部分进行编码，编码后使用点（.）连接组成字符串，最后使用header中声明签名算法进行签名。

一个例子：

```text
HMACSHA256( 
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

base64UrlEncode(header)：jwt令牌的第一部分。

base64UrlEncode(payload)：jwt令牌的第二部分。

secret：签名所使用的密钥。

#### **6.3.2 配置JWT令牌服务**

在uaa中配置jwt令牌服务，即可实现生成jwt格式的令牌。

1、TokenConfig

```java
@Configuration 
public class TokenConfig {
    private String SIGNING_KEY = "uaa123";
    @Bean
    public TokenStore tokenStore() {
        return new JwtTokenStore(accessTokenConverter());
    }
    @Bean
    public JwtAccessTokenConverter accessTokenConverter() {
        JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
        converter.setSigningKey(SIGNING_KEY); //对称秘钥，资源服务器使用该秘钥来验证
        return converter;
    }
}
```

2、定义JWT令牌服务

```java
@Autowired
private JwtAccessTokenConverter accessTokenConverter;

@Bean
public AuthorizationServerTokenServices tokenService() {
      DefaultTokenServices service=new DefaultTokenServices();
      service.setClientDetailsService(clientDetailsService);
      service.setSupportRefreshToken(true);
      service.setTokenStore(tokenStore);

      //设置令牌增强
      TokenEnhancerChain tokenEnhancerChain = new TokenEnhancerChain();
      tokenEnhancerChain.setTokenEnhancers(Arrays.asList(accessTokenConverter));
      service.setTokenEnhancer(tokenEnhancerChain);

      service.setAccessTokenValiditySeconds(7200); // 令牌默认有效期2小时
      service.setRefreshTokenValiditySeconds(259200); // 刷新令牌默认有效期3天
      return service;
  }
```

#### **6.3.3 生成jwt令牌**

![img](https://pic4.zhimg.com/80/v2-248a9c8ef0dfdfd78d03f08f51323393_720w.jpg)

#### **6.3.4 校验jwt令牌**

资源服务需要和授权服务拥有一致的签字、令牌服务等：

1、将授权服务中的TokenConfig类拷贝到资源 服务中

2、屏蔽资源 服务原来的令牌服务类

```java
@Configuration 
@EnableResourceServer
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class ResouceServerConfig extends ResourceServerConfigurerAdapter {
    public static final String RESOURCE_ID = "res1";
    @Autowired
    TokenStore tokenStore;
    //资源服务令牌解析服务
//    @Bean
//    public ResourceServerTokenServices tokenService() {
//        //使用远程服务请求授权服务器校验token,必须指定校验token 的url、client_id，client_secret
//        RemoteTokenServices service=new RemoteTokenServices(); 
//        service.setCheckTokenEndpointUrl("http://localhost:53020/uaa/oauth/check_token");
//        service.setClientId("c1");
//        service.setClientSecret("secret");
//        return service;
//    }
    @Override
    public void configure(ResourceServerSecurityConfigurer resources) {
        resources.resourceId(RESOURCE_ID)
                .tokenStore(tokenStore)
                .stateless(true);
    }
```

3、测试

1）申请jwt令牌

2）使用令牌请求资源

![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1222' height='411'></svg>)

小技巧：

令牌申请成功可以使用/uaa/oauth/check_token校验令牌的有效性，并查询令牌的内容，例子如下：

![img](https://pic2.zhimg.com/80/v2-f9fb860925cde493bd0e2e646c5aa961_720w.jpg)

### 6.4 完善环境配置

截止目前客户端信息和授权码仍然存储在内存中，生产环境中通过会存储在数据库中，下边完善环境的配置：

#### **6.4.1 创建表**

在user_db中创建如下表：

```sql
DROP TABLE IF EXISTS `oauth_client_details`;
CREATE TABLE `oauth_client_details`  (
  `client_id` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '客户端标
识',
  `resource_ids` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL
COMMENT '接入资源列表',
  `client_secret` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL
COMMENT '客户端秘钥',
  `scope` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `authorized_grant_types` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT
NULL,
  `web_server_redirect_uri` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT
NULL,
  `authorities` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `access_token_validity` int(11) NULL DEFAULT NULL,
  `refresh_token_validity` int(11) NULL DEFAULT NULL,
  `additional_information` longtext CHARACTER SET utf8 COLLATE utf8_general_ci NULL,
  `create_time` timestamp(0) NOT NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE
CURRENT_TIMESTAMP(0),
  `archived` tinyint(4) NULL DEFAULT NULL,
  `trusted` tinyint(4) NULL DEFAULT NULL,
  `autoapprove` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`client_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '接入客户端信息'
ROW_FORMAT = Dynamic;
INSERT INTO `oauth_client_details` VALUES ('c1', 'res1',
'$2a$10$NlBC84MVb7F95EXYTXwLneXgCca6/GipyWR5NHm8K0203bSQMLpvm', 'ROLE_ADMIN,ROLE_USER,ROLE_API',
'client_credentials,password,authorization_code,implicit,refresh_token', 'http://www.baidu.com',
NULL, 7200, 259200, NULL, '2019‐09‐09 16:04:28', 0, 0, 'false');
INSERT INTO `oauth_client_details` VALUES ('c2', 'res2',
'$2a$10$NlBC84MVb7F95EXYTXwLneXgCca6/GipyWR5NHm8K0203bSQMLpvm', 'ROLE_API',
'client_credentials,password,authorization_code,implicit,refresh_token', 'http://www.baidu.com',
NULL, 31536000, 2592000, NULL, '2019‐09‐09 21:48:51', 0, 0, 'false');
```

oauth_code表，Spring Security OAuth2使用，用来存储授权码：

```sql
DROP TABLE IF EXISTS `oauth_code`;
CREATE TABLE `oauth_code`  (
  `create_time` timestamp(0) NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `code` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `authentication` blob NULL,
  INDEX `code_index`(`code`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;
```

Oracle版 sql语句

```sql
CREATE TABLE oauth_client_details  (
  client_id varchar2(255)  NOT NULL  ,
  resource_ids varchar2(255)   ,
  client_secret varchar2(255)   ,
  scope varchar2(255)  ,
  authorized_grant_types varchar2(255)  ,
  web_server_redirect_uri varchar2(255)  ,
  authorities varchar2(255)  ,
  access_token_validity INTEGER ,
  refresh_token_validity INTEGER ,
  additional_information varchar(4000)  NULL,
  create_time timestamp(0) DEFAULT sysdate,
  archived INTEGER   ,
  trusted INTEGER  ,
  autoapprove varchar2(255)
) ;

INSERT INTO oauth_client_details VALUES ('c1', 'res1','$2a$10$NlBC84MVb7F95EXYTXwLneXgCca6/GipyWR5NHm8K0203bSQMLpvm', 'ROLE_ADMIN,ROLE_USER,ROLE_API','client_credentials,password,authorization_code,implicit,refresh_token', 'http://www.baidu.com',NULL, 7200, 259200, NULL, '2019‐09‐09 16:04:28', 0, 0, 'false');
INSERT INTO oauth_client_details VALUES ('c2', 'res2','$2a$10$NlBC84MVb7F95EXYTXwLneXgCca6/GipyWR5NHm8K0203bSQMLpvm', 'ROLE_API','client_credentials,password,authorization_code,implicit,refresh_token', 'http://www.baidu.com',NULL, 31536000, 2592000, NULL, '2019‐09‐09 21:48:51', 0, 0, 'false');


CREATE TABLE oauth_code  (
 create_time timestamp(0) DEFAULT sysdate,
 code varchar2(255)     DEFAULT NULL,
 authentication blob NULL
) ;

update oauth_client_details set CLIENT_SECRET ='adca7fd922b6d65704ee7da99094788d' where client_id='c1' -- 修改密码为secret 的3次SHA加密 2次MD5加密后的 密文
```



#### 6.4.2 配置授权服务

**（1）修改AuthorizationServer：**

ClientDetailsService和AuthorizationCodeServices从数据库读取数据。

```java
@Configuration
@EnableAuthorizationServer
public class AuthorizationServer extends
AuthorizationServerConfigurerAdapter {        
@Autowired    
private TokenStore tokenStore;    
@Autowired    
private JwtAccessTokenConverter accessTokenConverter;    
@Autowired    
private ClientDetailsService clientDetailsService;    
@Autowired    
private AuthorizationCodeServices authorizationCodeServices;    
@Autowired    
private AuthenticationManager authenticationManager;    
/**    
 * 1.客户端详情相关配置    
 */    
@Bean    
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
 
@Bean    
public ClientDetailsService clientDetailsService(DataSource dataSource) {    
ClientDetailsService clientDetailsService = new JdbcClientDetailsService(dataSource);        
((JdbcClientDetailsService)
clientDetailsService).setPasswordEncoder(passwordEncoder());
       
return clientDetailsService;        
}    
@Override
public void configure(ClientDetailsServiceConfigurer clients) throws Exception {            
clients.withClientDetails(clientDetailsService);        
}
/**    
 * 2.配置令牌服务(token services)    
 */    
@Bean    
public AuthorizationServerTokenServices tokenService() {    
DefaultTokenServices service=new DefaultTokenServices();        
service.setClientDetailsService(clientDetailsService);        
service.setSupportRefreshToken(true);//支持刷新令牌        
service.setTokenStore(tokenStore); //绑定tokenStore        
TokenEnhancerChain tokenEnhancerChain = new TokenEnhancerChain();        
tokenEnhancerChain.setTokenEnhancers(Arrays.asList(accessTokenConverter));        
service.setTokenEnhancer(tokenEnhancerChain);        
service.setAccessTokenValiditySeconds(7200); // 令牌默认有效期2小时        
service.setRefreshTokenValiditySeconds(259200); // 刷新令牌默认有效期3天        
return service;        
}    
/**    
 * 3.配置令牌（token）的访问端点    
 */    
@Bean    
public AuthorizationCodeServices authorizationCodeServices(DataSource dataSource) {     
return new JdbcAuthorizationCodeServices(dataSource);//设置授权码模式的授权码如何存取        
}    
@Override    
public void configure(AuthorizationServerEndpointsConfigurer endpoints) {    
endpoints.authenticationManager(authenticationManager)        
.authorizationCodeServices(authorizationCodeServices)                
.tokenServices(tokenService())                
.allowedTokenEndpointRequestMethods(HttpMethod.POST);                
}    
/**    
 * 4.配置令牌端点(Token Endpoint)的安全约束    
 */    
@Override    
public void configure(AuthorizationServerSecurityConfigurer security){    
security        
.tokenKeyAccess("permitAll()")                
.checkTokenAccess("permitAll()")                
.allowFormAuthenticationForClients()//允许表单认证                
;        
}    
}
```

#### **6.4.3测试**

1、测试申请令牌

使用密码模式申请令牌，客户端信息需要和数据库中的信息一致。

```text
POST http://localhost:53020/uaa/oauth/token
```

![img](https://pic4.zhimg.com/80/v2-9ec796029165ef35030fcf476e71c9ef_720w.jpg)

![img](https://pic2.zhimg.com/80/v2-d24121ea30cac86ec7f8042114b04859_720w.jpg)

2、测试授权码模式

生成的授权存储到数据库中。

```text
http://localhost:53020/uaa/oauth/authorize?client_id=c1&response_type=code&scope=all&redirect_uri=http://www.baidu.com
```

**注意scope=?,查看数据库**

```text
POST http://localhost:53020/uaa/oauth/token
```

![img](https://pic1.zhimg.com/80/v2-4a291bd5199cba2edd93b3d9b8059710_720w.jpg)

![img](https://pic4.zhimg.com/80/v2-83f9f92f6d60aa41aac29b0fde7307a3_720w.jpg)

## 7 SpringSecurity 实现分布式系统授权

### 7.1 需求分析

回顾技术方案如下：

![img](https://pic4.zhimg.com/80/v2-f38fa3a122d1ae0a880f38d3390557e7_720w.jpg)

1、UAA认证服务负责认证授权。

2、所有请求经过 网关到达微服务

3、网关负责鉴权客户端以及请求转发

4、网关将token解析后传给微服务，微服务进行授权。

### 7.2 注册中心

所有微服务的请求都经过网关，网关从注册中心读取微服务的地址，将请求转发至微服务。

本节完成注册中心的搭建，注册中心采用Eureka。

1、创建maven工程

![img](https://pic1.zhimg.com/80/v2-511995356d5629c2e466028d72371f6c_720w.jpg)

2、pom.xml依赖如下

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-
4.0.0.xsd">
    <parent>
        <artifactId>distributed-security</artifactId>
        <groupId>com.lw.security</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>distributed-security-discovery</artifactId>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
    </dependencies>
</project>
```

3、配置文件

在resources中配置application.yml

```yaml
spring: 
    application:
        name: distributed-discovery
server:
    port: 53000 #启动端口
eureka:
  server:
    enable-self-preservation: false    #关闭服务器自我保护，客户端心跳检测15分钟内错误达到80%服务会保护，导致别人还认为是好用的服务
    eviction-interval-timer-in-ms: 10000 #清理间隔（单位毫秒，默认是60*1000）5秒将客户端剔除的服务在服务注册列表中剔除# 
    shouldUseReadOnlyResponseCache: true #eureka是CAP理论种基于AP策略，为了保证强一致性关闭此切换CP默认不关闭 false关闭
  client: 
    register-with-eureka: false  #false:不作为一个客户端注册到注册中心
    fetch-registry: false      #为true时，可以启动，但报异常：Cannot execute request on any known server
    instance-info-replication-interval-seconds: 10 
    serviceUrl: 
      defaultZone: http://localhost:${server.port}/eureka/
  instance:
    hostname: ${spring.cloud.client.ip-address}
    prefer-ip-address: true
    instance-id: ${spring.application.name}:${spring.cloud.client.ip-address}:${spring.application.instance_id:${server.port}}
```

启动类：

```java
@SpringBootApplication
@EnableEurekaServer
public class DiscoveryServer {
   public static void main(String[] args) {
      SpringApplication.run(DiscoveryServer.class, args);
  }
}
```

### 7.3 网关

网关整合 OAuth2.0 有两种思路，一种是认证服务器生成jwt令牌, 所有请求统一在网关层验证，判断权限等操作；另一种是由各资源服务处理，网关只做请求转发。

我们选用第一种。我们把API网关作为OAuth2.0的资源服务器角色，实现接入客户端权限拦截、令牌解析并转发当前登录用户信息(jsonToken)给微服务，这样下游微服务就不需要关心令牌格式解析以及OAuth2.0相关机制了。

API网关在认证授权体系里主要负责两件事：

（1）作为OAuth2.0的资源服务器角色，实现接入方权限拦截。

（2）令牌解析并转发当前登录用户信息（明文token）给微服务

微服务拿到明文token(明文token中包含登录用户的身份和权限信息)后也需要做两件事：

（1）用户授权拦截（看当前用户是否有权访问该资源）

（2）将用户信息存储进当前线程上下文（有利于后续业务逻辑随时获取当前用户信息）

#### **7.3.1 创建工程**

![img](https://pic4.zhimg.com/80/v2-e97a2ee7641d58804f60a381460a8efb_720w.jpg)

1、pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-
4.0.0.xsd">
    <parent>
        <artifactId>distributed-security</artifactId>
        <groupId>com.lw.security</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>distributed-security-gateway</artifactId>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId> 
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>com.netflix.hystrix</groupId>
            <artifactId>hystrix-javanica</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.retry</groupId>
            <artifactId>spring-retry</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-oauth2</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-jwt</artifactId>
        </dependency>
        <dependency>
            <groupId>javax.interceptor</groupId>
            <artifactId>javax.interceptor-api</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
</project>
```

2、配置文件

[配置application.properties](https://link.zhihu.com/?target=http%3A//xn--application-2k23aj16m.properties/)

```properties
spring.application.name=gateway-server
server.port=53010
spring.main.allow-bean-definition-overriding = true
logging.level.root = info
logging.level.org.springframework = info
zuul.retryable = true
zuul.ignoredServices = *
zuul.add-host-header = true
zuul.sensitiveHeaders = *
zuul.routes.uaa-service.stripPrefix = false
zuul.routes.uaa-service.path = /uaa/**
zuul.routes.order-service.stripPrefix = false
zuul.routes.order-service.path = /order/**
eureka.client.serviceUrl.defaultZone = http://localhost:53000/eureka/
eureka.instance.preferIpAddress = true
eureka.instance.instance-id = ${spring.application.name}:${spring.cloud.client.ip-address}:${spring.application.instance_id:${server.port}}
management.endpoints.web.exposure.include = refresh,health,info,env
feign.hystrix.enabled = true
feign.compression.request.enabled = true
feign.compression.request.mime-types[0] = text/xml
feign.compression.request.mime-types[1] = application/xml
feign.compression.request.mime-types[2] = application/json
feign.compression.request.min-request-size = 2048
feign.compression.response.enabled = true
```

统一认证服务（UAA）与统一用户服务都是网关下微服务，需要在网关上新增路由配置：

```properties
zuul.routes.uaa-service.stripPrefix = false
zuul.routes.uaa-service.path = /uaa/**

zuul.routes.user-service.stripPrefix = false
zuul.routes.user-service.path = /order/**
```

上面配置了网关接收的请求url若符合/order/**表达式，将被被转发至order-service(统一用户服务)。

启动类：

```java
@SpringBootApplication 
@EnableZuulProxy
@EnableDiscoveryClient
public class GatewayServer {
    public static void main(String[] args) {
        SpringApplication.run(GatewayServer.class, args);
    }
}
```

#### 7.3.2 token配置

前面也介绍了，**资源服务器**由于需要验证并解析令牌，往往可以通过在授权服务器暴露check_token的Endpoint来完成，而我们在授权服务器使用的是对称加密的jwt，因此知道密钥即可，资源服务与授权服务本就是对称设计，那我们把授权服务的TokenConfig两个类拷贝过来就行 。

```java
@Configuration
public class TokenConfig {
  private String SIGNING_KEY = "uaa123";
    @Bean
    public TokenStore tokenStore() {
        return new JwtTokenStore(accessTokenConverter()); 
    }
   @Bean
    public JwtAccessTokenConverter accessTokenConverter() {
        JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
        converter.setSigningKey(SIGNING_KEY); //对称秘钥，资源服务器使用该秘钥来解密
        return converter;
    }
}
```

#### 7.3.3 配置资源服务

在ResouceServerConfig中定义资源服务配置，主要配置的内容就是定义一些匹配规则，描述某个接入客户端需要什么样的权限才能访问某个微服务，如：

```java
@Configuration
public class ResouceServerConfig {
    public static final String RESOURCE_ID = "res1";
    /**
     * 统一认证服务(UAA) 资源拦截
     */
    @Configuration
    @EnableResourceServer
    public class UAAServerConfig extends
            ResourceServerConfigurerAdapter {
        @Autowired
        private TokenStore tokenStore;
        @Override
        public void configure(ResourceServerSecurityConfigurer resources){
            resources.tokenStore(tokenStore).resourceId(RESOURCE_ID)
                    .stateless(true);
        }
        @Override
        public void configure(HttpSecurity http) throws Exception {
            http.authorizeRequests()
                    .antMatchers("/uaa/**").permitAll();
        }
    }
    
    /**
     *  订单服务
     */
    @Configuration
    @EnableResourceServer
    public class OrderServerConfig extends
        ResourceServerConfigurerAdapter {
            @Autowired
            private TokenStore tokenStore;
        @Override
        public void configure(ResourceServerSecurityConfigurer resources) {
            resources.tokenStore(tokenStore).resourceId(RESOURCE_ID)
                    .stateless(true);
        }
        @Override
        public void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .antMatchers("/order/**").access("#oauth2.hasScope('ROLE_API')");
        }
    }
}
```

上面定义了两个微服务的资源，其中：

UAAServerConfig指定了若请求匹配/uaa/**网关不进行拦截。

OrderServerConfig指定了若请求匹配/order/**，也就是访问统一用户服务，接入客户端需要有scope中包含read，并且authorities(权限)中需要包含ROLE_USER。

由于res1这个接入客户端，read包括ROLE_ADMIN,ROLE_USER,ROLE_API三个权限。

**7.3.4 安全配置**

```java
@Configuration 
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .antMatchers("/**").permitAll()
                .and().csrf().disable();
    }
}
```

### 7.3.转发明文token给微服务

通过Zuul过滤器的方式实现，目的是让下游微服务能够很方便的获取到当前的登录用户信息（明文token）

**（ 1）实现Zuul前置过滤器，完成当前登录用户信息提取，并放入转发微服务的request中**

```java
 /**
 * token传递拦截
 */
public class AuthFilter extends ZuulFilter {
    @Override
    public boolean shouldFilter() {
        return true;
    }
    @Override
    public String filterType() {
        return "pre";
    }
    @Override
    public int filterOrder() {
        return 0;
    }
    @Override
    public Object run() {
        /**
         * 1.获取令牌内容
         */
        RequestContext ctx = RequestContext.getCurrentContext();
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        if(!(authentication instanceof OAuth2Authentication)){ // 无token访问网关内资源的情况，目前仅有uua服务直接暴露
            return null;
        }
        OAuth2Authentication oauth2Authentication  = (OAuth2Authentication)authentication;
        Authentication userAuthentication = oauth2Authentication.getUserAuthentication();
        Object principal = userAuthentication.getPrincipal();
        /**
         * 2.组装明文token，转发给微服务，放入header，名称为json-token
         */
        List<String> authorities = new ArrayList();
        userAuthentication.getAuthorities().stream().forEach(s ->authorities.add(((GrantedAuthority) s).getAuthority()));
        OAuth2Request oAuth2Request = oauth2Authentication.getOAuth2Request();
        Map<String, String> requestParameters = oAuth2Request.getRequestParameters();
        Map<String,Object> jsonToken = new HashMap<>(requestParameters);
        if(userAuthentication != null){
            jsonToken.put("principal",userAuthentication.getName());
            jsonToken.put("authorities",authorities);
        }
        ctx.addZuulRequestHeader("json-token", EncryptUtil.encodeUTF8StringBase64(JSON.toJSONString(jsonToken)));
        return null;
   }
}
```

common包下建EncryptUtil类 UTF8互转Base64

```java
public class EncryptUtil {
    private static final Logger logger = LoggerFactory.getLogger(EncryptUtil.class);

    public static String encodeBase64(byte[] bytes){
        String encoded = Base64.getEncoder().encodeToString(bytes);
        return encoded;
    }

    public static byte[]  decodeBase64(String str){
        byte[] bytes = null;
        bytes = Base64.getDecoder().decode(str);
        return bytes;
    }

    public static String encodeUTF8StringBase64(String str){
        String encoded = null;
        try {
            encoded = Base64.getEncoder().encodeToString(str.getBytes("utf-8"));
        } catch (UnsupportedEncodingException e) {
            logger.warn("不支持的编码格式",e);
        }
        return encoded;

    }

    public static String  decodeUTF8StringBase64(String str){
        String decoded = null;
        byte[] bytes = Base64.getDecoder().decode(str);
        try {
            decoded = new String(bytes,"utf-8");
        }catch(UnsupportedEncodingException e){
            logger.warn("不支持的编码格式",e);
        }
        return decoded;
    }

    public static String encodeURL(String url) {
    	String encoded = null;
		try {
			encoded =  URLEncoder.encode(url, "utf-8");
		} catch (UnsupportedEncodingException e) {
			logger.warn("URLEncode失败", e);
		}
		return encoded;
	}


	public static String decodeURL(String url) {
    	String decoded = null;
		try {
			decoded = URLDecoder.decode(url, "utf-8");
		} catch (UnsupportedEncodingException e) {
			logger.warn("URLDecode失败", e);
		}
		return decoded;
	}

    public static void main(String [] args){
        String str = "abcd{'a':'b'}";
        String encoded = EncryptUtil.encodeUTF8StringBase64(str);
        String decoded = EncryptUtil.decodeUTF8StringBase64(encoded);
        System.out.println(str);
        System.out.println(encoded);
        System.out.println(decoded);

        String url = "== wo";
        String urlEncoded = EncryptUtil.encodeURL(url);
        String urlDecoded = EncryptUtil.decodeURL(urlEncoded);
        
        System.out.println(url);
        System.out.println(urlEncoded);
        System.out.println(urlDecoded);
    }
}
```

**（ 2）将filter纳入spring 容器：**

配置AuthFilter

```java
@Configuration
public class ZuulConfig {
    @Bean
    public AuthFilter preFileter() {
        return new AuthFilter();
    }
    @Bean
    public FilterRegistrationBean corsFilter() {
        final UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        final CorsConfiguration config = new CorsConfiguration();
        config.setAllowCredentials(true);
        config.addAllowedOrigin("*");
        config.addAllowedHeader("*");
        config.addAllowedMethod("*");
        config.setMaxAge(18000L);
        source.registerCorsConfiguration("/**", config);
        CorsFilter corsFilter = new CorsFilter(source);
        FilterRegistrationBean bean = new FilterRegistrationBean(corsFilter);
        bean.setOrder(Ordered.HIGHEST_PRECEDENCE);
        return bean;
    }
}
```

### 7.4.微服务用户鉴权拦截

当微服务收到明文token时，应该怎么鉴权拦截呢？自己实现一个filter？自己解析明文token，自己定义一套资源访问策略？能不能适配Spring Security呢，是不是突然想起了前面我们实现的Spring Security基于token认证例子。咱们还拿统一用户服务作为网关下游微服务，对它进行改造，增加**微服务用户鉴权拦截**功能。

**（1）增加测试资源**

OrderController增加以下endpoint

```java
@PreAuthorize("hasAuthority('p1')")
    @GetMapping(value = "/r1")
    public String r1(){
        UserDTO user = (UserDTO)
SecurityContextHolder.getContext().getAuthentication().getPrincipal();
         return user.getUsername() + "访问资源1";
    }
    @PreAuthorize("hasAuthority('p2')")
    @GetMapping(value = "/r2")
    public String r2(){//通过Spring Security API获取当前登录用户
        UserDTO user =
(UserDTO)SecurityContextHolder.getContext().getAuthentication().getPrincipal();
        return user.getUsername() + "访问资源2";
    }
```

model包下加实体类UserDto

```java
@Data
public class UserDTO {
    private String id;
    private String username;
    private String mobile;
    private String fullname;

}
```

**（2）Spring Security配置**

开启方法保护，并增加Spring配置策略，除了/login方法不受保护(统一认证要调用)，其他资源全部需要认证才能访问。

```java
@Override
    public void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .antMatchers("/**").access("#oauth2.hasScope('ROLE_ADMIN')")
                .and().csrf().disable()
                 .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
    }
```

综合上面的配置，咱们共定义了三个资源了，拥有p1权限可以访问r1资源，拥有p2权限可以访问r2资源，只要认证通过就能访问r3资源。

**（3）定义filter拦截token，并形成Spring Security的Authentication对象**

```java
@Component
public class TokenAuthenticationFilter extends OncePerRequestFilter {
    @Override
    protected void doFilterInternal(HttpServletRequest httpServletRequest, HttpServletResponse
httpServletResponse, FilterChain filterChain) throws ServletException, IOException {
               String token = httpServletRequest.getHeader("json-token");
        if (token != null){
            //1.解析token
            String json = EncryptUtil.decodeUTF8StringBase64(token);
            JSONObject userJson = JSON.parseObject(json);
            UserDTO user = new UserDTO();
            user.setUsername(userJson.getString("principal"));
            JSONArray authoritiesArray = userJson.getJSONArray("authorities");
            String  [] authorities = authoritiesArray.toArray( new
String[authoritiesArray.size()]);
            //2.新建并填充authentication
            UsernamePasswordAuthenticationToken authentication = new
UsernamePasswordAuthenticationToken(
                    user, null, AuthorityUtils.createAuthorityList(authorities));
            authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(
                    httpServletRequest));
            //3.将authentication保存进安全上下文
            SecurityContextHolder.getContext().setAuthentication(authentication);
        }
        filterChain.doFilter(httpServletRequest, httpServletResponse);
    }
}
```

经过上边的过虑 器，资源 服务中就可以方便到的获取用户的身份信息：

```java
UserDTO user = (UserDTO) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
```

还是三个步骤：

1.解析token

2.新建并填充authentication

3.将authentication保存进安全上下文

剩下的事儿就交给Spring Security好了。

### 7.5.集成测试

**注意:记得uaa跟order的pom导入eurika坐标,以及application.properties配置eurika**

本案例测试过程描述：

1、采用OAuth2.0的密码模式从UAA获取token

2、使用该token通过网关访问订单服务的测试资源

（1）**过网关**访问uaa的授权及获取令牌，获取token。注意端口是53010，网关的端口。

如授权 endpoint：

```java
http://localhost:53010/uaa/oauth/authorize?response_type=code&client_id=c1 
```

令牌endpoint

```text
http://localhost:53010/uaa/oauth/token
```

（2）使用Token过网关访问订单服务中的r1-r2测试资源进行测试。

结果：

使用张三token访问p1，访问成功

使用张三token访问p2，访问失败

使用李四token访问p1，访问失败

使用李四token访问p2，访问成功

符合预期结果。

（3）破坏token测试

无token测试返回内容：

```text
{ 
    "error": "unauthorized",
    "error_description": "Full authentication is required to access this resource"
}
```

破坏token测试返回内容：

```text
{ 
    "error": "invalid_token",
    "error_description": "Cannot convert access token to JSON"
}
```

### 7.6 扩展用户信息

**7.6.1 需求分析**

目前jwt令牌存储了用户的身份信息、权限信息，网关将token明文化转发给微服务使用，目前用户身份信息仅包括了用户的账号，微服务还需要用户的ID、手机号等重要信息。

所以，本案例将提供扩展用户信息的思路和方法，满足微服务使用用户信息的需求。

下边分析JWT令牌中扩展用户信息的方案：

在认证阶段DaoAuthenticationProvider会调用UserDetailService查询用户的信息，这里是可以获取到齐全的用户信息的。由于JWT令牌中用户身份信息来源于UserDetails，UserDetails中仅定义了username为用户的身份信息，这里有两个思路：第一是可以扩展UserDetails，使之包括更多的自定义属性，第二也可以扩展username的内容，比如存入json数据内容作为username的内容。相比较而言，方案二比较简单还不用破坏UserDetails的结构，我们采用方案二。

**7.6.2 修改UserDetailService**

从数据库查询到user，将整体user转成json存入userDetails对象。

```java
@Override 
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
    //登录账号
    System.out.println("username="+username);
    //根据账号去数据库查询...
    UserDto user = userDao.getUserByUsername(username);
    if(user == null){
        return null;
    }
    //查询用户权限
    List<String> permissions = userDao.findPermissionsByUserId(user.getId());
    String[] perarray = new String[permissions.size()];
    permissions.toArray(perarray);
    //创建userDetails
    //这里将user转为json，将整体user存入userDetails
    String principal = JSON.toJSONString(user);
    UserDetails userDetails =
User.withUsername(principal).password(user.getPassword()).authorities(perarray).build();
    return userDetails;
}
```

**7.6.3 修改资源服务过虑器**

资源服务中的过虑 器负责 从header中解析json-token，从中即可拿网关放入的用户身份信息，部分关键代码如下：

```text
... 
if (token != null){
    //1.解析token
    String json = EncryptUtil.decodeUTF8StringBase64(token);
    JSONObject userJson = JSON.parseObject(json);
    //取出用户身份信息
    String principal = userJson.getString("principal");
    //将json转成对象
    UserDTO userDTO = JSON.parseObject(principal, UserDTO.class);
    JSONArray authoritiesArray = userJson.getJSONArray("authorities");
    ...
```

以上过程就完成自定义用户身份信息的方案。

## 8. 课程总结

重点回顾：

什么是认证、授权、会话。

Java Servlet为支持http会话做了哪些事儿。

基于session认证机制的运作流程。

基于token认证机制的运作流程。

理解Spring Security的工作原理，Spring Security结构总览，认证流程和授权，中间涉及到哪些组件，这些组件分

别处理什么，如何自定义这些组件满足个性需求。

OAuth2.0认证的四种模式？它们的大体流程是什么？

Spring cloud Security OAuth2包括哪些组件？职责？

分布式系统认证需要解决的问题？

掌握学习方法，掌握思考方式。