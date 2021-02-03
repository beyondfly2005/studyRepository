 # SpringBoot核心技术教程

> (尚硅谷-2018-idea版)
>
> 基础篇 课程地址：https://www.bilibili.com/video/BV1gW411W76m
>
> 高级篇 课程地址：https://www.bilibili.com/video/BV1KW411F7oX

### 课程内容

#### 基础篇

一、SpringBoot入门

二、SpringBoot配置

三、SpringBoot与日志

四、SpringBoot与Web开发

五、SpringBoot与Docker

六、SpringBoot与数据访问

七、SpringBoot、启动与数据访问

八、SpringBoot自定义Starters

#### 高级篇

九、SpringBoot与缓存

十、SpringBoot与消息

十一、SpringBoot与检索

十二、SpringBoot与任务

十三、SpringBoot与安全安全

十四、SpringBoot与分布式

十五、SpringBoot与开发热部署

十六、SpringBoot与监控管理



# SpringBoot 基础篇

## 一、SpringBoot入门

#### 1、SpringBoot简介

##### **概念**

SpringBoot来简化Spring开发，约定大于配置，去繁从简，just run就能创建一个独立的，产品级别的应用

##### **背景**

J2EE笨重的而开发，繁多的配置，低下的开发效率，复杂的部署流程、第三方技术集成难度大

**解决**

Spring全家桶时代 SpringBoot提供了一站式解决方案 SprigCloud分布式解决方案。

##### **优点**

快速场景独立运行的Spring项目以及与主流框架集成

使用嵌入式的eServlet容器，应用无需打成war包

starter自动依赖于版本控制

大量的自动配置，简化开发，也可以修改默认值

无需大量配置xml 无代码生成 开箱即用

准生成环境的运行时监控

与云计算的天然集成

##### **缺点**

入门容易 精通难

排查错误较难

#### 2、微服务简介

##### 微服务架构概念：

2014 Martin Flowler 微服务是一种架构风格，它要求我们在开发一个应用的时候，这个应用必须构建成一组（一系列的）小型服务的集合，可以通过http方式进行交互。

> Martin Flower——微服务（Microservices）
>
> 原文地址：https://martinfowler.com/articles/microservices.html
>
> 翻译地址：https://www.cnblogs.com/liuning8023/p/4493156.html

##### 单体架构：

单体应用架构（All In One）：将一个应用的中的所有应用服务都封装在一个应用中。

无论是ERP、CRM或是其他什么系统，你都把数据库访问，web访问，等等各个功能放到一个war包内

##### 单体架构优点：

开发develop 测试test 简单；

部署简单deploy；

水平扩展简单scale；需要扩展时 只需要将war包复制多份，然后部署到多个服务器上，做个负载均衡Nginx就可以了。（水平复制，进行扩展）

##### 单体架构的缺点：

单体应用架构的缺点是，哪怕我要修改一个非常小的地方，我都需要停掉整个服务，重新打包、部署这个应用war包。特别是对于一个大型应用，我们不可能吧所有内容都放在一个应用里面，我们如何维护、如何分工合作都是问题。

##### 微服务架构特点：

微服务的每一个功能都是一个可独立替换的和独立升级的软件单元。

服务要有多小，如何规划拆分粒度（服务微化）

微服务架构是把每个功能元素独立出来。把独立出来的功能元素的动态组合，需要的功能元素才去拿来组合，需要多一些时可以整合多个功能元素。所以微服务架构是对功能元素进行复制，而没有对整个应用进行复制。

##### 微服务架构的优点：

- 节省了调用资源
- 每个功能原始都是一个可替换、可以独立升级的软件代码

![img](https://img2020.cnblogs.com/blog/1927057/202004/1927057-20200412092656638-1282156445.png)

##### 如何构建微服务：

一个大型系统的微服务架构，就像一个复杂交织的神经网络，每一个神经元就是一个功能元素，它们各自完成自己的功能，然后通过http相互请求调用。比如一个电商系统，查缓存、连数据库、浏览页面、结账、支付等服务都是一个个独立的功能服务，都被微化了，它们作为一个个微服务共同构建了一个庞大的系统。如果修改其中的一个功能，只需要更新升级其中一个功能服务单元即可。 但是这种庞大的系统架构给部署和运维带来很大的难度。于是，spring为我们带来了构建大型分布式微服务的全套、全程产品：

- 构建一个个功能独立的微服务应用单元，可以使用springboot，可以帮我们快速构建一个应用； 
- 大型分布式网络服务的调用，这部分由spring cloud来完成，实现分布式； 
- 在分布式中间，进行流式数据计算、批处理，我们有spring cloud data flow。 spring为我们想清楚了整个从开始构建应用到大型分布式应用全流程方案。

![img](https://img2020.cnblogs.com/blog/1927057/202004/1927057-20200412092808663-1120267243.png)

#### 3、入门环境准备

##### 前置知识：

- Spring框架的使用经验

- 熟练使用Maven进行项目构建和依赖管理
- 熟练使用Eclipse或者IDEA

##### 环境约束：

- jdk 1.8
- maven 3.x
- IntellJ IDEA 2017
- SpringBoot 1.5.9.RELEASE  推荐使用2.0以上

**Maven配置**

给maven的settings.xml配置文件的profile标签添加

```xml
<profile>
    <id>jdk-1.8</id>
    <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
        <properties>
        	<maven.compiler.source>1.8</maven.compiler.source>
           	<maven.compiler.targert>1.8</maven.compiler.target>
           	<maven.compiler.comilerVersion>1.8</maven.compiler.comilerVersion>
        </properties>
    </activation>
</profile>
```

##### Idea中Maven的设置:

Settings->Build,Excecution,Deployment ->Maven

设置Maven的主目录 、配置文件、本地Maven仓库

#### 4、springboot hello-world

###### 4.1 创建一个Maven项目

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>
```

###### 4.2 导入SpringBoot依赖 starters

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

###### 4.3 创建一个主程序 启动springboot应用

```java
/**
 * @SpringBootApplication 标注一个主程序类，说明这是一个Spring Boot应用
 */
@SpringBootApplication
public class HelloWorldApplication {
	public static void main(String[] args) {
        // 运行Spring应用
		SpringApplication.run(HelloWorldApplication.class, args);
	}
}
```

###### 4.4 编写业务逻辑 service controller

```java
package com.ac.demo.controller;

@Controller
public class HelloController{
    
    @RequestMapping("/hello")
	public String hello(){
        return "Hello World";
    }    
}
```

###### 4.5 运行主程序测试

​	运行主程序中的main方法

​	浏览器访问 http://localhost:8080/hello

###### 4.6 简化部署 jar包方式运行

导入maven插件 将应用打包成一个可执行的jar包

```xml
<build>
    <plugins>
    	<plugin>  
            <groupId>org.springframework.boot</groupId>  
            <artifactId>spring-boot-maven-plugin</artifactId>             
        </plugin>  
    </plugins>
</build>
```
使用java -jar 运行 ，不需要安装tomcat ，jar中自带了libs 嵌入式的tomcat
```bash
java -jar helloworld-1.0-SNAPSHOT.jar
```

#### 5、starter 场景启动器

1、父项目

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
   </parent>     

  <!-- 它的父项目是 -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.4.RELEASE</version>
  </parent>
  <!--
  他是真正管理SpringBoot应用里面的所有依赖版本
  通过<properties></properties>定义了常用的依赖的jar包版本
  -->
```

spring-boot-dependencies 是SpringBoot的版本仲裁中心

以后我们需要导入依赖默认是不需要版本的（没有在dependencies中管理的，才需要声明版本）

2、导入的依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**spring-boot-starter-**web

spring-boot-starter：sping-boot 场景启动器；帮助我们导入web模块正常运行所依赖的组件

其他的场景启动器：

spring-boot-starter-aop

spring-boot-starter-mail

spring-boot-starter-test

SpringBoot将所有的功能场景都抽取处理，做成一个个starters（启动器），只需要在项目里面引入这些satarer相关场景的所有依赖都会导入进来，你需要什么功能就导入什么场景启动器

#### 6、HelloWorld细节：自动配置

SpringBoot 将所有的功能场景都抽取处理，做成一个个的starter启动器，只需要在项目里面引入哲学starter相关场景的所有依赖都会导入进来，需要什么功能就导入什么场景启动器

##### 6.1 主程序类 主入口类

```java
@SpringBootApplication
public class HelloWorldApplication {
	public static void main(String[] args) {
        // 运行Spring应用
		SpringApplication.run(HelloWorldApplication.class, args);
	}
}
```

**@SpringBootApplication：** Spring Boot应用，标注在某个类上 是SpringBoot的主配置类，SpringBoot就会00运行这个类的main方法来启动SpringBoot应用；是一个组合注解，如下：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(type = FilterType.CUSTOM, classes = {TypeExcludeFilter.class}), 
    				  @Filter(type = FilterType.CUSTOM, classes = {AutoConfigurationExcludeFilter.class})}
)
public @interface SpringBootApplication {
}
```

**@SpringBootConfiguration：**Spring Boot配置类 ；标注在某个类上，表示这是一个SpringBoot的配置类，相当于spring时的配置文件

**@Configuration** 在配置类上来标注这个注解，表明这是一个配置类 ，也就是配置文件的java实现方式，配置类也是容器中的一个组件

**@EnableAutoConfiguration：** 开启自动配置功能；以前需要自己配置的地方，SpringBoot 帮助我们自动配置；这个注解告诉SPringBoot开启自动配置功能；这样自动配置才会生效。

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
}
```

**@AutoConfigurationPackage** 自动配置包 是用**@Import** 完成的功能，它的功能是：将主配置类(@SpringBootApplication标注的类)所在的包及所有子包的所有组件(类/方法)扫描到Spring容器中；

**@Import** 给容器中导入一个组件，导入的组件由AutoConfigurationPackages.Registrar.class来指定

​	AutoConfigurationImportSelector.class 导入哪些组件的选择器

​	将所有需要导入的组件以全类名的方式返回为一个数组；这些组件就会被添加到容器中；

​	会给容器中导入很多的自动配置类(xxxAutoConfiguration)；就是给容器中导入这个场景所需要的的所有组件，并配置好这些组件

​	有了这些自动配置就免去了我们手动编写配置和注入功能组件等工作；

​	SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.calss,ClassLoader);

**SpringBoot在启动的时候，从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值；将这些值作为自动配置导入到容器中，自动配置类就会生效，帮我们进行自动配置工作；**以前我们需要自己配置的东西，自动配置类都帮我们做了

J2EE的整体解决方案和自动配置都在 Spring-Boot-AutoConfigure-x.x.x.jar中

​	

*****更详细教程参考：Spring注解版（谷粒学院）



#### 7、使用向导快速创建SpringBoot应用

##### 7.1 创建项目

IDE都支持使用Spring的项目创建向导快速粗昂见一个SpringBoot应用，以Idea为例

- new Project -> Spring Initializr

- 选择JDK版本

- 输入项目信息

  Group Artifact Java版本 工程名称 包名 打包方式

- 选择依赖的模块

  如 Web、Security、RabbitMQ等

- 选型项目名称及存储路径 

- IDE会联网创建SpringBoot项目 访问spring.io 自动下载项目文件

##### 7.2 测试创建的项目

​	编写Controller 测试

​	如果整个Controller类的所有方法都是Json形式返回数据 那么可以将@ResponseBody标注到类上

​	@RestController = @ResponseBody+@Controller

##### 7.3 默认生成的SpringBoot项目

- 主程序已经生成好了，我们只需创建我们自己的逻辑。
- resources文件夹中的目录结构
  - static 存放静态资源 js css img
  - templates 存放模板页面（SpringBoot默认打包方式是Jar包 使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（Freemarker thymeleaf等）
  - application.properties：SpringBoot应用程序的配置文件，可以修改一些默认配置 如端口号

##### 7.4 Spring Tool suite

  使用Spring Tool suite（STS）/Eclipse提供的创建向导创建SpringBoot项目 



## 二、Spring Boo 配置

配置文件、加载顺序、配置原理

### P09 配置-yaml简介

#### 1、配置文件

##### 1.1 Spring Boot 使用一个全局的配置文件

- application.properties

- application.yml

##### 1.2 配置文件存放在src/main/resources目录或者类路径/config下

##### 1.3 yml是YAML(YAML Ain't Markup Language) 语言文件，以数据为中心，比json xml等更适合做配置文件

​	Ain't  ：YAML A  Markup Language; YAML isn't  Markup Language

​	yml参考语法规范 http://www.yaml.org

##### 1.4 全局配置文件作用

​	修改SpringBoot自动配置的默认值，可以对一些默认配置值进行修改；SpringBoot在底层都给我们自动配置好。

##### 1.5  yml与XML配置对比

YAML配置的例子

```yaml
server:
  port: 8080
```

XML配置

```xml
<server>
    <port>8080</port>
</server>
```

#### 2、配置文件yaml简介

##### 2.1 YML是什么

YAML (YAML Ain't a Markup Language)YAML不是一种标记语言，通常以.yml为后缀的文件，是一种直观的能够被电脑识别的数据序列化格式，并且容易被人类阅读，容易和脚本语言交互的，可以被支持YAML库的不同的编程语言程序导入，一种专门用来写配置文件的语言。可用于如： Java，C/C++, Ruby, Python, Perl, C#, PHP等。

##### 2.2 YML的优点

1. YAML易于人们阅读。
2. YAML数据在编程语言之间是可移植的。
3. YAML匹配敏捷语言的本机数据结构。
4. YAML具有一致的模型来支持通用工具。
5. YAML支持单程处理。
6. YAML具有表现力和可扩展性。
7. YAML易于实现和使用。

### P10 配置-yaml语法

#### 3、配置文件yaml语法

##### 3.1 基本语法

```
key: value 
```

 key: 空格value 表示一对键值对（必须有空格）；

以空格缩进考控制曾经关系；只要是左对齐的一列数据，都是同一层级，空格数不限，但是同一层级 空格必须一致

属性和值 都是大小写敏感的

示例：

```yaml
server:
  port: 8080
  path: /hello
```

##### 3.2 值的写法

**字面量：普通的值（数字、字符串、布尔）**

```
k: v  
```

字面量直接来写，注意v之前有空格

自浮夸默认不用加速单引号或双引号；

"" 双引号不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思输出

​		name: "zhagnsan \n lisi"  输出时 会换行 \n 会被表示为换行； zhangsan 换行 lisi

'' 单引号 会转义特殊字符，特殊字符最终知识一个普通的字符串数据

​		name: 'zhagnsan \n lisi'  输出时不会被换行原样输出 zhagnsan \n lisi  

**对象、Map（属性和值）（键值对）**

```
k: v
```

对象还是k: v的方式；在下一行写对象的属性和值的关系；注意缩进

```yaml
friends:
  lastName: zhagnsan
  age: 20
```

行内写法

```yaml
friends: {lastName: zhagnsan,age: 20}
```

**数组（List、Set）**

用 - 值表示数组中的一个元素

```yaml
pets:
  - cat
  - dog
  - pig
```

行内写法

```yaml
pets: [cat,dog,pig]
```



### P11 配置-yaml配置文件值获取

#### 4、配置文件yaml配置文件值的获取

##### 配置文件数据绑定类

```java
@Data
public class Person{
	private Stirng lastName;
	private Integer age;
	private Boolean boss;
	private Date birth;
	private Map<String,Object> maps;
	private List<Object> lists;
	private Dog dog;
}

@Data
public class Dog{
	private String name;
	private Integer age;
}
```

##### 配置文件

```yaml
person:
  lastName: zhangsan
  age: 20
  boss: false
  birth: 2001/02/12
  maps: {k1: val1,k2: val2}
  lists:
    - lisi
    - wangwu
  dog:
  	name: mydog
  	age: 2
```

##### 绑定配置文件数据

将配置文件中的配置的每个属性的值，映射到这类中

@ConfigurationProperties 告诉Springboot将本类中的属性和配置文件中的相关的配置进行绑定

prefix="person" 配置文件中哪个元素下面的所有属性进行一一映射

只有这个组件/类 是容器中的组件，才能使用容器中的功能：如快速动态绑定 ；使用@Component 加入到容器

```java
@Data
@Component
@ConfigurationProperties(prefix="person")
public class Person{
	private Stirng lastName;
	private Integer age;
	private Boolean boss;
	private Date birth;
	private Map<String,Object> maps;
	private List<Object> lists;
	private Dog dog;
}

@Data
public class Dog{
	private String name;
	private Integer age;
}
```

##### 配置文件处理器

编写配置文件时，默认没有代码提示，需要引入一个依赖 就可以进行代码提示

```xml
<!-- 配置文件处理器，配置文件进行绑定时 就会有提示-->
<dependency>
    <groupId>org.springframework,.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

##### 单元测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringBootTest{
    
    @Autowired
    Person person;
    
    @Test
    public void test{
        System.out.println(person)
    }    
}
```

### P12 配置-properties配置文件编码问题

#### 5、配置文件properties编码问题

##### properties文件方式绑定到类

```properties
person.last-name=张三  #等同于person.lastName=张三
person.age=20
```

##### propertie中文乱码问题

idea中propertie文件默认使用的是utf-8编码

而实际文件的编码是ASCII码

File --> Setting --> Editor --> File Encodings  右侧底部 有properties文件编码设置

properties默认的编码 设置为 UTF-8 (与实际文件的编码要一致)

勾选 运行时转为ASCII编码 Transparent native-to-asscii conversion

### P13 配置-@ConfigurationProperties与@Value区别

##### @Value方式获取值

@Value方式 等同于 xml配置时代

```xml
<bean class="Person">
    <propertity name="lastName"	value="zhagnsan" />
    <!--
		value 处可以是 
			字面量：字符串、数字、布尔值等
			${key} 从环境变量、配置文件中获取值
			#{SpEL}	spring的表达式语言
	-->
</bean>
```

```java
@Data
@Component
public class Person{
    @Value("${person.lastName}") //不支持    @Value("${person.last-name}")
	private Stirng lastName;
    @Value("#{10*2}")  //SpEL表达式
	private Integer age;
    @Value("true")
	private Boolean boss;
	private Date birth;
	private Map<String,Object> maps;
	private List<Object> lists;
	private Dog dog;
}

```

##### @ConfigurationProperties与@Value区别

|                            | ConfigurationProperties  | @Value         |
| -------------------------- | ------------------------ | -------------- |
| 功能                       | 批量注入配置文件中的属性 | 需要一个个指定 |
| 松散绑定(松散语法)         | 支持                     | 不支持         |
| SpEL（Sping EL表达式语言） | 不支持                   | 支持           |
| JSR303数据校验             | 支持                     | 不支持         |
| 复杂类型封装(Map、List)    | 支持                     | 不支持         |

##### 使用场景

配置文件 不论是yml还是properties，使用这两个注解都能获取到值

如果说，我们只是在业务逻辑中需要获取配置文件的某项值 ，使用@Value

如果说，我们专门编写了一个JavaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties，一次性将全部文件的值 全部注入进来

​			比如数据库配置文件读取类等



##### 松散绑定

person.firstName：使用标准方式

person.firs-name  大写用-代替

person.firs_name 大写用_代替

PERSON_FIRST_NAME 

这几种写法，在松散绑定/松散语法下 都可以绑定到类的属性值

##### 配置文件数据校验

```java
@Data
@Component
@ConfigurationProperties(prefix="person")
@Validated //开启JSR303数据校验
public class Person{
	private Stirng lastName;
	private Integer age;
	private Boolean boss;
	private Date birth;
    @Email  //使用邮箱格式 校验
    private String email;
	private Map<String,Object> maps;
	private List<Object> lists;
	private Dog dog;
}
```

### P14 配置-@PropertySource、@ImportResource、@Bean

#### @PropertySource 

@PropertySource 作用：加载指定的配置文件

@ConfigurationProperties 默认从全局配置文件中获取值；如果想从指定的配置文件中获取值，那么可以使用@PropertySource 来指定配置文件

person.properties

```properties
person.last-name=李四
person.age=21
person.birth=2000/02/12
person.boss=false
```

```java
@Component
@ConfigurationProperties(prefix="person")
@PropertySource(value= {"classpath:person.properties"})  //指定加载类路径下的 person.properties；可以写多个文件名 这里是一个数组
public class Person{
    // ......
}
```

#### @ImportResource

@ImportResource 作用：导入Spring配置文件，让配置文件里面的内容生效；

默认情况下，如果只是写了spring的xml配置文件，不会被自动加载，不会被识别

如果想要让 spring xml配置文件生效，需要加载进来；使用@ImportResource 标注在一个配置类上，也可以标注在主启动类上

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<bean id="helloService" class="com.beyondsoft.springboot.service.HelloService"></bean>
</beans>
```

HelloService

```java
public class HelloService {
    //这里不写任何方法 只为测试是否能够被spring管理，也不写@Service @Componse @Bean 目的就是 默认不被spring管理，
    //而是以导入xml文件的形式 让其被spring管理
}
```

测试容器中有无helloService

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringBootTest{
    
    @Autowired
    AppllicationContext ioc;
    
    @Test
    public void testHelloService(){
        Boolean flag = ioc.containBean("helloService");
        System.out.println(flag);
    }
}
```

@ImportResource 标注标注在配置类上

```java
@SpringBootApplication
@ImportResource(location={"classpath:beans.xml"})
public class SpringBootApplication{
	//main 方法省略......
}
```

使用@ImportResource注解 导入配置文件后，上面的测试 就返回true，表明 ，helloService 已经被正确加载了

#### @Bean

SpringBoot中推荐的给容器添加组件的方式是使用@Bean注解，而不是动态加载xml配置文件

编写配置类

使用@Configuration指明当前是一个配置类，就是替代之前的spring配置文件(xml方式)

在之前的xml配置文件中是使用**`<bean></bean>`**标签来添加组件

@Bean注解 是将方法的返回值添加到容器中，容器中这个组件默认的id就是方法名    

```java
package config;
@Configuration
public class MyAppConfig{
	
    //将方法的返回值添加到容器中，容器中这个组件默认的id就是方法名    
    @Bean
    public HelloService helloService(){
        return new HelloService();
    }
}
```



### P15 配置-配置文件占位符

##### 1、RandomValePropertySource：配置文件中使用随机数

- ${random.value}
- ${random.int}
- ${random.long}
- ${random.int(10)}
- ${random.int[1024,65536]}

##### 2、属性配置占位符

- 可以在配置文件中引用前面 配置过的属性值（优先级前面配置过得这里都能用）
- ${app.name:默认值} 来指定找不到属性时的默认值

##### 3、随机数占位符示例

springboot中，properties和yml都支持 随机数和占位符

```properties
person.last-name=张三${radom.value}
person.age=${random.int}
person.dog.name=${person.last-name}'s dog

person.dog.name=${person.hello}_dog  		###如果取不到person.hello 表达式的值 就会原样输出 {person.hello}作为字符串 原样输出
person.dog.name=${person.hello:hello}_dog  	###如果person.hello 表达式取不出值，就用冒号后面的值来代替
```



### P16 配置-Profile多环境支持

Profile是Spring对不同环境提不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境

##### 1、多Profile文件形式 

​	我们在主配置文件编写时

​	格式 application-{profile}.properties 或 application-{profile}.yml

​	例如：application-dev.properties    application-test.properties    application-prod.properties 

##### 2、多profile文件块模式

使用 --- 划分文档快

```yml
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8083
spring:
  profiles: dev
---
server:
  port: 8084
spring:
  profiles: prod  #指定属于哪个环境
```



##### 3、激活方式

- **命令行**

  Idea中，Run/Debug Configuration配置 ，Program arguments(程序参数): 填入

  ```
  --spring.profiles.active=prod
  ```

  打包成jar包，jar包运行时

  ```
  java -jar springboot-demo.jar --spring.profiles.active=prod
  ```

  

- **配置文件**

  ```yaml
  spring:
    profiles:
      active: dev
  ```

  

- **jvm参数**

  Idea中，Run/Debug Configuration配置 ，VM options:

  ```
  -Dspring.profiles.active=prod
  ```

  

### P17 配置-配置文件的加载位置

##### 六、配置文件加载位置

spring boot启动会扫描以下一致的application.properties或者application.yml 文件作为默认的配置文件

- file: ./config/		      当前项目根目录的 config文件夹下
- file: ./          	             当前项目根目录  
- classpath: /config/	类路径的config文件夹下
- classpath: /		    	类路径的根目录下
- 以上是按照**优先级从高到低的顺序**，所有位置的文件都会被加载（互补配置），**高优先级配置内容会覆盖低优先级配置的内容**
- 我们也可以通过配置spring.config.location来改变默认的配置
- 互补配置：无论在哪个位置配置都会被加载，没有冲突的内容 也会生效 ，无论在哪个优先级

##### 指定配置文件的位置

```properties
spring.config.location=d:/application.propoerties
```

如果在项目中 进行以上配置，这样配置后 发现并不生效

比如在项目内配置了端口A，在外部文件配置不同的端口，配置并不会覆盖。

正确的做法是：运维时，项目打包好以后，使用命令行参数的形式，启动项目式时来指定配置文件的新位置，指定的配置文件和默认加载的这些配置文件会共同起作用，形成互补配置

```bash
java -jar  springboot-demo.jar --spring.config.location=d:/application.propoerties
```



### P18 配置-外部配置加载顺序

##### 七、外部配置加载顺序

SpringBoot支持多种外部配置方式

这些方式优先级如下：

http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-external-config  17个位置

- **1 命令行参数**

  ```bash
  java -jar 项目jar包名 --server.port=8088 --server.context.path=/boot
  ```

- 2 来自java:comp/env的JNDI属性

- 3.Java系统属性(System.getProperties())

- 4 操作系统环境变量

- 5.RandomValuePropertySource配置的random.*属性值

  **优先加载带profile的**，由jar外向内进行加载，高优先级覆盖低优先级:

- **6.jar包外部的application-{profile}.properties 或 application.yml(带spring.profile)配置文件**

- **7.jar包内部的application-{profile}.properties 或 application.yml(带spring.profile)配置文件**

  **再来加载不带profile的**

- **8.jar包外部的application.properties 或 application.yml(不带spring.profile)配置文件**

- **9.jar包外部的application.properties 或 application.yml(不带spring.profile)配置文件**

- 10.@Configuration注解类上的@PropertySource

- 11.通过SpringApplication.setDefaultProperties指定的默认属性

SpringBoot可以从以上位置加载配置，优先级从高到低，高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置



### P19 配置-自动配置原理

#### 八、自动配置原理

##### 自动配置原理纲要

1、可以查看HtpEncodingAutoConfiguration

2、通用模式

	- xxxAutoConfiguration：自动配置类
	- xxxxProperties：属性配置类
	- yml/properties文件中能配置的值就来源于【属性配置类】

3、几个重要的注解

- @Bean
- @Conditonal

4、--debug 查看详细的自动配置报告



配置文件中能写什么，怎么写

配置文件配置属性：https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#common-application-properties

##### 自动配置原理

1、SpringBoot启动的时候加载主配置类，开起来自动配置功能@EnableAutoConfiguration

2、@EnableAutoConfiguration作用：

- 利用EnableAutoConfigurationImportSelector给容器中导入一些组件？

- 可以查看selectImports()方法的内容

- List<String> configurations = getCandidateConfigureations(annotationMetadata, attributes); 	获取候选的配置

  ```java
  SpringFactoriesLoader.loadFactoryNames(); //扫描所有jar包类路径下的 META-INF/spring.factories
  把扫描到的这个文件的内容包装成properties对象
  从properties中获取到EnableAutoConfiguration.class(类名)类的值，然后把它们添加在容器中
      
  ```

  将类路径下META-INF/spring.factories里面配置的所有EnableAutoConfiguration的值加入到了容器中；

  ```properties
  # Auto Configure
  org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
  org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
  org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
  org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
  org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
  org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
  org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
  org.springframework.boot.autoconfigure.context.LifecycleAutoConfiguration,\
  org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
  org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
  org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
  org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRestClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.jdbc.JdbcRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.r2dbc.R2dbcDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.r2dbc.R2dbcRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.r2dbc.R2dbcTransactionManagerAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
  org.springframework.boot.autoconfigure.elasticsearch.ElasticsearchRestClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
  org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
  org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
  org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
  org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
  org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
  org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
  org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
  org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration,\
  org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration,\
  org.springframework.boot.autoconfigure.influx.InfluxDbAutoConfiguration,\
  org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
  org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
  org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
  org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
  org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
  org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
  org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
  org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
  org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
  org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
  org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration,\
  org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
  org.springframework.boot.autoconfigure.availability.ApplicationAvailabilityAutoConfiguration,\
  org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
  org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
  org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
  org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
  org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
  org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
  org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
  org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration,\
  org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
  org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
  org.springframework.boot.autoconfigure.quartz.QuartzAutoConfiguration,\
  org.springframework.boot.autoconfigure.r2dbc.R2dbcAutoConfiguration,\
  org.springframework.boot.autoconfigure.rsocket.RSocketMessagingAutoConfiguration,\
  org.springframework.boot.autoconfigure.rsocket.RSocketRequesterAutoConfiguration,\
  org.springframework.boot.autoconfigure.rsocket.RSocketServerAutoConfiguration,\
  org.springframework.boot.autoconfigure.rsocket.RSocketStrategiesAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.servlet.UserDetailsServiceAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.reactive.ReactiveSecurityAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.reactive.ReactiveUserDetailsServiceAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.rsocket.RSocketSecurityAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.saml2.Saml2RelyingPartyAutoConfiguration,\
  org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
  org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.oauth2.client.servlet.OAuth2ClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.oauth2.client.reactive.ReactiveOAuth2ClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.oauth2.resource.servlet.OAuth2ResourceServerAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.oauth2.resource.reactive.ReactiveOAuth2ResourceServerAutoConfiguration,\
  org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
  org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration,\
  org.springframework.boot.autoconfigure.task.TaskSchedulingAutoConfiguration,\
  org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
  org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
  org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
  org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.HttpHandlerAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.ReactiveWebServerFactoryAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.WebFluxAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.error.ErrorWebFluxAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
  org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
  org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
  org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
  org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration,\
  org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration
  ```
  
  每一个这样的xxxAutoConfiguration类都是容器中的一个组件，都加入到容器中；用他们来做自动配置
  

3、每一个自动配置类进行自动配置功能；

4、以HttpEncodingAutoConfiguration（Http编码自动配置）为例分析自动配置原理

```java
@Configuration	//表示这是一个配置类，以前
@EnableConfigurationProperties({HttpEncodingProperties.class})  //启用指定类的ConfigurationProperties功能；
				//将配置文件中对应的值与HttpEncodingProperties绑定起来了，并把HttpEncodingProperties加入到IoC容器中
@ConditionalOnWebApplication	//Spring底层@Conditional注解，根据不同的条件，如果满足指定的条件，整个配置类里面的配置就会生效； 作用是判断当前应用是否web应用，如果是，当前配置类生效
@ConditionalOnClass({CharacterEncodingFilter.class}) //判断当前项目有没有这个类，CharacterEncodingFilter是SpringMVC中进行乱码解决的过滤器
@ConditionalOnProperty(  //判断配置文件中是否存在某个配置server.servlet.encoding.enabled,如果不存在也认为这个配置是成立的，如果不配做也是默认生效的
    prefix = "server.servlet.encoding",
    value = {"enabled"},
    matchIfMissing = true
)
public class HttpEncodingAutoConfiguration {
    
    private final Encoding properties; //它已经和springboot的配置硬件做了映射
    
    //只有一个有参构造器，参数的值就会从容器中获取
    public HttpEncodingAutoConfiguration(ServerProperties properties) {
        this.properties = properties.getServlet().getEncoding();
    }
    
    @Bean  //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @ConditionalOnMissingBean
    public CharacterEncodingFilter characterEncodingFilter() {
```

根据当前不同的条件判断，决定这个配置类是否生效

一旦这个类生效，这个配置类就会给容器中添加各种组件，这些组件的属性是从对应properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；

能配置的属性都是来源于这个功能的properties类

5、所有在配置文件中能配置的属性都是在xxxProperties类中封装着，配置文件能配置什么属性，就可以参照这个功能对应的这个属性类

```java
@ConfigurationProperties(	//从配置文件中获取指定的值和Bean的属性进行绑定
    prefix = "server",
    ignoreUnknownFields = true
)
public class HttpEncodingProperties {
```

##### SpringBoot精髓：

**1、SpringBoot启动会加载大量的自动配置类**

**2、我们需要的功能有没有SpringBoot自动配置类**

**3、如果有，我们再来看这个自动配置类中到底配置了哪些组件（只要我们需要的组件已经有了，就不需要再配了）**

**4、给容器中自动添加组件的时候，会从properties类中获取某些属性，我们就可以在配置文件中配置这些属性的值**

**xxxAutoConfiguration： 自动配置类 给容器中添加组件**

**xxxProperties：封装配置文件中相关属性**



### P20 配置-@Conditional&自动配置报告

##### 1、@Conditional派生的注解（Srping注解元素的@Conditional）

如@ConditionalOnXXXX

作用：必须是通过@Conditional指定的条件成立，才会给容器中添加组件，或者配置方法里面的内容才会生效；

| @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的java版本是否符合要求                       |
| @ConditionalOnBean              | 容器中存在指定Bean；                             |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean；没有这个Bean               |
| @ConditionalOnExpression        | 满足SpEL表达式指定                               |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissingClass      | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |

虽然加载了很多配置类，但是自动配置类必须在一定的条件下才能生效

##### 2、如何确定哪些配置类生效，哪些没有生效

```properties
debug=true #开启springboot的debug模式
```

打印的信息

**Positive matches**
匹配成功
表示，启用的自动配置类

**Negative matches**
没有匹配成功
表示，没有启用的自动配置类

![img](https://img-blog.csdn.net/20180901101734864?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25hbmdlYWxp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 三、SpringBoot与日志

日志框架、日志配置

### P21 日志-日志框架分类和选择

#### 1、日志框架产生背景

> 小张 开发了一个大型系统；
>
>  1、System.out.println("")；将关键数据打印在控制台；每次上线都要去掉，很麻烦，就想把输出写在一个文件里
>
>  2、框架来记录系统的一些运行时信息；开发了日志框架 ； zhanglogging.jar；
>
>  3、又想高大上的几个功能，例如：异步模式？自动归档？xxxx？ 就开发了zhanglogging-good.jar
>
>  4、那就需要将以前框架卸下来，换上新的框架，重新修改之前相关的API；可以以后再开发zhanglogging-prefect.jar的时候又需要来这样装卸一遍，很麻烦。
>
>  5、想到JDBC--数据库驱动的关系，于是考虑
>
>  写了一个统一的接口层；日志门面（日志的一个抽象层）；logging-abstract.jar；
>
>  给项目中导入具体的日志实现就行了；我们之前的日志框架都是实现的抽象层；

**市面上的日志框架；**

JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j…

| 日志门面 （日志的抽象层）                                    | 日志实现                                           |
| :----------------------------------------------------------- | :------------------------------------------------- |
| ~~JCL（Jakarta Commons Logging）~~ SLF4j（Simple Logging Facade for Java） ~~**jboss-logging**~~ | Log4j  JUL（java.util.logging） Log4j2 **Logback** |

左边选一个门面（抽象层）、右边来选一个实现；

日志门面： SLF4J；

日志实现：Logback；

SpringBoot：底层是Spring框架，Spring框架默认是用JCL；

**而SpringBoot选用 SLF4j和logback；**选择最好的一个组合。



### P22 日志-slf4j使用原理

#### 2、SLF4j使用

##### 2.1、如何在系统中使用SLF4j 

​	官网：https://www.slf4j.org

​	手册：http://www.slf4j.org/manual.html

以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法；

首先在系统里导入slf4j的jar和logback的jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

和各种实现类的配合使用图示；

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020101103503736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE3MDc5MjU1,size_16,color_FFFFFF,t_70#pic_center)

图中浅 蓝色为接口层，深蓝色为实现层，绿色为适配层

每一个日志的实现框架都有自己的配置文件。使用slf4j以后，**配置文件还是做成日志实现框架自己本身的配置文件；**



### P23 日志-其他日志框架统一转换为slf4j

##### 2.2、遗留问题

开发A系统（slf4j+logback）：要整合 Spring（commons-logging）、Hibernate（jboss-logging）、MyBatis、xxxx

如何统一日志记录，即使是别的框架和我一起统一使用slf4j进行输出？

![img](https://img-blog.csdnimg.cn/20201011035107572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE3MDc5MjU1,size_16,color_FFFFFF,t_70#pic_center)

**如何让系统中所有的日志都统一到slf4j；**

1、将系统中其他日志框架先排除出去；

2、用中间包（适配器）来替换原有的日志框架；

3、我们导入slf4j其他的实现



### P24 日志-SpringBoot日志关系

#### 3、SpringBoot日志关系

##### 1 分析SpringBoot日志依赖关系

创建项目 SpringBoot-Logging 选择依赖Web模块

在pom.xml中 右键Diagrams--->Show Dependencies...(Ctrl+Alt+Shift+U)  以图谱方式展示模块依赖关系

- spring-boot-starter-web依赖 spring-boot-starter

- spring-boot-starter依赖 spring-boot-starter-logging

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
```

SpringBoot使用它来做日志功能；

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-logging</artifactId>
		</dependency>
```

##### 2 SpringBoot底层依赖关系

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201011035130825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE3MDc5MjU1,size_16,color_FFFFFF,t_70#pic_center)

##### 3 **总结**

 1）、SpringBoot底层也是使用slf4j+logback的方式进行日志记录

 2）、SpringBoot也把其他的日志都替换成了slf4j；

 3）、中间替换包（适配器）代码如下：其实都是内部使用了slf4j

```java
@SuppressWarnings("rawtypes")
public abstract class LogFactory {
    static String UNSUPPORTED_OPERATION_IN_JCL_OVER_SLF4J = "http://www.slf4j.org/codes.html#unsupported_operation_in_jcl_over_slf4j";
    static LogFactory logFactory = new SLF4JLogFactory();
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201011035154906.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE3MDc5MjU1,size_16,color_FFFFFF,t_70#pic_center)

 4）、如果我们要引入其他框架，一定要把这个框架的默认日志依赖移除掉，

 例如，Spring框架用的是commons-logging；那么SpringBoot 是否排除掉了spring底层的commons-logging？

```xml
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<exclusions>
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
```

因为他默认的日志包和上图的适配器的包名其实是一样的，都是commons.logging

**SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可；**



### P25 日志-SpringBoot默认配置

#### 4、SpringBoot项目中日志的使用

##### 4.1 默认配置

SpringBoot默认帮我们配置好了日志；

```java
	//记录器
	Logger logger = LoggerFactory.getLogger(getClass());
	
	@Test
	public void contextLoads() {
		//System.out.println();
 
		//日志的级别；
		//由低到高   trace < debug < info < warn < error
		//可以调整输出的日志级别；日志就只会在这个级别及以后的高级别生效
		logger.trace("这是trace日志...");	//跟踪轨迹
		logger.debug("这是debug日志...");	//调试
		//SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别
		logger.info("这是info日志...");		//信息
		logger.warn("这是warn日志...");		//警告
		logger.error("这是error日志...");	//错误  
	}
```

SpringBoot修改日志的默认配置

```properties
# 输出的级别
logging.level.com.atguigu=trace

# 不指定路径在当前项目下生成springboot.log日志
#logging.file=springboot.log
 
# 可以指定完整的路径；
#logging.file=G:/springboot.log
 
# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用默认的名称 spring.log 作为默认文件
# /是根路径
logging.path=/spring/log
 
# 在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
# 指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} ==== %msg%n
```

```xml
    日志输出格式：
		%d表示日期时间，
		%thread表示线程名，
		%-5level：级别从左显示5个字符宽度
		%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
		%msg：日志消息，
		%n是换行符
    -->
    %d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
```

默认每10M 进行日志分割

### P26 日志-指定日志文件和日志Profile功能

##### 4.2 指定配置

给类路径下放上每个日志框架自己的配置文件即可；SpringBoot就不使用他默认配置的了

| Logging System          | Customization                                                |
| :---------------------- | :----------------------------------------------------------- |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

logback.xml：直接就被日志框架识别了；

**logback-spring.xml**：日志框架就不直接加载日志的配置项，由SpringBoot解析日志配置，可以使用SpringBoot的高级Profile功能

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
  	可以指定某段配置只在某个环境下生效
</springProfile>
```

如果使用logback.xml作为日志配置文件，还要使用profile功能，会有以下错误

```
no applicable action for [springProfile]
```

Profile配置示例

```xml

<appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--
        日志输出格式：
			%d表示日期时间，
			%thread表示线程名，
			%-5level：级别从左显示5个字符宽度
			%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
			%msg：日志消息，
			%n是换行符
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>
```



### PP27 日志-切换日志框架

##### 5.1 切换为slf4j+log4j 的方式（不推荐）

例如：切换到log4j（这里只是做例子，实际上没有替换的必要，因为Logback就是因为log4j不好用才重写的）

slf4j+log4j的方式；

可以按照slf4j的日志适配图，进行相关的切换（移除logback，移除替换包，加进slf4j-log4j12）

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <artifactId>logback-classic</artifactId>
      <groupId>ch.qos.logback</groupId>
    </exclusion>
    <exclusion>
      <artifactId>log4j-over-slf4j</artifactId>
      <groupId>org.slf4j</groupId>
    </exclusion>
  </exclusions>
</dependency>
 
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
</dependency>
```

##### 5.2 切换为 slf4j+log4j2 的方式

还有一种方法，springboot的日志默认使用是spring-boot-starter-logging，其实还有一个spring-boot-starter-log4j2，他里边用的就是log4j，所以可以直接切换为log4j2

```xml
	   <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-logging</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
        </dependency>
 
		<dependency>
		  <groupId>org.springframework.boot</groupId>
		  <artifactId>spring-boot-starter-log4j2</artifactId>
		</dependency>
```

## 四、SpringBoot与Web开发

Tymeleaf Web定制 容器定制

### P28 web开发-简介

##### 使用SpringBoot

1）、创建SpringBoot应用，选中我们需要的模块

2）、SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来

3）、自己编写业务代码；

##### 自动配置原理？

这个场景SpringBoot帮我们配置了什么？能不能修改？能修改哪些配置？能不能扩展？....

```
xxxxAutoConfiguration：帮我们给容器中自动配置组件；
xxxxProperties:配置类来封装配置文件的内容；
```

### P29 web开发-webjars&静态资源映射规则

#### 4.2 webjars&静态资源映射规则

##### SpringBoot对静态资源的映射规则

```java
@ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
public class ResourceProperties implements ResourceLoaderAware {
  //可以设置和静态资源有关的参数，缓存时间等
```

​	WebMvcAuotConfiguration文件：

```java
		@Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
			Integer cachePeriod = this.resourceProperties.getCachePeriod();
			if (!registry.hasMappingForPattern("/webjars/**")) {
				customizeResourceHandlerRegistration(
						registry.addResourceHandler("/webjars/**")
								.addResourceLocations(
										"classpath:/META-INF/resources/webjars/")
						.setCachePeriod(cachePeriod));
			}
			String staticPathPattern = this.mvcProperties.getStaticPathPattern();
          	//静态资源文件夹映射
			if (!registry.hasMappingForPattern(staticPathPattern)) {
				customizeResourceHandlerRegistration(
						registry.addResourceHandler(staticPathPattern)
								.addResourceLocations(
										this.resourceProperties.getStaticLocations())
						.setCachePeriod(cachePeriod));
			}
		}
 
        //配置欢迎页映射
		@Bean
		public WelcomePageHandlerMapping welcomePageHandlerMapping(
				ResourceProperties resourceProperties) {
			return new WelcomePageHandlerMapping(resourceProperties.getWelcomePage(),
					this.mvcProperties.getStaticPathPattern());
		}
 
       //配置喜欢的图标
		@Configuration
		@ConditionalOnProperty(value = "spring.mvc.favicon.enabled", matchIfMissing = true)
		public static class FaviconConfiguration {
 
			private final ResourceProperties resourceProperties;
 
			public FaviconConfiguration(ResourceProperties resourceProperties) {
				this.resourceProperties = resourceProperties;
			}
 
			@Bean
			public SimpleUrlHandlerMapping faviconHandlerMapping() {
				SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
				mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
              	//所有  **/favicon.ico 
				mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
						faviconRequestHandler()));
				return mapping;
			}
 
			@Bean
			public ResourceHttpRequestHandler faviconRequestHandler() {
				ResourceHttpRequestHandler requestHandler = new ResourceHttpRequestHandler();
				requestHandler
						.setLocations(this.resourceProperties.getFaviconLocations());
				return requestHandler;
			}
 
		}
```

1)、所有/webjars/** ，都去zclasspath:/META-INF/resources/webjars/找资源；

​    webjars：以jar包的方式引入静态资源：https://www.webjars.org/

```xml
<!-- https://mvnrepository.com/artifact/org.webjars/jquery -->
<!--引入jquery-webjar，在访问的时候只需要写webjars下面资源的名称即可-->
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.3.1</version>
</dependency>
```

![img](https://img-blog.csdnimg.cn/20190811093123226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

http://localhost:8080/webjars/jquery/3.3.1/jquery.js

2）、"/**" 访问当前项目的任何资源，都去（静态资源的文件夹）找映射

```

"classpath:/META-INF/resources/", 
"classpath:/resources/",
"classpath:/static/", 
"classpath:/public/" 
"/"：当前项目的根路径
```

localhost:8080/abc === 去静态资源文件夹里面找abc  

3）、欢迎页； 静态资源文件夹下的所有index.html页面；被"/**"映射；

localhost:8080/ 找index页面

4）、所有的 **/favicon.ico 都是在静态资源文件下找

5）、除SpringBoot默认之外，如何自己指定静态资源文件夹，配置静态资源文件夹

```properties
spring.resources.static-location=classpath:/hello/,classpath:/files/   #自定义多个文件夹 自定义之后 之前springboot默认的就不能访问了
```



### P30 web开发-引入thymeleaf

#### 4.3 引入Thymeleaf模板引擎

模板引擎：JSP、Velocity、Freemarker、Thymeleaf

![img](https://img-blog.csdnimg.cn/20190812064638236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

SpringBoot推荐的Thymeleaf；

语法更简单，功能更强大；

**引入thymeleaf；**

```xml
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
<!-- -->
```

默认的2.1.6版本太低，推荐使用3.x.x版本，如何切换为高版本呢，在pom文件中的`<properties>`标签内 替换原来的版本号，注意 与layout的版本对应

```xml
<properties>
     <java.version>1.8</java.version>
     <!-- 切换thymeleaf版本 -->
     <thymeleaf.version>3.0.11.RELEASE</thymeleaf.version>
     <!-- 布局功能的支持程序  thymeleaf3主程序  layout2以上版本 -->
     <!-- thymeleaf2 和 layout1 适配-->
     <thymeleaf-layout-dialect.version>2.4.1</thymeleaf-layout-dialect.version>
</properties>
```



### P31 web开发-thymeleaf语法

#### 4.4 thymeleaf语法

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {
 
	private static final Charset DEFAULT_ENCODING = Charset.forName("UTF-8");
 
	private static final MimeType DEFAULT_CONTENT_TYPE = MimeType.valueOf("text/html");
 
	public static final String DEFAULT_PREFIX = "classpath:/templates/";
 
	public static final String DEFAULT_SUFFIX = ".html";
```

只要我们把HTML页面放在classpath:/templates/，thymeleaf就能自动渲染；

使用：

语法：参考官方文档  https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.pdf

##### **1、导入thymeleaf的名称空间**

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<!-- 导入后该语法会有提示信息 -->
```

##### **2、使用thymeleaf语法**

controller：

```java
@Controller
public class TestContorller {
 
    @RequestMapping("/success")
    public String success( Map<String,Object> map){
        map.put("hello", "hello world");
        return "success";
    }
}
```

html:

```html

<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>成功！</h1>
    <!--th:text 将div里面的文本内容设置为我们指定的值 -->
    <div th:text="${hello}">这是显示欢迎信息</div>
</body>
</html>
```

启动SpringBoot，访问http://localhost:8081/success

![img](https://img-blog.csdnimg.cn/20190812072330654.png)

##### 3、语法规则

1）、th:text：改变当前元素里面的文本内容；

​     th：任意html属性；来替换原生属性的值

```html
<div id="div01" class="class01" th:id="${hello}" th:class="${hello}" th:text="${hello}" > 欢迎信息</div>
 th：任意html属性；来替换原生属性的值
```

![img](https://img-blog.csdnimg.cn/20190813070738125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

2）表达式

1. Simple expressions:（表达式语法）

```properties
Simple expressions:（表达式语法）
	Variable Expressions: ${...}：获取变量值；OGNL；
    1）、获取对象的属性、调用方法
    2）、使用内置的基本对象：
    	#ctx : the context object.
    	#vars: the context variables.
        #locale : the context locale.
        #request : (only in Web Contexts) the HttpServletRequest object.
        #response : (only in Web Contexts) the HttpServletResponse object.
        #session : (only in Web Contexts) the HttpSession object.
        #servletContext : (only in Web Contexts) the ServletContext object.
          eg： ${session.foo}
     3）、内置的一些工具对象：
        #execInfo : information about the template being processed.
        #messages : methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
        #uris : methods for escaping parts of URLs/URIs
        #conversions : methods for executing the configured conversion service (if any).
        #dates : methods for java.util.Date objects: formatting, component extraction, etc.
        #calendars : analogous to #dates , but for java.util.Calendar objects.
        #numbers : methods for formatting numeric objects.
        #strings : methods for String objects: contains, startsWith, prepending/appending, etc.
        #objects : methods for objects in general.
        #bools : methods for boolean evaluation.
        #arrays : methods for arrays.
        #lists : methods for lists.
        #sets : methods for sets.
        #maps : methods for maps.
        #aggregates : methods for creating aggregates on arrays or collections.
        #ids : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).
 
2.Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
   补充：配合 th:object="${session.user}：
       <div th:object="${session.user}">
            <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
            <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
            <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
       </div>
    
3.Message Expressions: #{...}：获取国际化内容
4.Link URL Expressions: @{...}：定义URL；
    @{/order/process(execId=${execId},execType='FAST')}
5.Fragment Expressions: ~{...}：片段引用表达式
    <div th:insert="~{commons :: main}">...</div>
```

2.Literals（字面量）

```properties
  Text literals: 'one text' , 'Another one!' ,…
  Number literals: 0 , 34 , 3.0 , 12.3 ,…
  Boolean literals: true , false
  Null literal: null
  Literal tokens: one , sometext , main ,…
```

3.Text operations:（文本操作）

```properties
 String concatenation: +
 Literal substitutions: |The name is ${name}|
```

4.Arithmetic operations:（数学运算）

```properties
Binary operators: + , - , * , / , %
Minus sign (unary operator): -
```

5.Boolean operations:（布尔运算）

```properties
Binary operators: and , or
Boolean negation (unary operator): ! , not
```

6.Comparisons and equality:（比较运算）

```properties
 Comparators: > , < , >= , <= ( gt , lt , ge , le )
 Equality operators: == , != ( eq , ne )
```

7.Conditional operators:条件运算（三元运算符）

```properties
If-then: (if) ? (then)
If-then-else: (if) ? (then) : (else)
Default: (value) ?: (defaultvalue)
```

8.Special tokens:(特殊操作)

```properties
无操作
No-Operation: _ 
```

##### 4、练习

html：

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>成功！</h1>
<!--th:text 将div里面的文本内容设置为 -->
<div th:id="${hello}" th:text="${hello}">这是显示欢迎信息</div>
 
<hr/>
<div th:text="${title}">title</div>
<div th:utext="${title}">title</div>
</hr>
<h1>th:each 使用方法一</h1>
<!-- th:each 每次遍历都会生成当前的这个标签 生成3个h4-->
<h4 th:text="${user}" th:each = "user : ${users}"/>
 
<h1>th:each 使用方法二</h1>
<h4>
    <!-- h4 下 有3个 span -->
    <span th:each="user : ${users}"> [[${user}]]</span>
</h4>
</body>
</html>
```

运行：

![img](https://img-blog.csdnimg.cn/20190813073702784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

网页源代码：

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>成功！</h1>
<!--th:text 将div里面的文本内容设置为 -->
<div id="hello world">hello world</div>
 
<hr/>
<div>&lt;h1&gt;你好&lt;/h1&gt;</div>
<div><h1>你好</h1></div>
</hr>
<h1>th:each 使用方法一</h1>
<!-- th:each 每次遍历都会生成当前的这个标签 生成3个h4-->
<h4>zhangsan</h4>
<h4>lisi</h4>
<h4>wangwu</h4>
 
<h1>th:each 使用方法二</h1>
<h4>
    <span> zhangsan</span><span> lisi</span><span> wangwu</span>
</h4>
</body>
</html>
```

### P32 web开发-SpringMVC自动配置原理

#### 1. Spring MVC auto-configuration

[官方文档](https://docs.spring.io/spring-boot/docs/1.5.10.RELEASE/reference/htmlsingle/#boot-features-developing-web-applications)

Spring Boot 自动配置好了SpringMVC

以下是SpringBoot对SpringMVC的默认配置:**（WebMvcAutoConfiguration）**

- Inclusion of **`ContentNegotiatingViewResolver`** and **`BeanNameViewResolver`** beans.

  自动配置了ViewResolver（视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发？重定向？））

  ContentNegotiatingViewResolver：组合所有的视图解析器的；

  如何定制：我们可以自己给容器中添加一个视图解析器；ContentNegotiatingViewResolver就会自动的将其组合进来；

- Support for serving static resources, including support for WebJars (see below). //静态资源文件夹路径,webjars

- Static **`index.html`** support. //静态首页访问

- Custom **`Favicon`** support (see below). //favicon.ico

- 自动注册了 of **`Converter`**, **`GenericConverter`**, **`Formatter`** beans.

  Converter：转换器； public String hello(User user)：类型转换使用Converter

  **`Formatter`** 格式化器； 2017.12.17===Date；

  ```java
  @Bean
  @ConditionalOnProperty(prefix = "spring.mvc", name = "date-format")//在配置文件中 配置日期格式化的规则
  public Formatter<Date> dateFormatter() {
      return new DateFormatter(this.mvcProperties.getDateFormat());//日期格式化组件
  }
  ```
  
  **自己添加的格式化器转换器，我们只需要放在容器中即可**
  
- Support for **`HttpMessageConverters`** (see below).

  HttpMessageConverter：SpringMVC用来转换Http请求和响应的；User---Json；

  `HttpMessageConverters` 是从容器中确定；获取所有的HttpMessageConverter；

  自己给容器中添加HttpMessageConverter，只需要将自己的组件注册容器中（@Bean,@Component)

- Automatic registration of `MessageCodesResolver` (see below).  //定义错误代码生成规则

- Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).

  我们可以配置一个ConfigurableWebBindingInitializer来替换默认的；（需要添加到容器中）

  ```
  初始化WebDataBinder；
  请求数据=====JavaBean；
  ```

**org.springframework.boot.autoconfigure.web：web的所有自动场景；**

If you want to keep Spring Boot MVC features, and you just want to add additional [MVC configuration](https://docs.spring.io/spring/docs/4.3.14.RELEASE/spring-framework-reference/htmlsingle#mvc) (interceptors, formatters, view controllers etc.) you can add your own `@Configuration` class of type `WebMvcConfigurerAdapter`, but **without** `@EnableWebMvc`. If you wish to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter` or `ExceptionHandlerExceptionResolver` you can declare a `WebMvcRegistrationsAdapter` instance providing such components.If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`.

#### 2. 如何修改SpringBoot的默认配置

模式：

1）、SpringBoot在自动配置很多组件的时候，先查看容器中有没有用户自己配置的组件，如果有则使用用户配置的，如果没有才走动配置；如果有些组件可以有多个（如：ViewResolver）则将用户配置的和自己默认的组合起来；

2）、如果您想保留springbootmvc特性，并且只想添加额外的[MVC配置]，您可以添加自己的“WebMVCConfigureAdapter”类型的“@Configuration”类

​	将在下一节中介绍



### P33 web开发-扩展与全面接管SpringMVC

#### 1.扩展springMVC

以前的springmvc.xml文件

```xml
    <!-- 视图映射，发hello请求的时候也是映射到success页面 -->
    <mvc:view-controller path="/hello" view-name="success"/>
 
    <!-- springMVC 拦截器 拦截hello请求，利用bean拦截 -->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/hello"/>
            <bean></bean>
        </mvc:interceptor>
    </mvc:interceptors>
```

现在继续翻译理解上面一段：you can add your own `@Configuration` class of type `WebMvcConfigurerAdapter`, but without `@EnableWebMvc`. 

**需要编写一个配置类（@Configuration），类型是`WebMvcConfigurerAdapter，不能标注` `@EnableWebMvc`**

既保留了所有的自动配置，也能用我们扩展的配置；

原理：

1）、WebMvcAutoConfiguration是SpringMVC的自动配置类

2）、在做其他自动配置时会导入：@Import(**EnableWebMvcConfiguration**.class)	

```java
    @Configuration
	public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration {
      private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
 
	 //从容器中获取所有的WebMvcConfigurer
      @Autowired(required = false)
      public void setConfigurers(List<WebMvcConfigurer> configurers) {
          if (!CollectionUtils.isEmpty(configurers)) {
              this.configurers.addWebMvcConfigurers(configurers);
              //一个参考实现；将所有的WebMvcConfigurer相关配置都来一起调用；  
              //@Override
              // public void addViewControllers(ViewControllerRegistry registry) {
              //    for (WebMvcConfigurer delegate : this.delegates) {
              //       delegate.addViewControllers(registry);
              //    }
              //  }
          }
	}
```

3）、容器中所有的WebMvcConfigurer都会一起起作用；

4）、我们的配置类也会被调用；

效果：SpringMVC的自动配置和我们的扩展配置都会起作用

#### 2.全面接管SpringMVC；

不加@EnableWebMvc的情况下，既保留了所有的自动配置，也能用我们扩展的配置；

```java
//使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {
 
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
       // super.addViewControllers(registry);
        //浏览器发送 /atguigu 请求来到 success
        registry.addViewController("/atguigu").setViewName("success");
    }
}
```

加@EnableWebMvc的情况下，SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己配置；所有的SpringMVC的自动配置都失效了

**我们需要在配置类中添加@EnableWebMvc即可；**

```java
//使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
@EnableWebMvc
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {
 
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
       // super.addViewControllers(registry);
        //浏览器发送 /atguigu 请求来到 success
        registry.addViewController("/atguigu").setViewName("success");
    }
}
```

原理：

为什么加了@EnableWebMvc自动配置就失效了；

1）@EnableWebMvc的核心

```java
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
```

2）DelegatingWebMvcConfiguration类

```java
@Configuration
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
```

3）WebMvcAutoConfiguration类中的ConditionalOnMissingBean注解

```java
@Configuration
@ConditionalOnWebApplication
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class,
        WebMvcConfigurerAdapter.class })
//容器中没有这个组件的时候，这个自动配置类才生效
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class,
        ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
```

4）@EnableWebMvc将WebMvcConfigurationSupport组件导入进来；

5）导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能；

#### 3.如何修改SpringBoot的默认配置方法二

模式：

1）、SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有，才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合起来；

2）、在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置

3）、在SpringBoot中会有很多的xxCustomizer帮助我们进行定制配置

```
快捷键ctrl+o ：打开可重写的方法列表
```

### P34 web开发-【实验】-引入资源

#### 4.7 引入资源

##### 1.设置默认首页-静态资源文件夹下的templates

**方法一：**

```java
    //访问“/，/index.html”的时候会去静态资源文件templetas下查找以index命名的html文件
    @RequestMapping({"/", "/index.html"})
    public String index(){
        return "index";
    }
```

**方法二：**

```java
//@EnableWebMvc 不要接管SpringMVC
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {
	@Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //super.addViewControllers(registry);
        //浏览器发送xxx(前一个参数) 请求来到xxx页面（后一个参数）
        registry.addViewController("/ella").setViewName("success");
        registry.addViewController("/").setViewName("login");
        registry.addViewController("/index.html").setViewName("login");
 
    }
}
```

**方法三：** 在MyMvcConfig类里面 再添加一个方法

```java
//@EnableWebMvc 不要接管SpringMVC
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {
	//所有的WebMvcConfigurerAdapter组件都会一起起作用
    //将组件注册在容器
    @Bean
    public WebMvcConfigurer webMvcConfigurerAdapter(){
        WebMvcConfigurer adapter = new WebMvcConfigurer() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/index.html").setViewName("login");
            }
        };
        return adapter;
    }
}
```

##### 2.修改静态页面的资源引用

1）、引入bootstrap和jQuery

```xml
    	<!-- 引入jquery -->
		<dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.3.1</version>
        </dependency>
    	<!-- 引入bootstrap -->
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>bootstrap</artifactId>
            <version>4.0.0</version>
        </dependency>
```

2）、修改静态文件及图片引用

```html
<link href="asserts/css/bootstrap.min.css" th:href="@{/webjars/bootstrap/4.0.0/css/bootstrap.css}" rel="stylesheet">
 
<link href="asserts/css/signin.css" th:href="@{asserts/css/signin.css}" rel="stylesheet">
 
<img class="mb-4" src="asserts/img/bootstrap-solid.svg" th:src="@{asserts/img/bootstrap-solid.svg}" alt="" width="72" height="72">
```

问题：

一：方法一执行的时候报错“Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request p”

解决方法：将启动类里写的viewController返回null的方法删掉

```java
    @Bean
    public ViewResolver myViewResolver(){
        return null;
    }
```

### P35 web开发-【实验】-国际化

#### 4.8 国际化

##### ***SpringMVC配置过程：\***

**1）、编写国际化配置文件；**

2）、使用ResourceBundleMessageSource管理国际化资源文件

3）、在jsp页面使用fmt:message取出国际化内容

##### ***SpringBoot配置过程\***

步骤：

1）、编写国际化配置文件，抽取页面需要显示的国际化消息

![img](https://img-blog.csdnimg.cn/20190817071157575.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

2）、SpringBoot自动配置好了管理国际化资源文件的组件；

```java
@ConfigurationProperties(prefix = "spring.messages")
public class MessageSourceAutoConfiguration {
    
    /**
	 * Comma-separated list of basenames (essentially a fully-qualified classpath
	 * location), each following the ResourceBundle convention with relaxed support for
	 * slash based locations. If it doesn't contain a package qualifier (such as
	 * "org.mypackage"), it will be resolved from the classpath root.
	 */
	private String basename = "messages";  
    //我们的配置文件可以直接放在类路径下叫messages.properties；
    //如果配置文件没有直接放在类路径下，则需要在配置文件中配置spring.message.basename指定基础名
    //如：spring.messages.basename=i18n/login
    
    @Bean
	public MessageSource messageSource() {
		ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
		if (StringUtils.hasText(this.basename)) {
            //设置国际化资源文件的基础名（去掉语言和国家代码的）
			messageSource.setBasenames(StringUtils.commaDelimitedListToStringArray(
					StringUtils.trimAllWhitespace(this.basename)));
		}
		if (this.encoding != null) {
			messageSource.setDefaultEncoding(this.encoding.name());
		}
		messageSource.setFallbackToSystemLocale(this.fallbackToSystemLocale);
		messageSource.setCacheSeconds(this.cacheSeconds);
		messageSource.setAlwaysUseMessageFormat(this.alwaysUseMessageFormat);
		return messageSource;
	}
```

指定 国际化配置文件的位置 和名称

默认情况下 配置文件放到类路径下，叫messages.propertie，如果不是这个默认配置，需要在配置文件中指定

```properties
spring.message=i18n.login
```

3、去页面获取国际化的值；

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
		<meta name="description" content="">
		<meta name="author" content="">
		<title>Signin Template for Bootstrap</title>
		<!-- Bootstrap core CSS -->
		<link href="asserts/css/bootstrap.min.css" th:href="@{/webjars/bootstrap/4.0.0/css/bootstrap.css}" rel="stylesheet">
		<!-- Custom styles for this template -->
		<link href="asserts/css/signin.css" th:href="@{asserts/css/signin.css}" rel="stylesheet">
	</head>
 
	<body class="text-center">
		<form class="form-signin" action="dashboard.html">
			<img class="mb-4" src="asserts/img/bootstrap-solid.svg" th:src="@{asserts/img/bootstrap-solid.svg}" alt="" width="72" height="72">
			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
			<label class="sr-only" th:text="#{login.username}">Username</label>
			<input type="text" class="form-control" placeholder="Username" th:placeholder="#{login.username}" required="" autofocus="">
			<label class="sr-only" th:text="#{login.password}">Password</label>
			<input type="password" class="form-control" placeholder="Password" th:placeholder="#{login.password}"required="">
			<div class="checkbox mb-3">
				<label>
          <input type="checkbox" value="remember-me"> [[#{login.remember}]]
        </label>
			</div>
			<button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.btn}">Sign in</button>
			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
			<a class="btn btn-sm">中文</a>
			<a class="btn btn-sm">English</a>
		</form> 
	</body> 
</html>
```

效果：根据浏览器语言设置的信息切换了国际化；

![img](https://img-blog.csdnimg.cn/img_convert/17ddcbc46151bdb1c0b25a5e748830e9.png)

中文乱码问题：配置文件跟随系统为GBK编码，而Java默认是以ISO-8859-1的编码读取配置的，所以会出现乱码。

需要修改properties文件默认为utf8 ，并且自动转为ascii保存

项目设置 File =>  Setting => Editor Encodings => File Encodings   设置Properties文件 的默认编码为utf-8 并且自动转换为ascii编码保存

全局设置 File => Other Setting => Default Setting  => Editor Encodings => File Encodings  设置Properties文件 的默认编码为utf-8 并且自动转为ascii编码保存

![img](https://img-blog.csdnimg.cn/img_convert/9e33947b00f89a529ba129b5671178c8.png)



4）、点击链接切换国际化

原理：

国际化Locale（区域信息对象）；LocaleResolver（获取区域信息对象）；

springboot 自动配置的mvc，WebAutoConfiguration中默认的区域解析

```java
		@Bean
		@ConditionalOnMissingBean
		@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
		public LocaleResolver localeResolver() {
			if (this.mvcProperties
					.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
				return new FixedLocaleResolver(this.mvcProperties.getLocale());
			}
			AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
			localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
			return localeResolver;
		}
        //默认的就是根据请求头带来的区域信息获取Locale进行国际化
```

在连接上携带区域信息

```html
<!-- 没用thymeleaf之前 -->
	<a class="btn btn-sm" th:href="/index.html?l=zh_CN">中文</a>
	<a class="btn btn-sm" th:href="/index.html?l=en_US">English</a>
<!-- 用了thymeleaf之后 -->
	<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
	<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
```

自定义区域解析器

```java
package com.atguigu.springboot04web.component;
 
import org.springframework.web.servlet.LocaleResolver;
import org.thymeleaf.util.StringUtils;
 
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;
 
//在连接上携带区域信息
public class MyLocaleResolver implements LocaleResolver {
 
    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {
 
    }
 
    @Override  //解析区域信息
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        String l = httpServletRequest.getParameter("l");
        //Locale locale = Locale.getDefault();
        String acceptLanguage = httpServletRequest.getHeader("Accept-Language");
        Locale locale = new Locale(acceptLanguage);
        if (!StringUtils.isEmpty(l)) {
            String[] split = l.split("_");
            locale = new Locale(split[0], split[1]); //第一个参数 语言 第二个参数 国家
        }
        return locale;
    }
}
```

加到视图容器中，被spring管理

```java
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter{
	
	//...... 这里省略 之前视图控制器的配置
	
    @Bean
    public LocaleResolver localeResolver() {
        return new MyLocaleResolver();
    }
}
```



### P36 web开发-【实验】-登陆&拦截器

#### 1.登录

##### 1）登录页login.html

```html
<form class="form-signin" action="dashboard.html" th:action="@{user/login}", method="post">
	<!-- 登陆错误消息的显示 判断errorMsg是否为空 -->
    <p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p> <!-- 错误消息框 -->
    <input type="text" name="username" class="form-control" placeholder="Username" th:placeholder="#{login.username}" required="" autofocus="">
	<input type="password" name = "password" class="form-control" placeholder="Password" th:placeholder="#{login.password}" required="">
 
```

登录消息提示

```html
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

##### 2）LoginController

```java
@Controller
public class LoginController {
 
    @PostMapping(value = "/user/login")
    //@RequestMapping(value = "/user/login",method = RequestMethod.POST)
    public String login(@RequestParam("username") String username,
                        @RequestParam("password") String password,
                        Map<String, Object> map) {
 
        if (!StringUtils.isEmpty(username) && "123456".equals(password)) {
            //登录成功
            return "dashboard"; //请求转发到主页
        } else {
            //登录失败
            map.put("errorMsg", "用户名和密码错误");
            return "login";
        }
    }
}
```

##### 3）开发期间模板引擎页面修改以后，要实时生效

1、禁用模板引擎的缓存

```yaml
# 禁用缓存
spring.thymeleaf.cache=false 
```

2、页面修改完成以后ctrl+f9：重新编译；

3、或者使用热部署工具DevTools、JRebel

4）、登录成功后的页面，看url还是在login页面，是转发到dashboard页面，这时刷新页面就会提示表单重复提交，防止重复提交，可以使用重定向

![img](https://img-blog.csdnimg.cn/20190818081848955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

##### 5）重定向--添加视图映射

```java
//重定向
if (!StringUtils.isEmpty(username) && "123456".equals(password)) {
    //登录成功 防止表单重复提交，
    return "redirect:/main.html";

```

```java
@Bean
public WebMvcConfigurerAdapter webMvcConfigurerAdapter(){
    WebMvcConfigurerAdapter adapter = new WebMvcConfigurerAdapter(){}
    	public void addViewCOntrollers(ViewControllerRegistry registry){
			//... 其他视图映射这里省略
            //添加视图映射
			registry.addViewController("/main.html").setViewName("dashboard");
        }
}
```

#### 2.拦截器进行登陆检查

登录状态拦截器，检查登录状态，没有登录的用户，不允许访问主页和增删改查页

##### **1）添加拦截器**

```java
 
/**
 * 登陆检查，
 */
public class LoginHandlerInterceptor implements HandlerInterceptor {
    //目标方法执行之前
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object user = request.getSession().getAttribute("loginUser");
        if(user == null){
            //未登陆，返回登陆页面
            request.setAttribute("msg","没有权限，请先登陆");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else{
            //已登陆，放行请求
            return true;
        } 
    }
 
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
 
    }
 
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
 
    }
}
```

##### **2）注册拦截器**

```java
 //所有的WebMvcConfigurerAdapter组件都会一起起作用
    @Bean //将组件注册在容器
    public WebMvcConfigurerAdapter webMvcConfigurerAdapter(){
        WebMvcConfigurerAdapter adapter = new WebMvcConfigurerAdapter() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/index.html").setViewName("login");
                registry.addViewController("/main.html").setViewName("dashboard");
            }
 
            //注册拦截器 ******************************
            @Override
            public void addInterceptors(InterceptorRegistry registry) {
                //super.addInterceptors(registry);
                //静态资源；  *.css , *.js ，html
                //SpringBoot已经做好了静态资源映射  这里不需要处理静态资源
                registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**")  //拦截请求
                        .excludePathPatterns("/index.html","/","/user/login");	//排除请求
            }
        };
        return adapter;
    }
```

左上角显示登录用户

```html
Company name 改为  
[[session.loginUser]]  # 行内写法
```



### P37 web开发-【实验】-Restful实验要求

实验要求：

1）、RestfulCRUD：CRUD满足Rest风格；

URI： /资源名称/资源标识 HTTP请求方式区分对资源CRUD操作

|      | 普通CRUD（uri来区分操作） | Restful-CRUD      |
| :--- | :------------------------ | :---------------- |
| 查询 | getEmp                    | emp---GET         |
| 添加 | addEmp?xxx                | emp---POST        |
| 修改 | updateEmp?id=xxx&xxx=xx   | emp/{id}---PUT    |
| 删除 | deleteEmp?id=1            | emp/{id}---DELETE |

2）、实验的请求架构;

| 实验功能                             | 请求URI | 请求方式 |
| :----------------------------------- | :------ | :------- |
| 查询所有员工                         | emps    | GET      |
| 查询某个员工(来到修改页面)           | emp/1   | GET      |
| 来到添加页面                         | emp     | GET      |
| 添加员工                             | emp     | POST     |
| 来到修改页面（查出员工进行信息回显） | emp/1   | GET      |
| 修改员工                             | emp     | PUT      |
| 删除员工                             | emp/1   | DELETE   |




### P38 web开发-【实验】-员工列表-公共页抽取

```html
<a class="nav-link" href="#" th:href="@{/emps}"> 员工管理</a>
```

##### 3) 员工列表页

```java
@Controller
public class EmployeeController {
 
    @Autowired
    EmployeeDao employeeDao;
 
    //查询所有员工，返回员工列表页面
    //thymeleaf默认就会拼接字符串
    //classpath：/templates/xxx.html    xxx=emp/list
    @GetMapping(value = "/emps")
    public String list(Map<String, Object> map) {
        Collection<Employee> employees = employeeDao.getAll();
        map.put("emps", employees);
        return "emp/list";
    }
}
```

##### 4） thymeleaf公共页面元素抽取

--将侧边栏和上边栏公共部分抽取出来

```html
1、抽取公共片段
<div th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</div>
 
2、引入公共片段
<div th:insert="~{footer :: copy}"></div>
~{templatename::selector}：模板名::选择器
~{templatename::fragmentname}:模板名::片段名
模板名 会使用thymeleaf的前后缀进行解析

3、默认效果：
insert的公共片段在div标签中，如果不想多一层外层div 可以使用th:replace
如果使用th:insert等属性进行引入，可以不用写~{}：
行内写法可以加上：[[~{}]];[(~{})]；

```

三种引入公共片段的th属性：

**th:insert**：将公共片段整个插入到声明引入的元素中

**th:replace**：将声明引入的元素替换为公共片段

**th:include**：将被引入的片段的内容包含进这个标签中

三种引入方式的区别：

```html
<footer th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</footer>
 
引入方式
<div th:insert="footer :: copy"></div>
<div th:replace="footer :: copy"></div>
<div th:include="footer :: copy"></div>
 
效果
<div>
    <footer>
    &copy; 2011 The Good Thymes Virtual Grocery
    </footer>
</div>
 
<footer>
&copy; 2011 The Good Thymes Virtual Grocery
</footer>
 
<div>
&copy; 2011 The Good Thymes Virtual Grocery
</div>
```

两种方式实现引入

```html
<nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0" th:fragment="topbar">
<nav class="col-md-2 d-none d-md-block bg-light sidebar" id="sideBar">
 
引入：
<!--  引入topbar -->
<div th:replace="dashboard :: topbar"></div>
 
<!-- 引入侧边栏 -->
<div th:replace="dashboard::#sideBar"></div>
 
```

![img](https://img-blog.csdnimg.cn/20190819072535252.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

### P39 web开发-【实验】-员工列表-链接高亮&列表完成

#### 1.链接高亮

引入片段的时候，传入参数（官方文档，参数化的片段签名）

```html
 <a class="nav-link active" href="#" th:href="@{/main.html}">
 <a class="nav-link active" th:href="@{/emps}">
```



#### 2.高亮某一个选中显示的页面

1）、把sideBar和topBar公共部分抽取成一个bar.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>bar</title>
</head>
<body>
<!-- topBar -->
<nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0" th:fragment="topbar">
    <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">[[${session.loginUser}]]</a>
    <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
    <ul class="navbar-nav px-3">
        <li class="nav-item text-nowrap">
            <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">Sign out</a>
        </li>
    </ul>
</nav>
 
<!-- sidebar -->
<nav class="col-md-2 d-none d-md-block bg-light sidebar" id="sideBar">
    <div class="sidebar-sticky">
        <ul class="nav flex-column">
            <li class="nav-item">
                <a class="nav-link active"
                   href="#" th:href="@{/main.html}">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home">
                        <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
                        <polyline points="9 22 9 12 15 12 15 22"></polyline>
                    </svg>
                    Dashboard <span class="sr-only">(current)</span>
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file">
                        <path d="M13 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"></path>
                        <polyline points="13 2 13 9 20 9"></polyline>
                    </svg>
                    Orders
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link active" th:href="@{/emps}" href="#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-users">
                        <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                        <circle cx="9" cy="7" r="4"></circle>
                        <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                        <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                    </svg>
                    员工管理
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-bar-chart-2">
                        <line x1="18" y1="20" x2="18" y2="10"></line>
                        <line x1="12" y1="20" x2="12" y2="4"></line>
                        <line x1="6" y1="20" x2="6" y2="14"></line>
                    </svg>
                    Reports
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-layers">
                        <polygon points="12 2 2 7 12 12 22 7 12 2"></polygon>
                        <polyline points="2 17 12 22 22 17"></polyline>
                        <polyline points="2 12 12 17 22 12"></polyline>
                    </svg>
                    Integrations
                </a>
            </li>
        </ul>
 
        <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
            <span>Saved reports</span>
            <a class="d-flex align-items-center text-muted" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-plus-circle"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
            </a>
        </h6>
        <ul class="nav flex-column mb-2">
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                        <polyline points="14 2 14 8 20 8"></polyline>
                        <line x1="16" y1="13" x2="8" y2="13"></line>
                        <line x1="16" y1="17" x2="8" y2="17"></line>
                        <polyline points="10 9 9 9 8 9"></polyline>
                    </svg>
                    Current month
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                        <polyline points="14 2 14 8 20 8"></polyline>
                        <line x1="16" y1="13" x2="8" y2="13"></line>
                        <line x1="16" y1="17" x2="8" y2="17"></line>
                        <polyline points="10 9 9 9 8 9"></polyline>
                    </svg>
                    Last quarter
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                        <polyline points="14 2 14 8 20 8"></polyline>
                        <line x1="16" y1="13" x2="8" y2="13"></line>
                        <line x1="16" y1="17" x2="8" y2="17"></line>
                        <polyline points="10 9 9 9 8 9"></polyline>
                    </svg>
                    Social engagement
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                        <polyline points="14 2 14 8 20 8"></polyline>
                        <line x1="16" y1="13" x2="8" y2="13"></line>
                        <line x1="16" y1="17" x2="8" y2="17"></line>
                        <polyline points="10 9 9 9 8 9"></polyline>
                    </svg>
                    Year-end sale
                </a>
            </li>
        </ul>
    </div>
</nav>
  
</body>
</html>
```

2）、利用三重条件判断来判断当前时哪个页面并且高亮

```html
  <a class="nav-link active" href="#" th:href="@{/main.html}"
     th:class="${activeUri=='main.html'?'nav-link active':'nav-link'}">
 
  <a class="nav-link active" th:href="@{/emps}"
     href="#" th:class="${activeUri=='emps'?'nav-link active':'nav-link'}">
```

3)、Dashboard页面  变量如何传入

```html
	<!-- 引入sidebar -->
	<div th:replace="commons/bar::#sideBar(activeUri='main.html')"></div>
```

4）、list页面  变量如何传入

```html
<!-- 引入侧边栏 -->
<div th:replace="commons/bar::#sideBar(activeUri='emps')"></div>
```

#### 3.遍历取出员工数据

现状是现在的员工列表时写死在html中的

![img](https://img-blog.csdnimg.cn/20190822073131341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

现在要实时取出数据

```html
<thead>
	<tr>
	  <th>id</th>
	  <th>lastName</th>
	  <th>email</th>
	  <th>gender</th>
	  <th>department</th>
	  <th>birth</th>
	 </tr>
</thead>
<tbody>
	<tr th:each="emp:${emps}">
		<td th:text="${emp.id}"></td>
		<td>[[${emp.lastName}]]</td>  <!-- 行内写法 -->
		<td th:text="${emp.email}"></td>
		<td th:text="${emp.gender}==0?'女':'男'"></td>
		<td th:text="${emp.department.departmentName}"></td>
		<td th:text="${emp.birth}"></td>
        <td>
            <button class="btn btn-sm btn-primary">编辑</button>
            <button class="btn btn-sm btn-danger">删除</button>
        </td>
	</tr>
</tbody>
```

暂时不访问数据库，从内存中构造数据

```java
	static{
		employees = new HashMap<Integer, Employee>();
 
		employees.put(1001, new Employee(1001, "E-AA", "aa@163.com", 1, new Department(101, "D-AA")));
		employees.put(1002, new Employee(1002, "E-BB", "bb@163.com", 1, new Department(102, "D-BB")));
		employees.put(1003, new Employee(1003, "E-CC", "cc@163.com", 0, new Department(103, "D-CC")));
		employees.put(1004, new Employee(1004, "E-DD", "dd@163.com", 0, new Department(104, "D-DD")));
		employees.put(1005, new Employee(1005, "E-EE", "ee@163.com", 1, new Department(105, "D-EE")));
	}
```

![img](https://img-blog.csdnimg.cn/2019082207410082.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

#### 4.日期格式化

```
${#dates.fromate(emp.birth, 'yyyy-MM-dd HH:mm')}
```

![img](https://img-blog.csdnimg.cn/20190822074219830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

#### 5.添加，编辑和删除按钮

```html
<h2>员工列表 <button class="btn btn-sm btn-success">添加</button></h2>
 
<td>
	<button class="btn btn-sm btn-primary">编辑</button>
	<button class="btn btn-sm btn-danger">删除</button>
</td>
```

![img](https://img-blog.csdnimg.cn/20190822075107824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

### P40 web开发-【实验】-员工添加-来到添加页面

点击“员工添加”按钮来到员工添加页面，页面应该是form表单形式，添加员工信息，然后点击“保存”按钮

#### 0. 列表页，添加按钮 增加href

```html
<h2>员工列表 <a class="btn btn-sm btn-success" href="emp" th:href="@{/emp}">添加</a></h2>
```



#### 1.“员工添加”按钮：/emp，get请求

```java
    //来到员工添加页面
    @GetMapping(value = "/emp")
    public String getAddEmp(Model model){
        //查出所有部门
        Collection<Department> departments = departmentDao.getDepartments();
        model.addAttribute("depts", departments);
        return "emp/add";
    }
```



```html
<form>
    <div class="form-group">
        <label>LastName</label>
        <input type="text" class="form-control" placeholder="zhangsan">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input type="email" class="form-control" placeholder="zhangsan@atguigu.com">
    </div>
    <div class="form-group">
        <label>Gender</label><br/>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender"  value="1">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender"  value="0">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <select class="form-control">
	        <option th th:value="${dept.id}" th:each="dept:${depts}">[[${dept.departmentName}]]</option>  
        </select>
    </div>
    <div class="form-group">
        <label>Birth</label>
        <input type="text" class="form-control" placeholder="zhangsan">
    </div>
    <button type="submit" class="btn btn-primary">添加</button>
</form>
```

### P41 web开发-【实验】-员工添加-添加完成

#### 2.“保存”按钮：/emp，post请求

```java
 //添加员工
    //SpringMvc自动将请求参数和入参对象的属性进行一一绑定，
    // 要求请求参数的名字和javaBean入参的对象的属性名保持一致
    @PostMapping("/emp")
    public String addEmp(Employee employee){
 
        System.out.println("保存的员工信息"+employee);
        employeeDao.save(employee);
        //redirect:表示重定向到一个地址，/代表当前项目路径
        //forward：表示转发到一个地址
        //直接return "/emp"; thymeleaf默认就会拼接字符串 使用模板引擎 找/templates/emps.html 并不能把req数据放入页面
        //原理可以查看ThymeleafView具体源码 了解thymeleaf是如何解析视图的
	    //classpath：/templates/xxx.html    xxx=emp/list
        return "redirect:/emps";
    }
```

#### 3.解决遇到的问题

##### 3.1 日期格问题

添加信息后报400错误，查看日志报错

```
“Field error in object 'employee' on field 'birth': rejected value [2019-08-23]; codes [typeMismatch.employee.birth,typeMismatch.birth,typeMismatch.java.util.Date,typeMismatch]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [employee.birth,birth]; arguments []; default message [birth]]; default message [Failed to convert property value of type 'java.lang.String' to required type 'java.util.Date' for property 'birth'; nested exception is org.springframework.core.convert.ConversionFailedException: Failed to convert from type [java.lang.String] to type [java.util.Date] for value '2019-08-23'; nested exception is java.lang.IllegalArgumentException]]”
```

添加页面最容易遇到的问题：

提交的数据格式不对：特别是日期、时间格式等；不支持2019-08-23这种格式，只支持2017/12/12这种格式吗

日期的格式化；SpringMVC将页面提交的值需要转换为指定的类型；涉及类型转换和日期格式

默认日期是按照/的方式；

在application.properties配置文件中配置数据格式方式

```properties
spring.mvc.date-format=yyyy-MM-dd
```

查看源码： 

```java
WebMvcAutoConfiguration.java
 
@Bean
@ConditionalOnProperty(
    prefix = "spring.mvc",
    name = {"date-format"}
)
public Formatter<Date> dateFormatter() {
   return new DateFormatter(this.mvcProperties.getDateFormat());
}
 
 
WebMvcProperties.java
//默认格式 yyyy/MM/dd
public String getDateFormat() {
   return this.dateFormat;
}
```

![img](https://img-blog.csdnimg.cn/20190824081857330.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190824081923240.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

##### 3.2 数据未保存成功问题

![img](https://img-blog.csdnimg.cn/20190824072013966.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)



在添加员工信息的时候之后，点击添加没有成功跳转到员工列表页，员工信息也没有添加成功

![img](https://img-blog.csdnimg.cn/2019082407223472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

看标题像是请求了一个get方法，检查代码也没有检查出来问题，debug发现是添加信息之后的后续操作action没有添加，默认是get方法，应为post提交

```html
<form>
	<div class="form-group">
		<label>LastName</label>
		<input name="lastName" type="text" class="form-control" placeholder="zhangsan">
	</div>
	<div class="form-group">
		<label>Email</label>
 
修改成.....
 
<form th:action="@{/emp}"  method ="post">
	<div class="form-group">
		<label>LastName</label>
		<input name="lastName" type="text" class="form-control" placeholder="zhangsan">
	</div>
```



### P42 web开发-【实验】-员工修改-重用页面&修改完成

点击某个员工的#编辑#按钮，进入修改员工页面（/emp/{id}，get），修改员工信息，然后点击#保存#按钮(返回到员工列表页，put)

##### 列表页list.html  修改编辑按钮 添加链接

```html
<button class="btn btn-sm btn-primary">编辑</button>
改为	
<a class="btn btn-sm btn-primary" th:href="@{/epm/}+${emp.id}">编辑</a>
```

##### 修改员工信息页

修改添加二合一表单 add.html

```html
<!--需要区分是员工修改还是添加；-->
<form th:action="@{/emp}" method="post">
    <!--发送put请求修改员工数据-->
    <!--
1、SpringMVC中配置HiddenHttpMethodFilter;（SpringBoot自动配置好的）
2、页面创建一个post表单
3、创建一个input项，name="_method";值就是我们指定的请求方式
-->
    <input type="hidden" name="_method" value="put" th:if="${emp!=null}"/>
    <input type="hidden" name="id" th:if="${emp!=null}" th:value="${emp.id}">
    <div class="form-group">
        <label>LastName</label>
        <input name="lastName" type="text" class="form-control" placeholder="zhangsan" th:value="${emp!=null}?${emp.lastName}">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input name="email" type="email" class="form-control" placeholder="zhangsan@atguigu.com" th:value="${emp!=null}?${emp.email}">
    </div>
    <div class="form-group">
        <label>Gender</label><br/>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="1" th:checked="${emp!=null}?${emp.gender==1}">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="0" th:checked="${emp!=null}?${emp.gender==0}">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <!--提交的是部门的id-->
        <select class="form-control" name="department.id">
            <option th:selected="${emp!=null}?${dept.id == emp.department.id}" th:value="${dept.id}" th:each="dept:${depts}" th:text="${dept.departmentName}">1</option>
        </select>
    </div>
    <div class="form-group">
        <label>Birth</label>
        <input name="birth" type="text" class="form-control" placeholder="zhangsan" th:value="${emp!=null}?${#dates.format(emp.birth, 'yyyy-MM-dd HH:mm')}">
    </div>
    <button type="submit" class="btn btn-primary" th:text="${emp!=null}?'修改':'添加'">添加</button>
</form>
```

##### 查询员工信息Controller

```java
 
    //来到修改页面，查出当前员工，在页面回显
    @GetMapping("/emp/{id}")
    public String toEditPage(@PathVariable("id") Integer id, Model model) {
        Employee employee = employeeDao.get(id);
        model.addAttribute("emp", employee);
 
        //页面要显示所有的部门列表
        Collection<Department> departments = departmentDao.getDepartments();
        model.addAttribute("depts", departments);
        //回到修改页面（add是一个修改添加二合一页面）
        return "/emp/add";
    }
 
    //员工修改,并保存员工信息
    @PutMapping("/emp")  //处理put请求
    public String updateEmployee (Employee employee){
        System.out.println("修改的员工数据，"+employee);
        employeeDao.save(employee);
        return "redirect:/emps";
    }
```

##### 修改页面表单为put请求

表单如何发送put请求

```
发送put请求修改员工数据
    
1、SpringMVC中配置HiddenHttpMethodFilter;（SpringBoot自动配置好的） 将请求转为指定的方式发送
2、页面创建一个post表单
3、创建一个input项，name="_method";值就是我们指定的请求方式
```

```html
<input type="hidden" name="_method" value="put" th:if="${emp!=null}"/>
```

```html
value="put" put 不区分大小写
```

##### 修改时提交员工id

```html
<input type="hidden" name="id" th:if="${emp!=null}" th:value="${emp.id}">
```

![img](https://img-blog.csdnimg.cn/20190825163752890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190825163832895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)



### P43 web开发-【实验】-员工删除-删除完成

#### 删除员工

##### 列表删除按钮

点击某个员工的#删除#按钮，发送delete请求，删除员工，然后返回到员工列表页面

```html
<td>
    <a class="btn btn-sm btn-primary" th:href="@{/epm/}+${emp.id}">编辑</a>
    <form th:action="@{/emp}+${emp.id}" method="post">
        <input type="hidden" naem="_method" value="delete" />  <!--发送delete请求-->
        <button type="submit" class="btn btn-sm btn-danger">删除</button> <!--提交表单-->
    </form>
</td>
```

每一个按钮 都要写一个表单，这样太麻烦，并且，表单会积压按钮换行

##### 改进

1- 将表单移动到表格之外

2-使用js方式触发表单提交

```html
 	<body>
		<!--  引入topbar -->
		<div th:replace="commons/bar::topbar"></div>
 
		<div class="container-fluid">
			<div class="row">/
				<!-- 引入侧边栏 -->
				<div th:replace="commons/bar::#sideBar(activeUri='emps')"></div>
 
				<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
					<h2>员工列表 <a class="btn btn-sm btn-success" href="/emp" th:href="@{/emp}">添加</a></h2>
					<div class="table-responsive">
						<table class="table table-striped table-sm">
							<thead>
								<tr>
									<th>id</th>
									<th>lastName</th>
									<th>email</th>
									<th>gender</th>
									<th>department</th>
									<th>birth</th>
								</tr>
							</thead>
							<tbody>
								<tr th:each="emp:${emps}">
									<td th:text="${emp.id}"></td>
									<td>[[${emp.lastName}]]</td>
									<td th:text="${emp.email}"></td>
									<td th:text="${emp.gender}==0?'女':'男'"></td>
									<td th:text="${emp.department.departmentName}"></td>
									<td th:text="${#dates.format(emp.birth,'yyyy-MM-dd HH:mm')}"></td>
									<td>
										<a class="btn btn-sm btn-primary" th:href="@{/emp/}+${emp.id}">编辑</a>
										<button th:attr="deleteUri = @{/emp/}+${emp.id}" class="btn btn-sm btn-danger deletebtn">删除</button>
 
									</td>
								</tr>
 
							</tbody>
						</table>
					</div>
				</main>
                <!-- 将表单移动到这里 防止页面变形 ———————————————————————————————————————————————————————— -->
				<form id="deleteEmpForm" method="post">
					<input type="hidden" name="_method" value="delete"/>
				</form>
			</div>
		</div>
 
		<!-- Bootstrap core JavaScript
    ================================================== -->
		<!-- Placed at the end of the document so the pages load faster -->
		<script type="text/javascript" src="asserts/js/jquery-3.2.1.slim.min.js"></script>
		<script type="text/javascript" src="asserts/js/popper.min.js"></script>
		<script type="text/javascript" src="asserts/js/bootstrap.min.js"></script>
 
		<!-- Icons -->
		<script type="text/javascript" src="asserts/js/feather.min.js"></script>
		<script>
			feather.replace()
		</script>
 
		<!-- Graphs -->
		<script type="text/javascript" src="asserts/js/Chart.min.js"></script>
		<script>
			var ctx = document.getElementById("myChart");
			var myChart = new Chart(ctx, {
				type: 'line',
				data: {
					labels: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
					datasets: [{
						data: [15339, 21345, 18483, 24003, 23489, 24092, 12034],
						lineTension: 0,
						backgroundColor: 'transparent',
						borderColor: '#007bff',
						borderWidth: 4,
						pointBackgroundColor: '#007bff'
					}]
				},
				options: {
					scales: {
						yAxes: [{
							ticks: {
								beginAtZero: false
							}
						}]
					},
					legend: {
						display: false,
					}
				}
			});
		</script>
	<script>
		$(".deletebtn").click(function () {
			//删除当前员工
			$("#deleteEmpForm").attr("action",$(this).attr("deleteUri")).submit();
			return false;
		})
	</script> 
	</body> 
</html>
```



##### 删除员工的controller

```java
   //员工删除
    @DeleteMapping("/emp/{id}")
    public String deleteEmp(@PathVariable("id") Integer id){
        employeeDao.delete(id);
        return "redirect:/emps";
    }
```

![img](https://img-blog.csdnimg.cn/201908260717304.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190826071800804.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

### P44 web开发-错误处理原理&定制错误页面

#### 7. 错误处理原理

#### 7.1 SpringBoot默认的错误处理机制

##### 1、**错误处理默认效果：**

1）浏览器访问，返回一个默认的错误页面

![img](https://img-blog.csdnimg.cn/20190827065844321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

 浏览器发送请求的请求头：

![img](https://img-blog.csdnimg.cn/20190827065914179.png)

使用postman模拟发送客户端请求

2）如果是其他客户端，默认响应一个json数据

![img](https://img-blog.csdnimg.cn/20190827065949553.png)

​    客户端发送请求的请求头：

![img](https://img-blog.csdnimg.cn/20190827070022770.png)



##### 2、**错误处理原理：**

可以参照ErrorMvcAutoConfiguration；错误处理的自动配置；

给容器中添加了以下组件

**1）、ErrorPageCustomizer：**

```java
	@Value("${error.path:/error}")
	private String path = "/error";  //出现错误以后来到error请求进行处理；（相当于spring时期 web.xml注册的错误页面规则）
```

**2）、BasicErrorController：处理默认/error请求**

```java
@Controller
//使用顺续“server.error.path;error.path;/error
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {
    
    @RequestMapping(produces = "text/html")//产生html类型的数据；浏览器发送的请求来到这个方法处理
	public ModelAndView errorHtml(HttpServletRequest request,
			HttpServletResponse response) {
		HttpStatus status = getStatus(request);
		Map<String, Object> model = Collections.unmodifiableMap(getErrorAttributes(
				request, isIncludeStackTrace(request, MediaType.TEXT_HTML)));
		response.setStatus(status.value());
        
        //去哪个页面作为错误页面；包含页面地址和页面内容
		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
		return (modelAndView == null ? new ModelAndView("error", model) : modelAndView);
	}
 
	@RequestMapping
	@ResponseBody    //产生json数据，其他客户端来到这个方法处理；
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
		Map<String, Object> body = getErrorAttributes(request,
				isIncludeStackTrace(request, MediaType.ALL));
		HttpStatus status = getStatus(request);
		return new ResponseEntity<Map<String, Object>>(body, status);
	}
```

**3）、DefaultErrorViewResolver：**

```java
@Override
	public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status,
			Map<String, Object> model) {
		ModelAndView modelAndView = resolve(String.valueOf(status), model);
		if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
			modelAndView = resolve(SERIES_VIEWS.get(status.series()), model);
		}
		return modelAndView;
	}
 
	private ModelAndView resolve(String viewName, Map<String, Object> model) {
        //默认SpringBoot可以去找到一个页面？  error/404
		String errorViewName = "error/" + viewName;
        
        //模板引擎可以解析这个页面地址就用模板引擎解析
		TemplateAvailabilityProvider provider = this.templateAvailabilityProviders
				.getProvider(errorViewName, this.applicationContext);
		if (provider != null) {
            //模板引擎可用的情况下返回到errorViewName指定的视图地址
			return new ModelAndView(errorViewName, model);
		}
        //模板引擎不可用，就在静态资源文件夹下找errorViewName对应的页面   error/404.html
		return resolveResource(errorViewName, model);
	}
```

**4）、DefaultErrorAttributes：**

​	作用：帮我们在页面共享信息；

```java
帮我们在页面共享信息；
@Override
	public Map<String, Object> getErrorAttributes(RequestAttributes requestAttributes,
			boolean includeStackTrace) {
		Map<String, Object> errorAttributes = new LinkedHashMap<String, Object>();
		errorAttributes.put("timestamp", new Date());
		addStatus(errorAttributes, requestAttributes);
		addErrorDetails(errorAttributes, requestAttributes, includeStackTrace);
		addPath(errorAttributes, requestAttributes);
		return errorAttributes;
	}
```



##### 3、**错误处理步骤：**

一但系统出现4xx或者5xx之类的错误；ErrorPageCustomizer就会生效（定制错误的响应规则）；就会来到/error请求；就会被**BasicErrorController**处理；

1）响应页面；去哪个页面是由**DefaultErrorViewResolver**解析得到的；

```java
protected ModelAndView resolveErrorView(HttpServletRequest request,
      HttpServletResponse response, HttpStatus status, Map<String, Object> model) {
    //所有的ErrorViewResolver得到ModelAndView
   for (ErrorViewResolver resolver : this.errorViewResolvers) {
      ModelAndView modelAndView = resolver.resolveErrorView(request, status, model);
      if (modelAndView != null) {
         return modelAndView;
      }
   }
   return null;
}
```



#### 7.2 如何定制错误响应：

##### 1、如何定制错误的页面；

**1）、有模板引擎的情况下；error/状态码;** 【将错误页面命名为 错误状态码.html 放在模板引擎文件夹里面的 **error文件夹下**】，发生此状态码的错误就会来到 对应的页面；

我们可以使用4xx和5xx作为错误页面的文件名来匹配这种类型的所有错误，精确优先（优先寻找精确的状态码.html）；

页面能获取的信息，（从**DefaultErrorAttributes**中的getErrorAttributes获取信息）：

​		timestamp：时间戳

​		status：状态码

​		error：错误提示

​		exception：异常对象

​		message：异常消息

​		errors：JSR303数据校验的错误都在这里

![img](https://img-blog.csdnimg.cn/20190828070339367.png)（优先寻找404.html）

![img](https://img-blog.csdnimg.cn/20190828070237314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190828070303232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

404.html  页面中添加状态码、时间戳：

```html
<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
	<h1>status:[[${status}]]</h1>
	<h2>timestamp:[[${timestamp}]]</h2>
    <h2>error:[[${error}]]</h2>
</main>
```

![img](https://img-blog.csdnimg.cn/20190828071232549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

**2）、没有模板引擎**（模板引擎找不到这个错误页面），静态资源文件夹下找；

![img](https://img-blog.csdnimg.cn/20190828071503698.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

**3）、没有模板引擎也没有错误码html文件时** 以上都没有错误页面，就是默认来到SpringBoot默认的错误提示页面；

```java
 public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
        HttpStatus status = this.getStatus(request);
        Map<String, Object> model = Collections.unmodifiableMap(this.getErrorAttributes(request, this.isIncludeStackTrace(request, MediaType.TEXT_HTML)));
        response.setStatus(status.value());
        ModelAndView modelAndView = this.resolveErrorView(request, response, status, model);
//modelAndView为null的时候返回new ModelAndView("error", model)，error在ErrorMvcAutoConfiguration中的render定义了默认的返回格式
        return modelAndView != null ? modelAndView : new ModelAndView("error", model);
    }
 
  
  @Bean( name = {"error"})
  @ConditionalOnMissingBean(name = {"error"})
  public void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
 
      builder.append("<html><body><h1>Whitelabel Error Page</h1>").append("<p>This application has no explicit mapping for /error, so you are seeing this as a fallback.</p>").append("<div id='created'>").append(timestamp).append("</div>").append("<div>There was an unexpected error (type=").append(this.htmlEscape(model.get("error"))).append(", status=").append(this.htmlEscape(model.get("status"))).append(").</div>");
                if (message != null) {
                    builder.append("<div>").append(this.htmlEscape(message)).append("</div>");
                }
 
                if (trace != null) {
                    builder.append("<div style='white-space:pre-wrap;'>").append(this.htmlEscape(trace)).append("</div>");
                }
 
                builder.append("</body></html>");
                response.getWriter().append(builder.toString());
            }
```

##### 2、如何定制错误的json数据响应

### P45 web开发-定制错误数据

##### 定制错误页面总结

在模板引擎文件夹下或静态资源文件夹下，放置一个error文件夹、在这里面存放错误状态码对应的页面（可以每个错误码对应一个页面，也可以使用4xx.html 5xx.html 存放一类错误页面），在页面中还可以取出相关错误信息

浏览器访问返回一个页面，postman或客户端访问返回json数据

##### 自定义异常

```java
    @ResponseBody
    @RequestMapping("/hello")
    public String hello(@RequestParam("user") String user) {
        if (user.equals("aaa")) {
            throw new UserNotExistException();
        }
        return "Hello World";
    }
 
//自定义异常类
public class UserNotExistException extends RuntimeException{
    public UserNotExistException(){
        super("用户不存在");
    }
}
```

![img](https://img-blog.csdnimg.cn/20190829071606396.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)



客户端访问时

![img](https://img-blog.csdnimg.cn/20190829071725866.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

#### 如何定制错误的返回的JSON数据

##### 1、通过定义异常处理器，返回自定义异常json数据

```java
@ControllerAdvice
public class MyExceptionHandler {
 
    //浏览器和客户端返回的都是json
    @ResponseBody
    @ExceptionHandler(UserNotExistException.class)
    public Map<String,Object> handleException(Exception e){
        Map<String,Object> map = new HashMap<>();
        map.put("code","user.notexist");
        map.put("message",e.getMessage());
        return map;
    }
}
//没有自适应效果... 无论是浏览器还是客户端都返回json数据，浏览器不能返回错误页面
```

![img](https://img-blog.csdnimg.cn/20190829072123603.png)

##### 2、通过转发到/error，进行自适应响应效果处理

```java
    @ExceptionHandler(UserNotExistException.class)
    public String handleException(Exception e, HttpServletRequest request){
        Map<String,Object> map = new HashMap<>();
        //传入我们自己的错误状态码  4xx 5xx，否则就不会进入定制错误页面的解析流程
        /**
         * Integer statusCode = (Integer) request
         .getAttribute("javax.servlet.error.status_code");
         */
        request.setAttribute("javax.servlet.error.status_code",500);
        map.put("code","user.notexist");
        map.put("message",e.getMessage());
        //转发到/error
        return "forward:/error";
    }
```

![img](https://img-blog.csdnimg.cn/20190829072659228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

##### 3、转发到/error进行自适应响应效果处理

出现错误以后，会来到/error请求，会被BassicController处理，响应出去的请求可以获取的数据是由GetErrorAttributes得到的（是AbstractErrorController规定的方法）；

完全编写一个ErrorController的实现类【或者是编写AbstractErrorController的子类】，放入容器中；

```java
@ExceptionHandler(UserNotExistException.class)
    public String handleException(Exception e, HttpServletRequest request){
        Map<String,Object> map = new HashMap<>();
        //传入我们自己的错误状态码  4xx 5xx，否则就不会进入定制错误页面的解析流程
        /**
         * Integer statusCode = (Integer) request
         .getAttribute("javax.servlet.error.status_code");
         */
        request.setAttribute("javax.servlet.error.status_code",500);
        map.put("code","user.notexist");
        map.put("message",e.getMessage());
        //转发到/error
        return "forward:/error";
    }
```

![img](https://img-blog.csdnimg.cn/20190829072659228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

##### 4、将我们的定制数据携带出去；

出现错误以后，会来到/error请求，会被BasicErrorController处理，响应出去可以获取的数据是由getErrorAttributes得到的（是AbstractErrorController（ErrorController）规定的方法）；

1、完全来编写一个ErrorController的实现类【或者是编写AbstractErrorController的子类】，放在容器中；

2、页面上能用的数据，或者是json返回能用的数据都是通过errorAttributes.getErrorAttributes得到；

容器中DefaultErrorAttributes.getErrorAttributes()；默认进行数据处理的；

自定义ErrorAttributes

```java
//给容器中加入我们自己定义的ErrorAttributes
@Component
public class MyErrorAttributes extends DefaultErrorAttributes {
 
     @Override
    public Map<String, Object> getErrorAttributes(WebRequest webRequest, boolean includeStackTrace) {
        Map<String, Object> map = super.getErrorAttributes(webRequest, includeStackTrace);
        map.put("company", "atguigu");
        //我们的异常处理器携带的数据
        Map<String, Object> map1 = (Map<String, Object>) webRequest.getAttribute("ext", 0);
        map.put("ext", map1);
        return map;
    }
}
```

最终的效果：响应是自适应的，可以通过定制ErrorAttributes改变需要返回的内容，

![img](https://img-blog.csdnimg.cn/20190829074851992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190829074914785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

### P46 web开发-嵌入式Servlet容器配置修改

#### 四、配置嵌入式Servlet容器

SpringBoot默认使用Tomcat作为嵌入式的Servlet容器；

![img](https://img-blog.csdnimg.cn/20190830070510710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

##### 4.1、如何定制和修改Servlet容器的相关配置；

1、修改和server有关的配置（ServerProperties【也是EmbeddedServletContainerCustomizer】底层原理是一样的）；

```properties
server.port=8081
server.context-path=/crud

server.tomcat.uri-encoding=UTF-8

//通用的Servlet容器设置
server.xxx
//Tomcat的设置
server.tomcat.xxx
```

2、编写一个**EmbeddedServletContainerCustomizer**：嵌入式的Servlet容器的定制器；来修改Servlet容器的配置

```java
@Configureation
public class MyMvcConfig extends WebMvcCOnfigurerAdapter{

    @Bean
    //一定要将这个定制器加入到容器中
    public EmbeddedServletContainerCustomizer embeddedServletContainerCustomizer(){
        return new EmbeddedServletContainerCustomizer() {
            //定制嵌入式的Servlet容器相关的规则
            @Override
            public void customize(ConfigurableEmbeddedServletContainer container) {
                container.setPort(8083);
            }
        };
    }

 
     @Bean
    //一定要将这个定制器加入到容器中
    public WebServerFactoryCustomizer<ConfigurableWebServerFactory> webServerFactoryCustomizer() {
        return new WebServerFactoryCustomizer<ConfigurableWebServerFactory>() {
            //定制嵌入式的Servlet容器相关的规则
            @Override
            public void customize (ConfigurableWebServerFactory configurableWebServerFactory）{
               configurableWebServerFactory.setPort(8099);
            }
         };
    }                               
}                             
```

springBoot2.0及以上版本没有EmbeddedServletContainerCustomizer类
使用WebServerFactoryCustomizer接口替换EmbeddedServletContainerCustomizer组件完成对嵌入式Servlet容器的配置
在WebServerFactoryCustomizer接口中使用ConfigurableWebServerFactory对象实现对customize()方法的转换，从而实现对嵌入式servlet容器的配置。

![img](https://img-blog.csdnimg.cn/20190830074345627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

### P47 web开发-注册servlet三大组件

#### 4.17 注册Servlet三大组件【Servlet、Filter、Listener】

由于SpringBoot默认是以jar包的方式启动嵌入式的Servlet容器来启动SpringBoot的web应用，而不是标准的web目录结构，因此没有web.xml文件（以前注册servlet三大组件都是在该xml文件中进行的，但是现在可以通过以下方式）。

注册三大组件用以下方式

**ServletRegistrationBean**

**FilterRegistrationBean**

**ServletListenerRegistrationBean**

##### 1）、ServletRegistrationBean

MyServlet:

```java

public class MyServlet extends HttpServlet {
 
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }
 
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("hello, MyServlet");
    }
}
```

```java
    //注册三大组件Servlet
    @Bean
    public ServletRegistrationBean myServlet() {
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(new Myservlet(), "/myServlet");
        registrationBean.setLoadOnStartup(1);
        return registrationBean;
    }
```

![img](https://img-blog.csdnimg.cn/20190831195745844.png)

##### 2）、FilterRegistrationBean

```java
public class MyFilter implements Filter {
 
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("MyFilter progress ...");
        filterChain.doFilter(servletRequest,servletResponse);
    }
}
```

```java
  @Bean
    public FilterRegistrationBean myFilter() {
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
        filterRegistrationBean.setFilter(new MyFilter());
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/hello", "/myServlet"));
        return filterRegistrationBean;
    }
```

![img](https://img-blog.csdnimg.cn/20190831200835722.png)

##### 3）、ServletListenerRegistrationBean

```java
public class MyListener implements ServletContextListener {
 
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("contextInitialized ...web应用启动");
    }
 
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("contextDestroyed...当前web项目销毁");
    }
}
```

```java
 @Bean
    public ServletListenerRegistrationBean myListener(){
        ServletListenerRegistrationBean<MyListener> registrationBean = new ServletListenerRegistrationBean<>(new MyListener());
        return registrationBean;
    }
```

![img](https://img-blog.csdnimg.cn/20190831201522778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

SpringBoot帮我们自动SpringMVC的时候，自动的注册SpringMVC的前端控制器；DIspatcherServlet；

DispatcherServletAutoConfiguration中：

```java
@Bean(name = DEFAULT_DISPATCHER_SERVLET_REGISTRATION_BEAN_NAME)
@ConditionalOnBean(value = DispatcherServlet.class, name = DEFAULT_DISPATCHER_SERVLET_BEAN_NAME)
public ServletRegistrationBean dispatcherServletRegistration(
      DispatcherServlet dispatcherServlet) {
   ServletRegistrationBean registration = new ServletRegistrationBean(
         dispatcherServlet, this.serverProperties.getServletMapping());
    //默认拦截： /  所有请求；包静态资源，但是不拦截jsp请求；   /*会拦截jsp
    //可以通过server.servletPath来修改SpringMVC前端控制器默认拦截的请求路径
    
   registration.setName(DEFAULT_DISPATCHER_SERVLET_BEAN_NAME);
   registration.setLoadOnStartup(
         this.webMvcProperties.getServlet().getLoadOnStartup());
   if (this.multipartConfig != null) {
      registration.setMultipartConfig(this.multipartConfig);
   }
   return registration;
}
```

### P48 web开发-切换其他嵌入式Servlet容器

使用其他Servlet容器

Jetty（更适合开发长连接的应用，Web聊天）

Undertow（不支持JSp,高性能非阻塞的Servlet容器，并发性能非常好）



![img](https://img-blog.csdnimg.cn/2019090108181918.png)



默认支持：

Tomcat（默认使用）

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
   <!-- 引入web模块默认就是使用嵌入式的Tomcat作为Servlet容器；-->
</dependency>
```

Jetty

```xml
<!-- 引入web模块 -->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
   <exclusions>
      <exclusion>
         <artifactId>spring-boot-starter-tomcat</artifactId>
         <groupId>org.springframework.boot</groupId>
      </exclusion>
   </exclusions>
</dependency>

<!-- 引入其他的Servlet容器-jetty -->
<dependency>
   <artifactId>spring-boot-starter-jetty</artifactId>
   <groupId>org.springframework.boot</groupId>
</dependency>
```

Undertow

```xml
<!-- 引入web模块 -->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
   <exclusions>
      <exclusion>
         <artifactId>spring-boot-starter-tomcat</artifactId>
         <groupId>org.springframework.boot</groupId>
      </exclusion>
   </exclusions>
</dependency>

<!-- 引入其他的Servlet容器-undertow -->
<dependency>
   <artifactId>spring-boot-starter-undertow</artifactId>
   <groupId>org.springframework.boot</groupId>
</dependency>
```



### P49 web开发-嵌入式Servlet容器自动配置原理

EmbeddedServletContainerAutoConfiguration：嵌入式的Servlet容器自动配置类

```java
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE)
@Configuration
@ConditionalOnWebApplication
@Import(BeanPostProcessorsRegistrar.class)
//导入BeanPostProcessorsRegistrar：Spring注解版；给容器中导入一些组件
//导入了EmbeddedServletContainerCustomizerBeanPostProcessor：
//后置处理器：bean初始化前后（创建完对象，还没进行赋值）执行初始化工作
public class EmbeddedServletContainerAutoConfiguration {
    
    @Configuration
	@ConditionalOnClass({ Servlet.class, Tomcat.class })//判断当前是否引入了Tomcat依赖；
	@ConditionalOnMissingBean(value = EmbeddedServletContainerFactory.class, search = SearchStrategy.CURRENT)//判断当前容器没有用户自己定义EmbeddedServletContainerFactory：嵌入式的Servlet容器工厂；作用：创建嵌入式的Servlet容器
	public static class EmbeddedTomcat {
 
		@Bean
		public TomcatEmbeddedServletContainerFactory tomcatEmbeddedServletContainerFactory() {
			return new TomcatEmbeddedServletContainerFactory();
		} 
	}
    
    /**
	 * Nested configuration if Jetty is being used.
	 */
	@Configuration
	@ConditionalOnClass({ Servlet.class, Server.class, Loader.class,
			WebAppContext.class })
	@ConditionalOnMissingBean(value = EmbeddedServletContainerFactory.class, search = SearchStrategy.CURRENT)
	public static class EmbeddedJetty {
 
		@Bean
		public JettyEmbeddedServletContainerFactory jettyEmbeddedServletContainerFactory() {
			return new JettyEmbeddedServletContainerFactory();
		} 
	}
 
	/**
	 * Nested configuration if Undertow is being used.
	 */
	@Configuration
	@ConditionalOnClass({ Servlet.class, Undertow.class, SslClientAuthMode.class })
	@ConditionalOnMissingBean(value = EmbeddedServletContainerFactory.class, search = SearchStrategy.CURRENT)
	public static class EmbeddedUndertow {
 
		@Bean
		public UndertowEmbeddedServletContainerFactory undertowEmbeddedServletContainerFactory() {
			return new UndertowEmbeddedServletContainerFactory();
		}
 
	}
```

1）、EmbeddedServletContainerFactory（嵌入式Servlet容器工厂）

```java
public interface EmbeddedServletContainerFactory {
 
   //获取嵌入式的Servlet容器
   EmbeddedServletContainer getEmbeddedServletContainer(
         ServletContextInitializer... initializers);
 
}
```

![img](https://img-blog.csdnimg.cn/2019090108225822.png)



2）、EmbeddedServletContainer：（嵌入式的Servlet容器）

![img](https://img-blog.csdnimg.cn/20190901082328442.png)

3）、以**TomcatEmbeddedServletContainerFactory**为例

```java
@Override
public EmbeddedServletContainer getEmbeddedServletContainer(
      ServletContextInitializer... initializers) {
    //创建一个Tomcat
   Tomcat tomcat = new Tomcat();
    
    //配置Tomcat的基本环境
   File baseDir = (this.baseDirectory != null ? this.baseDirectory
         : createTempDir("tomcat"));
   tomcat.setBaseDir(baseDir.getAbsolutePath());
   Connector connector = new Connector(this.protocol);
   tomcat.getService().addConnector(connector);
   customizeConnector(connector);
   tomcat.setConnector(connector);
   tomcat.getHost().setAutoDeploy(false);
   configureEngine(tomcat.getEngine());
   for (Connector additionalConnector : this.additionalTomcatConnectors) {
      tomcat.getService().addConnector(additionalConnector);
   }
   prepareContext(tomcat.getHost(), initializers);
    
    //将配置好的Tomcat传入进去，返回一个EmbeddedServletContainer；并且启动Tomcat服务器
   return getTomcatEmbeddedServletContainer(tomcat);
}
```

4）、我们对嵌入式容器的配置修改是怎么生效？

```
两种方法：ServerProperties、EmbeddedServletContainerCustomizer
```

**EmbeddedServletContainerCustomizer**：定制器帮我们修改了Servlet容器的配置

怎么修改的原理？

5）、容器中导入了**EmbeddedServletContainerCustomizerBeanPostProcessor** 嵌入式的Servlet容器定制器的后置处理器

```java
//初始化之前
@Override
public Object postProcessBeforeInitialization(Object bean, String beanName)
      throws BeansException {
    //如果当前初始化的是一个ConfigurableEmbeddedServletContainer类型的组件
   if (bean instanceof ConfigurableEmbeddedServletContainer) {
      postProcessBeforeInitialization((ConfigurableEmbeddedServletContainer) bean);
   }
   return bean;
}
 
private void postProcessBeforeInitialization(
			ConfigurableEmbeddedServletContainer bean) {
    //获取所有的定制器，调用每一个定制器的customize方法来给Servlet容器进行属性赋值；
    for (EmbeddedServletContainerCustomizer customizer : getCustomizers()) {
        customizer.customize(bean);
    }
}
 
private Collection<EmbeddedServletContainerCustomizer> getCustomizers() {
    if (this.customizers == null) {
        // Look up does not include the parent context
        this.customizers = new ArrayList<EmbeddedServletContainerCustomizer>(
            this.beanFactory
            //从容器中获取所有这个类型的组件：EmbeddedServletContainerCustomizer
            //定制Servlet容器，给容器中可以添加一个EmbeddedServletContainerCustomizer类型的组件
            .getBeansOfType(EmbeddedServletContainerCustomizer.class,
                            false, false)
            .values());
        Collections.sort(this.customizers, AnnotationAwareOrderComparator.INSTANCE);
        this.customizers = Collections.unmodifiableList(this.customizers);
    }
    return this.customizers;
}
 
//ServerProperties也是定制器
```

步骤：

1）、SpringBoot根据导入的依赖情况，给容器中添加相应的EmbeddedServletContainerFactory【TomcatEmbeddedServletContainerFactory】

2）、容器中某个组件要创建对象就会惊动后置处理器；EmbeddedServletContainerCustomizerBeanPostProcessor；

只要是嵌入式的Servlet容器工厂，后置处理器就工作；

3）、后置处理器，从容器中获取所有的**EmbeddedServletContainerCustomizer**，调用定制器的定制方法

### P50 web开发-嵌入式Servlet容器启动原理

***什么时候创建嵌入式的Servlet容器工厂？什么时候获取嵌入式的Servlet容器并启动Tomcat；\***

获取嵌入式的Servlet容器工厂：

1）、SpringBoot应用启动运行run方法

2）、refreshContext(context);SpringBoot刷新IOC容器【创建IOC容器对象，并初始化容器，创建容器中的每一个组件】；如果是web应用创建**AnnotationConfigEmbeddedWebApplicationContext**，否则：**AnnotationConfigApplicationContext**

3）、refresh(context);刷新刚才创建好的ioc容器；

```java
public void refresh() throws BeansException, IllegalStateException {
   synchronized (this.startupShutdownMonitor) {
      // Prepare this context for refreshing.
      prepareRefresh();
 
      // Tell the subclass to refresh the internal bean factory.
      ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();
 
      // Prepare the bean factory for use in this context.
      prepareBeanFactory(beanFactory);
 
      try {
         // Allows post-processing of the bean factory in context subclasses.
         postProcessBeanFactory(beanFactory);
 
         // Invoke factory processors registered as beans in the context.
         invokeBeanFactoryPostProcessors(beanFactory);
 
         // Register bean processors that intercept bean creation.
         registerBeanPostProcessors(beanFactory);
 
         // Initialize message source for this context.
         initMessageSource();
 
         // Initialize event multicaster for this context.
         initApplicationEventMulticaster();
 
         // Initialize other special beans in specific context subclasses.
         onRefresh();
 
         // Check for listener beans and register them.
         registerListeners();
 
         // Instantiate all remaining (non-lazy-init) singletons.
         finishBeanFactoryInitialization(beanFactory);
 
         // Last step: publish corresponding event.
         finishRefresh();
      }
 
      catch (BeansException ex) {
         if (logger.isWarnEnabled()) {
            logger.warn("Exception encountered during context initialization - " +
                  "cancelling refresh attempt: " + ex);
         }
 
         // Destroy already created singletons to avoid dangling resources.
         destroyBeans();
 
         // Reset 'active' flag.
         cancelRefresh(ex);
 
         // Propagate exception to caller.
         throw ex;
      }
 
      finally {
         // Reset common introspection caches in Spring's core, since we
         // might not ever need metadata for singleton beans anymore...
         resetCommonCaches();
      }
   }
}
```

4）、 onRefresh(); web的ioc容器重写了onRefresh方法

5）、web IOC容器会创建嵌入式的Servlet容器；**createEmbeddedServletContainer**();

6）、获取嵌入式的Servlet容器工厂：

EmbeddedServletContainerFactory containerFactory = getEmbeddedServletContainerFactory();

从ioc容器中获取EmbeddedServletContainerFactory 组件；**TomcatEmbeddedServletContainerFactory**创建对象，后置处理器一看是这个对象，就获取所有的定制器来先定制Servlet容器的相关配置；

7）、使用容器工厂获取嵌入式的Servlet容器：

this.embeddedServletContainer = containerFactory .getEmbeddedServletContainer(getSelfInitializer());

8）、嵌入式的Servlet容器创建对象并启动Servlet容器；先启动嵌入式的Servlet容器，再将ioc容器中剩下没有创建出的对象获取出来；

**总结：IOC容器启动创建嵌入式的Servlet容器**

![img](https://img-blog.csdnimg.cn/20190902065828560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)





### P51 web开发-使用外部Servlet容器&JSP支持

##### 1.***嵌入式Servlet容器***：

应用打成可执行的jar

优点：简单、便捷；

缺点：默认不支持JSP、优化定制比较复杂（使用定制器【ServerProperties、自定义EmbeddedServletContainerCustomizer】，自己编写嵌入式Servlet容器的创建工厂【EmbeddedServletContainerFactory】）；

##### 2.***外置的Servlet容器***：

外面安装Tomcat---应用war包的方式打包；

##### 3.***配置外部servlet步骤：***

1）、必须创建一个war项目；（利用idea创建好目录结构）

2）、将嵌入式的Tomcat指定为provided；

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-tomcat</artifactId>
   <scope>provided</scope>
</dependency>
```

3）、必须编写一个**SpringBootServletInitializer**的子类，并调用configure方法

```java
public class ServletInitializer extends SpringBootServletInitializer {
 
   @Override
   protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
       //传入SpringBoot应用的主程序
      return application.sources(SpringBoot04WebJspApplication.class);
   } 
}
```

4）、启动服务器就可以使用；

5）、配置jsp视图解析解器

```properties
spring.mvc.view.prefix=/WEB-INF/
spring.mvc.view.suffix=.jsp
```

##### 4.***实践\***

新建项目结构：

![img](https://img-blog.csdnimg.cn/20190903071235108.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

生成webapp目录

​	Project Structure ==> Modules ==> Web ==> Web Resoure Directories 双击路径 提示路径存在 是否需要创建

​	在上面的Deployment Descriptors 点击右侧的加号 添加web.xml，主要放到 webapp\WEB-INF\web.xml

配置tomcat：

![img](https://img-blog.csdnimg.cn/20190903071348950.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190903071413699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190903073747882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

##### 5.***错误解决方法：\***

**5.1 控制台乱码**

打开idea的安装路径，进入bin目录
用文本文件打开idea64.exe.vmoptions（如果安装的是32位系统选择idea.exe.vmoptions）
在最下面一行添加-Dfile.encoding=UTF-8

![img](https://img-blog.csdnimg.cn/20190903071018902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzE1ODczNTUx,size_16,color_FFFFFF,t_70)

最后关闭idea然后重新启动即可

5.2 tomcat启动报错”java.lang.IllegalStateException: ContainerBase.addChild: start: org.apache.catalina.LifecycleExcepti“

是jdk和tomcat的版本不一致导致，使用tomcat8即可



### P52 web开发-外部Servlet容器启动SpringBoot应用原理

#### 4.21 使用外置servlet容器启动原理

jar包：执行SpringBoot主类的main方法，启动ioc容器，创建嵌入式的Servlet容器；

war包：启动服务器，**服务器启动SpringBoot应用**【SpringBootServletInitializer】，启动ioc容器；

servlet3.0（Spring注解版）：

8.2.4 Shared libraries / runtimes pluggability 共享库和运行时插件

 

##### 规则：

1）、服务器启动（web应用启动）会创建当前web应用里面每一个jar包里面ServletContainerInitializer实例：

2）、ServletContainerInitializer的实现放在jar包的META-INF/services文件夹下，有一个名为javax.servlet.ServletContainerInitializer的文件，内容就是ServletContainerInitializer的实现类的全类名

3）、还可以使用@HandlesTypes，在应用启动的时候加载我们感兴趣的类；

 

##### 流程：

1）、启动Tomcat

2）、org\springframework\spring-web\4.3.14.RELEASE\spring-web-4.3.14.RELEASE.jar!\META-INF\services\javax.servlet.ServletContainerInitializer：

Spring的web模块里面有这个文件：**org.springframework.web.SpringServletContainerInitializer**

3）、SpringServletContainerInitializer将@HandlesTypes(WebApplicationInitializer.class)标注的所有这个类型的类都传入到onStartup方法的Set<Class<?>>；为这些WebApplicationInitializer类型的类创建实例；

4）、每一个WebApplicationInitializer都调用自己的onStartup；

![img](https://img-blog.csdnimg.cn/20190904071310635.png)

5）、相当于我们的SpringBootServletInitializer的类会被创建对象，并执行onStartup方法

6）、SpringBootServletInitializer实例执行onStartup的时候会createRootApplicationContext；创建容器

```java
protected WebApplicationContext createRootApplicationContext(
      ServletContext servletContext) {
    //1、创建SpringApplicationBuilder
   SpringApplicationBuilder builder = createSpringApplicationBuilder();
   StandardServletEnvironment environment = new StandardServletEnvironment();
   environment.initPropertySources(servletContext, null);
   builder.environment(environment);
   builder.main(getClass());
   ApplicationContext parent = getExistingRootWebApplicationContext(servletContext);
   if (parent != null) {
      this.logger.info("Root context already created (using as parent).");
      servletContext.setAttribute(
            WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, null);
      builder.initializers(new ParentContextApplicationContextInitializer(parent));
   }
   builder.initializers(
         new ServletContextApplicationContextInitializer(servletContext));
   builder.contextClass(AnnotationConfigEmbeddedWebApplicationContext.class);
    
    //调用configure方法，子类重写了这个方法，将SpringBoot的主程序类传入了进来
   builder = configure(builder);
    
    //使用builder创建一个Spring应用
   SpringApplication application = builder.build();
   if (application.getSources().isEmpty() && AnnotationUtils
         .findAnnotation(getClass(), Configuration.class) != null) {
      application.getSources().add(getClass());
   }
   Assert.state(!application.getSources().isEmpty(),
         "No SpringApplication sources have been defined. Either override the "
               + "configure method or add an @Configuration annotation");
   // Ensure error pages are registered
   if (this.registerErrorPageFilter) {
      application.getSources().add(ErrorPageFilterConfiguration.class);
   }
    //启动Spring应用
   return run(application);
}
```

7）、Spring的应用就启动并且创建IOC容器

```java
public ConfigurableApplicationContext run(String... args) {
   StopWatch stopWatch = new StopWatch();
   stopWatch.start();
   ConfigurableApplicationContext context = null;
   FailureAnalyzers analyzers = null;
   configureHeadlessProperty();
   SpringApplicationRunListeners listeners = getRunListeners(args);
   listeners.starting();
   try {
      ApplicationArguments applicationArguments = new DefaultApplicationArguments(
            args);
      ConfigurableEnvironment environment = prepareEnvironment(listeners,
            applicationArguments);
      Banner printedBanner = printBanner(environment);
      context = createApplicationContext();
      analyzers = new FailureAnalyzers(context);
      prepareContext(context, environment, listeners, applicationArguments,
            printedBanner);
       
       //刷新IOC容器
      refreshContext(context);
      afterRefresh(context, applicationArguments);
      listeners.finished(context, null);
      stopWatch.stop();
      if (this.logStartupInfo) {
         new StartupInfoLogger(this.mainApplicationClass)
               .logStarted(getApplicationLog(), stopWatch);
      }
      return context;
   }
   catch (Throwable ex) {
      handleRunFailure(context, listeners, analyzers, ex);
      throw new IllegalStateException(ex);
   }
}
```

**==启动Servlet容器，再启动SpringBoot应用==**



## 五、SpringBoot与Docker

### P53 Docker-简介

#### 一、什么是docker

Docker是一个开源的用于容器引擎，实现了资源隔离



### P54 Docker-核心概念

##### 二、Docker的核心概念

##### 2.1 **docker主机(Host)**

安装了Docker的机器（Docker是安装在操作系统之上的）

##### 2.2 docker仓库

用来保存各种打包好的docker镜像的；公共仓库docker-hub；私有仓库

##### 2.3 docker 客户端(Client )

连接docker主机进行操作

##### 2.4 docker镜像(Images)

软件打包好的镜像；放在docker仓库中

##### 2.5 docker容器(Container)

镜像启动运行后的实例，称为容器

#### 使用docker步骤

1 安装docker

2 从docker仓库中找到这个软件对应的镜像

3 使用docker运行这个镜像，这个镜像就会生成一个docker容器

4-对容器的启动停止就是对软件的启动停止

### P55 Docker-linux环境准备

#### 1、安装linux虚拟机

​	1- vmware，VirtualBox （安装）比较小 并且免费

​	2-导入虚拟机文件（已经装好的虚拟机）

​	3- 启动Linux虚拟机 输入用户名密码root/123456

​	4- 安装使用客户端连接工具 SmarTTY2.2.msi

​	5-设置虚拟机网络 连接方式-桥接 选择联网的网卡， 选择接入网线

​	6-使用命名重启虚拟机网卡

```
service network restart
```

​	7- 查看虚拟机IP地址

```
ip addr
#或
ifconfig
```

​	8- 使用客户端连接到linux

### P56 Docker-docker安装&启动&停止

#### 2、在虚拟机上安装docker

- 查看centos版本内核

  Docker要求CentOS系统的内容版本高于3.10

  ```bash
  uname -r
  ```

- 升级软件包及内核（选做）

  如果linux内核低于3.10 需要做这一步

  ```bash
  yum update
  ```

- 安装docker

  ```bash
  yum install docker
  ```

  输入 y 确认下载安装

- 启动docker

  ```bash
  systemctl start docker
  ```

- 将docker服务设置为开机自动启动

  ```bash
  systemctl enable docker
  ```

- 停止docker

  ```
  systemctl stop docker
  ```

  

### P57 Docker-docker镜像操作常用命令

#### 四、常用操作

##### 4.1 镜像操作

搜索镜像

```bash
docker search 关键字
docker search nginx
docker search redis
docker search mysql
#也可以去官网搜索hub.docker.com
```

拉取镜像

```bash
docker pull 镜像名:tag
#例如
docker pull mysql:5.7
docker pull mysql
docker pull mysql:latest #最终版
```

查看所有本地镜像

```bash
docker images
```

删除本地镜像

```bash
docker rmi 镜像id
docker rmi image_id
#例如
docker rmi 0166bbbbd9004
```

### P58 Docker-docker容器操作常用命令

##### 4.2 容器操作

拉取镜像 - 运行镜像 - 产生一个容器

```
docker search tomcat
docker pull tomcat
```

启动容器

```
docke run --name my-tomcat -d tomcat:latest
docke run --name my-tomcat -d -p 8080:8080 tomcat:latest
docke run --name my-tomcat -d -p 8888:8080 tomcat:latest  #端口映射 前一个端口代表虚拟机的端口 后一个端口是容器内的端口
```

查看运行中的容器

```
docker ps
```

停止运行中的容器

```
docker stop 容器id
docker stop 容器name

```

启动容器

```
docker start 容器id
docker start 容器name
```

查看所有容器 （包括停止的和运行中的）

```
docker ps -a
```

```
docker rm 容器id
```

```
docker ps -a
```

关闭Linux防火墙

```bash
service firewalld status
service firewalld stop
systemctl disable firewalld.service
systemctl disable firewalld.service
```

开启防火墙端口

```bash
# 开启防火墙端口：
firewall-cmd --zone=public --add-port=9200/tcp --permanent

# 命令含义：
　　–zone #作用域
　　–add-port=9200/tcp #添加端口，格式为：端口/通讯协议
　　–permanent #永久生效，没有此参数重启后失效
```

查看容器日志

```
docker logs 容器id
docker logs 容器name
```

更多命令,参考官方文档

```
https://docs.docker.com/engine/reference/commandline/docker
```

一个镜像启动多个容器

```bash
docke run --name my-tomcat1 -d -p 8086:8080 tomcat
docke run --name my-tomcat2 -d -p 8087:8080 tomcat
docke run --name my-tomcat3 -d -p 8088:8080 tomcat
```



### P59 Docker-docker安装MySQL

```bash
docker search mysql
docker pull mysql
docker images

#错误的演示
docker run --name mysql01 -d mysql

docker logs mysql01

docker rm mysql01

docker ps -a

docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql

docker stop mysql01
docker rm mysql01

#端口映射
docker run -p 3306:3306 --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

其他参数的使用

```bash
docker run --name mysql03 -v /my/custom:/etc/mysql/conf.d -e YSQL_ROOT_PASSWORD=123456  mysql:latst
#将/my/custom挂载到/etc/mysql/

```



## 六、SpringBoot与数据访问

### P60 数据访问-简介

关系型数据库的连接：JDBC、MyBatis、Spring Data JPA

SpringData不仅能管理关系型数据库，也能管理非关系型数据库 Redis MongoDB 

数据库使用spring-data-xxx 来操作

原生jdbc 使用spring-boot-jdbc-starter来操作

springboot默认没有提供mybatis的场景整合

#### 一、简介

对于数据库访问层，无论是SQL还是NO-SQL，SpringBoot默认采用整合Spring Data的方式进行统一处理，添加大量自动配置，屏蔽了很多设置。引入

各种xxxTemplate，xxxRepository来简化我们对数据访问层的操作。对我们来说只需要进行简单的设置即可。我们将在数据访问章节测试使用SQL相关、

NOSQL在缓存、消息、检索等章节测试。

- JDBC
- MyBatis
- JPA

### P61 数据访问-JDBC&自动配置原理

#### 二、整合最基本的JDBC数据源

##### 整合Jdbc基本步骤

- 引入Starter spring-boot-starter-jdbc
- 配置application.yml
- 测试
- 高级配置：使用druid数据源
  - 引入druid
  - 配置属性
- 配置druid数据源监控

##### 项目整合过程演示

##### 1、创建项目 

​	创建boot项目：springboot-jdbc 选中MySQL、JDBC、Web模块

##### 2、pom文件内容

```xml
<!--Web -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--JDBC -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<!--mysql 驱动-->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <scope>runtime</scope>
</dependency>
```

##### 3、配置application.yml或properties

```yaml
spring:
  datasource:
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
    #driver-class-name: com.mysql.cj.jdbc.Driver  #8.0以上版本
    url: jdbc:mysql://127.0.0.1:3306/users?serverTimezone=GMT
```

测试

```java
@RumWith(SpringRunner.class)
@SpringBootTest
public class TestDataSource{
	@Autowired
	DataSource dataSource;

	@Test
	public void testJdbc(){
        System.out.pringln(dataSource.getClass());
        Connection connection = dataSource.getConnectin();
        System.out.println(connection);
        connection.close();
    }
}	
```

默认使用org.apache.tomcat.jdbc.pool.DataSource tomcat连接池作为数据源

数据源的相关配置在DataSourceProperties中参考

自动配置原理：

org.springframework.boot.autoconfigure.jdbc

1、参考DataSourceConfiguration，根据配置创建数据源，默认使用tomcat连接池；可以使用spring.datasource.type指定自定义的数据源类型

2、SpringBoot默认支持：

```
org.apache.tomcat.jdbc.pool.DataSource
HikariDataSource
dbcp.BasicDataSource
dbcp2.BasicDataSource
```

3、自定义数据源类型

```java
@ConditionalOnMissingBean({DataSource.class})
@ConditionalOnProperty(
    name = {"spring.datasource.type"}
)
static class Generic {
    Generic() {
    }

    @Bean
    public DataSource dataSource(DataSourceProperties properties) {
        //使用DataSourceBuilder创建数据源，利用反射创建响应type的数据源，并且绑定相关属性
        return properties.initializeDataSourceBuilder().build();
    }
}
```

4、DataSourceInitiallizer：ApplicationListener

​	作用：

​		1）、runSchemaScripts()  运行建表语句

​		2）、runDataScripts();	运行插入数据的语句

默认规则：需要将文件命名为 

```properties
建表： schema-*.sql、 数据： data-*.sql
默认规则： schema.sql  或  schema-all.sql
可以使用  spring.datasource.schema=classpath:/dept.sql  指定位置
建表语句 会每次程序启动的时候 创建一次，多次启动会被执行多次
spring.datasource.initialization-mode  初始化模式（springboot2.0），其中有三个值，always为始终执行初始化，embedded只初始化内存数据库（默认值）,如h2等，never为不执行初始化。
```

```yaml
spring:
  datasource:
    schema:
      - classpath: department.sql  # 通过配置指定 建表文件
```

5、操作数据库：自动配置了JdbcTemplate 操作数据

![img](https://img2018.cnblogs.com/blog/1488757/201902/1488757-20190209151202176-303376459.png)

### P62 数据访问-整合Druid&配置数据源监控

##### 3.1 整合Durid数据源

上面讲到SpringBoot使用tomcat作为默认的数据源，还提供了性能更高的Hikari， 但一般情况我们还是会使用阿里提供的Druid数据源，因为Druid提供的功能更多，并且能够监控统计和安全管理。

这个时候我们需要先引入pom依赖，然后将spring.datasource.type 修改：

```xml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.16</version>
</dependency>
```

```yaml
spring:
  datasource:
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
    #driver-class-name: com.mysql.cj.jdbc.Driver  #8.0以上版本
    url: jdbc:mysql://127.0.0.1:3306/users?serverTimezone=GMT
    type: com.alibaba.druid.pool.DruidDataSource
    
    # 数据源其他配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #  配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

其他配置不生效的解决方法，我们还需要编写配置类：

```java
@Configuration
public class DruidConfig {
    @ConfigurationProperties(prefix = "spring.datasource")
        @Bean
        public DataSource druid(){
        return new DruidDataSource();
    }
}
```

再次运行上面查询数据源的方法，可以得到如下结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200928165842702.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ1MjcwNjY3,size_1,color_FFFFFF,t_70#pic_center)

如果报日志错误，与引入日志依赖

```
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>
```



##### 3.2 配置Druid监控

步骤：

```
1、配置一个管理后台的Servlet
	由于没有web.xml  使用ServletRegistrationBean
2、配置一个监控的Filter
```

```java
@Configuration
public class DruidConfig {
    @ConfigurationProperties(prefix = "spring.datasource")
        @Bean
        public DataSource druid(){
        return new DruidDataSource();
    }

    //配置Druid的监控
    //1、配置一个管理后台的Servlet
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        Map<String,String> initParams = new HashMap<>();
        initParams.put("loginUsername","admin");
        initParams.put("loginPassword","123456");
        initParams.put("allow","");
        //默认就是允许所有访问
        initParams.put("deny","192.168.15.21");  //拒绝访问
        bean.setInitParameters(initParams);
        return bean;
    }
    
    //2、配置一个web监控的filter
    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());
        Map<String,String> initParams = new HashMap<>();
        initParams.put("exclusions","*.js,*.css,/druid/*");  //不拦截
        bean.setInitParameters(initParams);
        bean.setUrlPatterns(Arrays.asList("/*"));
        return  bean;
    }
}
```



### P63 数据访问-整合MyBatis（一）-基础环境搭建

#### 三、整合Mybatis

##### 3.1 创建项目springboot-mybatis

选择Web、MySQL、JDBC、MyBatis

##### 3.2 pom文件

```xml
<dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>1.3.1</version>
 </dependency>
```

![img](https://img2018.cnblogs.com/blog/1488757/201902/1488757-20190209204844953-1066135607.png)

###### 导入Druid数据源 

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.10</version>
</dependency>
```

###### Druid数据源配置

```properties
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/users?serverTimezone=GMT
spring.datasource.username=root
spring.datasource.password=1234
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
#其他配置
# 下面为连接池的补充设置，应用到上面所有数据源中
spring.datasource.initialSize=5
spring.datasource.minIdle=5
spring.datasource.maxActive=20
# 配置获取连接等待超时的时间
spring.datasource.maxWait=60000
# 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
spring.datasource.timeBetweenEvictionRunsMillis=60000
# 配置一个连接在池中最小生存的时间，单位是毫秒
spring.datasource.minEvictableIdleTimeMillis=300000
spring.datasource.validationQuery=SELECT 1 FROM DUAL
spring.datasource.testWhileIdle=true
spring.datasource.testOnBorrow=false
spring.datasource.testOnReturn=false
# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.filters=stat,wall
spring.datasource.logSlowSql=true
```

并加入之前学习Mybatis时用到的实体，而后就可以进行测试

###### Druid的后台监控

​	这里省略.......  查看前面的文档

###### 创建Pojo

```java
public class Employee {
    private Integer id;
    private String lastName;
    private Integer gender;
    private String email;
    private Integer dId;
...
}
```

```java
public class Department {
    private Integer id;
    private String departmentName;
....
}
```

###### 自动建表语句

​	放在resource/sql/department.sql 和 employ.sql

###### 自动建表配置

​	表创建完成后，；立即注释掉，防止再次启动 重启建表，导致数据丢失

```yaml
spring:
  datasource:
    schema:
      - classpath:sql/department.sql
      - classpath:sql/employ.sql
```



### P64 数据访问-整合MyBatis（二）-注解版MyBatis

##### 定义Mapper接口类

```java
@Repository
//指定这是一个操作数据库的mapper
@Mapper
public interface DepartMapper {

    @Select("select * from department where id=#{id}")
    public Department getDeptById(Integer id);

    @Delete("delete from department where id=#{id}")
    public int deleteDeptById(Integer id);

    @Options(useGeneratedKeys=true, keyProperty="id")  //插入成功后返回自增主键
    @Insert("insert into department(departmentName) values(#{departmentName})")
    public int insertDept(Department department);

    @Update("update department set department_name=#{departmentName} where id=#{id}")
    public int updateDept(Department department);
}
```

##### 定义控制器Controller

```java
@ResponseBody
@Controller
public class DeptController {
    @Autowired
    DepartMapper departMapper;

    //模拟查询
    @RequestMapping("/dept/{id}")
    public Department getDept(@PathVariable("id")Integer id){
        Department dept = departMapper.getDeptById(id);
        return dept;
    }

    //模拟插入
    @RequestMapping("/dept")
    public Department insertDept(Department department){
        departMapper.insertDept(department);
        return department;
    }
}
```

##### 返回自增主键

插入成功后，将自增主键也被重新封装到对象中

```java
//使用自动生成的组件
@Options(useGeneratedKeys = true,keyProperty = "id")
@Insert("insert into department(departmentName) values(#{departmentName})")
public int insertDept(Department department);
```

##### 驼峰命名问题

默认情况下 ，没有开启驼峰命名，类的属性departName 和数据库的depart_name字段没有自动映射

![img](https://img2018.cnblogs.com/blog/1488757/201902/1488757-20190209205712216-1808695904.png)

此时的数据表列值发生改变

```java
@Select("select * from department where id=#{id}")
public Department getDeptById(Integer id);
```

 此时执行查询department_name是封装不到对象中的

 ![img](https://img2018.cnblogs.com/blog/1488757/201902/1488757-20190209205745901-279140575.png)

##### 开启驼峰命名

自定义MyBatis的配置规则，需要给容器添加一个ConfigurationCustomizer组件即可；

```java
import org.apache.ibatis,.session.Configuration;
@org.springframework.context.context.annotation.Configuration  //声明这是个配置类， 与mybatis的Configuration 冲突 所以使用全类名
public class MyBatisConfig {

    @Bean //加入容器
    public ConfigurationCustomizer configurationCustomizer(){
        return new ConfigurationCustomizer() {
            @Override
            public void customize(org.apache.ibatis.session.Configuration configuration) {

                //开启驼峰命名发
                configuration.setMapUnderscoreToCamelCase(true);
            }
        };
    }
}
```

Mybatis自动配置的源码如下

```java
@Bean
@ConditionalOnMissingBean
public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
    SqlSessionFactoryBean factory = new SqlSessionFactoryBean();
    factory.setDataSource(dataSource);
    factory.setVfs(SpringBootVFS.class);
    if (StringUtils.hasText(this.properties.getConfigLocation())) {
        factory.setConfigLocation(this.resourceLoader.getResource(this.properties.getConfigLocation()));
    }

    org.apache.ibatis.session.Configuration configuration = this.properties.getConfiguration();
    if (configuration == null && !StringUtils.hasText(this.properties.getConfigLocation())) {
        configuration = new org.apache.ibatis.session.Configuration();
    }

    if (configuration != null && !CollectionUtils.isEmpty(this.configurationCustomizers)) {
        Iterator var4 = this.configurationCustomizers.iterator();

        while(var4.hasNext()) {  //这里读取用户定义的 ConfigurationCustomizer
            ConfigurationCustomizer customizer = (ConfigurationCustomizer)var4.next();
            customizer.customize(configuration);
        }
    }
......
}
```

##### 配置Mybatis包扫描

**关于mapper类特别多的情况：**

如果mapper特别多的情况、每一个mapper类都是用@Mapper是极为麻烦的

此时可以使用@MapperScan直接指定mapper的包，进行对mapper的类批量扫描

@MapperScan可以加载配置类或者 主启动类上 都可以

```java
//使用MapperScan批量扫描所有的Mapper接口
@MapperScan(value = "com.beyondsoft.mybatis.mapper")
@SpringBootApplication
public class MybatisApplication {

    public static void main(String[] args) {
        SpringApplication.run(MybatisApplication.class, args);
    }
}
```



### P65 数据访问-整合MyBatis（二）-配置版MyBatis

##### 定义Mapper接口类

无论是使用注解还是xml配置文件的方式，在都需要使用@Mapper标注在接口类，或者使用@MapperScan接口扫描的方式 装配到容器中

```java
//@Mapper
//@Repository
public interface EmployeeMapper {

    public Employee getById(Integer id);

    public void insertEmp(Employee employee);
}
```

注意：这里的mapper接口需要使用@Mapper/@MapperScan进行扫描

##### Mybatis全局配置文件

配置驼峰命名

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 配置驼峰命名 -->
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
</configuration>
```

##### SQL映射文件

EmployeeMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cr.mybatis.mapper.EmployeeMapper">

    <select id="getById"
            resultType="com.cr.mybatis.pojo.Employee">
        SELECT * FROM employee WHERE id=#{id}
    </select>

    <insert id="insertEmp">
        INSERT INTO employee(lastName,email,gender,d_id)
        VALUES (#{lastName},#{email},#{gender},#{dId})
    </insert>
</mapper>
```

##### 项目配置文件中配置mybatis

springboot全局配置文件中，需要指定其配置文件的位置

proiperties方式

```properties
#配置mybatis
#mybatis的配置文件
mybatis.config-location=classpath:mybatis/mybatis-config.xml
#mapper的配置文件
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```

yml方式

```yaml
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
```

##### Controller测试类

```java
@ResponseBody
@Controller
public class EmpController {
    @Autowired
    EmployeeMapper employeeMapper;

    //查询
    @RequestMapping("/emp/{id}")
    public Employee getEmp(@PathVariable("id") Integer id){
        Employee emp = employeeMapper.getById(id);
        return emp;
    }

    @RequestMapping("/emp")
    public Employee insert(Employee employee){
        employeeMapper.insertEmp(employee);
        return employee;
    }
}
```

##### 在mybatis全局配置文件中配置驼峰命名

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 配置驼峰命名 -->
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
</configuration>
```



<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 配置驼峰命名 -->
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
</configuration>

### P66 数据访问-SpringData JPA简介

#### 四、Spring Data

##### 4.1 简介

Spring Data 项目的目的是为了简化构建基于Spring框架应用的数据访问技术，包括非关系数据库、Map-Reduce 框架、云数据服务等等；另外也包含对关系数据库的访问支持。

##### 4.2 Spring Data包含多个子项目

- Commons - 提供共享的基础框架，适合各个子项目使用，支持跨数据库持久化 

- Hadoop - 基于 Spring 的 hadoop 作业配置和一个 POJO 编程模型的 MapReduce 作业

- Key-Value  - 集成了 Redis 和 Riak ，提供多个常用场景下的简单封装

- Document - 集成文档数据库：CouchDB 和 MongoDB 并提供基本的配置映射和资料库支持

- Graph - 集成 Neo4j 提供强大的基于 POJO 的编程模型

- Graph Roo AddOn - Roo support for Neo4j

- JDBC Extensions - 支持 Oracle RAD、高级队列和高级数据类型

- JPA - 简化创建 JPA 数据访问层和跨存储的持久层功能

- Mapping - 基于 Grails 的提供对象映射框架，支持不同的数据库

- Examples - 示例程序、文档和图数据库

- Guidance - 高级文档

##### 4.3 Spring Data 特点

SpringData为我们提供使用统一的API来对数据访问层进行操作；这主要是Spring Data Commons项目来实现的。Spring Data Commons让我们在使用关系型或者非关系型数据访问技术时都基于Spring提供的统一标准，标准包含了CRUD（创建、获取、更新、删除）、查询、排序和分页的相关操作。

##### 4.4 Spring Data统一的Repository接口

SpringData帮我们封装了数据库操作，我们只需要继承接口，就可以进行操作，

SpringData有如下统一的接口：

- Repository<T, ID extends Serializable>：统一接口  

- RevisionRepository<T, ID extends Serializable, N extends Number & Comparable>：基于乐观锁机制 
- CrudRepository<T, ID extends Serializable>：基本CRUD操作 
- PagingAndSortingRepository<T, ID extends Serializable>：基本CRUD及分页

我们要使用JPA，就是继承JpaRepository,我们只要按照它的命名规范去对命名接口，便可以实现数据库操作功能

![img](https://img-blog.csdn.net/20180904185100265?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTQ4MTg3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 4.5 提供数据访问模板类xxxTemplate

​	如：JdbcTemplate、RedisTemplate、MongoTemplate

![img](https://img2018.cnblogs.com/blog/1113901/201904/1113901-20190423000641998-374051382.png)

##### 4.6 JPA与Spring Data

###### 	**1）JpaRepository基本功能**

​		编写接口继承继承JpaRepository既有crud及分页等基本功能

###### 	2）定义符合规范的方法命名

​		在接口中只需要声明符合规范的方法，即拥有对应的功能

​		![img](https://img-blog.csdn.net/20180904185218467?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTQ4MTg3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###### 	3）@Query自定义查询，定制查询SQL 

###### 	4）Specifications查询（Spring Data JPA支持JPA2.0的Criteria查询）



### P67 数据访问-整合JPA

#### 五、整合JPA

##### 整合步骤

```
1、引入spring-boot-starter-data-jpa
2、配置文件打印SQL语句
3、创建Entity标注JPA注解
4、创建Repository接口继承JpaRepository
5、测试方法
```

##### 创建工程

工程名：springboot-data-jpa 选择Web  JPA MySQL JDBC模块依赖

##### POM文件

```xml
    <!-- springdata jpa依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
```

##### 配置文件

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/jpa?useUnicode=true&characterEncoding=utf8&useSSL=true&serverTimezone=UTC
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      #更新或者创建数据表结构
      ddl-auto: update
    #控制台显示SQL
    show-sql: true
```

```
JPA：ORM（Object Relational Mapping）
1、编写一个实体类（Bean）和数据表进行映射，并且配置好映射关系
2、编写一个Dao接口操作实体类对应的数据表，SpringData称为Repository
```

##### 实体类User

```java
//使用JPA注解配置映射关系
@Entity //告诉JPA这是一个实体类（和数据表映射的类）
@Table(name = "tbl_user") //@Table来指定和哪个数据表对应; 如果省略默认表名就是user；
@JsonIgnoreProperties(value = { "hibernateLazyInitializer"})
public class User {
 
    @Id //这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)//自增主键  主键生成策略
    private Integer id;
 
    @Column(name = "last_name",length = 50) //这是和数据表对应的一个列
    private String lastName;
 
    @Column //省略默认列名就是属性名
    private String email;
}
```

##### Repository接口/Dao接口

UserRepository.java

```java
//继承JpaRepository来完成对数据库的操作
public interface UserRepository extends JpaRepository<User,Integer> { //第一个参数是对象类型，第二个参数是主键的类型 可序列化的
}
```

##### 配置文件中配置JPA

```yaml
spring:
  jpa:
    hibernate:
      #更新或者创建数据表结构
      ddl-auto: update
    #控制台显示SQL
    show-sql: true
```

** 所有JPA相关配置都在JpaProperties对象中绑定着对应的属性，可以在这个文件里查看配置项

##### 控制器Controller

```java
@RestController
public class UserController {
 
    @Autowired
    private UserRepository userRepository;
 
    @GetMapping("/user/{id}")
    public User getUser(@PathVariable("id") Integer id){
        User user = userRepository.getOne(id);
        return user;
    }
 
    @GetMapping("/user")
    public User insertUser(User user){
        User save = userRepository.save(user);
        return save;
    }
}
```



## 七、SpringBoot 启动配置原理

启动原理、运行流程、自动配置原理

![img](https://img-blog.csdnimg.cn/20191018120432948.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwNDE3NDk5,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20191018120507500.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwNDE3NDk5,size_16,color_FFFFFF,t_70)

### P68 原理-第一步：创建SpringApplication

#### 1、几个重要的事件回调机制

配置在META-INF/spring.factories

**ApplicationContextInitializer**

**SpringApplicationRunListener**



只需要放在ioc容器中

**ApplicationRunner**

**CommandLineRunner**

#### 2、启动流程

##### 2.1 创建SpringApplication对象

```java
public static ConfigurableApplicationContext run(Object[] sources,String[] args){
	return new SpringApplication(source)//第一步创建SpringApplication对象
		.run(args);					    //第二步运行run()方法
}
```

```java
public SpringApplication(Object... sources){
    initialize(sources); //初始化方法
}
```

```java
private void initialize(Object[] sources) {
    //保存主配置类
    if (sources != null && sources.length > 0) {
        this.sources.addAll(Arrays.asList(sources));
    }
    //判断当前是否一个web应用
    this.webEnvironment = deduceWebEnvironment();
    //从类路径下找到META-INF/spring.factories配置的所有ApplicationContextInitializer；然后保存起来
    setInitializers((Collection) getSpringFactoriesInstances(
        ApplicationContextInitializer.class));
    //从类路径下找到ETA-INF/spring.factories配置的所有ApplicationListener
    setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
    //从多个配置类中找到有main方法的主配置类
    this.mainApplicationClass = deduceMainApplicationClass();
}
```

![img](https://img-blog.csdnimg.cn/20201119134812441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0F1cm9yYV9fX19f,size_16,color_FFFFFF,t_70#pic_center)

![img](https://img-blog.csdnimg.cn/20201119134812432.png#pic_center)


### P69 原理-第二步：启动应用


##### 2.2 运行run()方法

```java
public ConfigurableApplicationContext run(String... args) {
   StopWatch stopWatch = new StopWatch();	//主程序停止事件的监听
   stopWatch.start();
   ConfigurableApplicationContext context = null;
   FailureAnalyzers analyzers = null;
   configureHeadlessProperty();
    
   //获取SpringApplicationRunListeners；从类路径下META-INF/spring.factories中获取
   SpringApplicationRunListeners listeners = getRunListeners(args);
   //回调所有的获取SpringApplicationRunListener.starting()方法
   listeners.starting();
   try {
       //封装命令行参数
      ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
      //准备环境
      ConfigurableEnvironment environment = prepareEnvironment(listeners, applicationArguments);
       		//创建环境完成后回调SpringApplicationRunListener.environmentPrepared()；表示环境准备完成
       
      Banner printedBanner = printBanner(environment); //打印SpringBoot的Banner
       
       //创建ApplicationContext；决定创建web的ioc还是普通的ioc
      context = createApplicationContext();
       
      analyzers = new FailureAnalyzers(context); //创建异常分析报告
       //准备上下文环境;将environment保存到ioc中；而且applyInitializers()；
       //applyInitializers()：回调之前保存的所有的ApplicationContextInitializer的initialize方法
       //回调所有的SpringApplicationRunListener的contextPrepared()；
       //
      prepareContext(context, environment, listeners, applicationArguments,printedBanner);
       //prepareContext运行完成以后回调所有的SpringApplicationRunListener的contextLoaded()；
       
       //s刷新容器；ioc容器初始化（如果是web应用还会创建嵌入式的Tomcat）；Spring注解版
       //扫描，创建，加载所有组件的地方；（配置类，组件，自动配置）
      refreshContext(context);
       //从ioc容器中获取所有的ApplicationRunner和CommandLineRunner进行回调
       //ApplicationRunner先回调，CommandLineRunner再回调
      afterRefresh(context, applicationArguments);
       //所有的SpringApplicationRunListener回调finished()方法
      listeners.finished(context, null);
      stopWatch.stop();
      if (this.logStartupInfo) {
         new StartupInfoLogger(this.mainApplicationClass)
               .logStarted(getApplicationLog(), stopWatch);
      }
       //整个SpringBoot应用启动完成以后返回启动的ioc容器；
      return context;
   }
   catch (Throwable ex) {
      handleRunFailure(context, listeners, analyzers, ex);
      throw new IllegalStateException(ex);
   }
}
```



### P70 原理-事件监听机制相关测试

#### 1- 创建事件监听机制：Initializer和Runner

##### 配置在META-INF/spring.factories

**ApplicationContextInitializer**

**SpringApplicationRunListener**



##### 只需要放在ioc容器中

**ApplicationRunner**

**CommandLineRunner**

#### 2- **ApplicationContextInitializer**

配置在META-INF/spring.factories

```java
public class HelloApplicationContextInitializer implements ApplicationContextInitializer<ConfigurableApplicationContext> {
    @Override
    public void initialize(ConfigurableApplicationContext applicationContext) {
        System.out.println("ApplicationContextInitializer...initialize..."+applicationContext);
    }
}
```

#### 3- **SpringApplicationRunListener**

```java
public class HelloSpringApplicationRunListener implements SpringApplicationRunListener {

    //必须有的构造器  否则启动报错
    public HelloSpringApplicationRunListener(SpringApplication application, String[] args){

    }

    @Override
    public void starting() {
        System.out.println("SpringApplicationRunListener...starting...");
    }

    @Override
    public void environmentPrepared(ConfigurableEnvironment environment) {
        Object o = environment.getSystemProperties().get("os.name");
        System.out.println("SpringApplicationRunListener...environmentPrepared.."+o);
    }

    @Override
    public void contextPrepared(ConfigurableApplicationContext context) {
        System.out.println("SpringApplicationRunListener...contextPrepared...");
    }

    @Override
    public void contextLoaded(ConfigurableApplicationContext context) {
        System.out.println("SpringApplicationRunListener...contextLoaded...");
    }

    @Override
    public void finished(ConfigurableApplicationContext context, Throwable exception) {
        System.out.println("SpringApplicationRunListener...finished...");
    }
}
```

#### 配置（META-INF/spring.factories）

\ 为换行的意思

```properties
org.springframework.context.ApplicationContextInitializer=\
com.atguigu.springboot.listener.HelloApplicationContextInitializer

org.springframework.boot.SpringApplicationRunListener=\
com.atguigu.springboot.listener.HelloSpringApplicationRunListener
```



#### 4- **ApplicationRunner**

需要加载在ioc容器中 所以添加@Component注解

```java
@Component
public class HelloApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("ApplicationRunner...run....");
    }
}
```

#### 5- **CommandLineRunner**

```java
@Component
public class HelloCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("CommandLineRunner...run..."+ Arrays.asList(args));
    }
}
```

#### 启动顺序

SpringApplicationRunListener...starting...

SpringApplicationRunListener...environmentPreoared...

ApplicationContextInitializer...initialize...

SpringApplicationRunListener...contextPrepared...

SpringApplicationRunListener...contextLoaded...

ApplicationRunner...run...

SpringApplicationRunListener...finished...



## 八、SpringBoot自定义Starters

starters原理、自定义starters

### P71 尚硅谷_原理-自定义starter

#### starter启动器的一些知识

![img](https://img-blog.csdnimg.cn/20191018165515177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwNDE3NDk5,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20191018170040851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwNDE3NDk5,size_16,color_FFFFFF,t_70)



#### 自定义starter考虑的几个问题

starter场景启动器：

 1、这个场景需要使用到的依赖是什么？

 2、如何编写自动配置

```java
@Configuration  //指定这个类是一个配置类
@ConditionalOnXXX  //在指定条件成立的情况下自动配置类生效
@AutoConfigureAfter  //指定自动配置类的顺序
@Bean  //给容器中添加组件

@ConfigurationPropertie  //结合相关xxxProperties类来绑定相关的配置
@EnableConfigurationProperties //让xxxProperties生效 并加入到容器中

自动配置类要能加载
将需要启动就加载的自动配置类，配置在META-INF/spring.factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\

```

3、模式：

启动器（starter）只用来做依赖导入；

专门来写一个自动配置模块；

启动器依赖自动配置；别人只需要引入启动器（starter）

mybatis-spring-boot-starter；自定义启动器：功能名-spring-boot-starter

#### 自定义starter步骤

1）、启动器模块

2）、自动配置模块

#### 自定义starter示例

创建一个空项目，项目中添加两个模块，atguigu-spring-boot-starter模块和 atguigu-spring-boot-starter-autoconfigurer模块

##### 1 启动器模块 atguigu-spring-boot-starter

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.atguigu.starter</groupId>
    <artifactId>atguigu-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!--启动器-->
    <dependencies>

        <!--引入自动配置模块-->
        <dependency>
            <groupId>com.atguigu.starter</groupId>
            <artifactId>atguigu-spring-boot-starter-autoconfigurer</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>
</project>
```



##### 2 自动配置模块 atguigu-spring-boot-starter-autoconfigurer

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>

   <groupId>com.atguigu.starter</groupId>
   <artifactId>atguigu-spring-boot-starter-autoconfigurer</artifactId>
   <version>0.0.1-SNAPSHOT</version>
   <packaging>jar</packaging>

   <name>atguigu-spring-boot-starter-autoconfigurer</name>
   <description>Demo project for Spring Boot</description>

   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>1.5.10.RELEASE</version>
      <relativePath/> <!-- lookup parent from repository -->
   </parent>

   <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
      <java.version>1.8</java.version>
   </properties>

   <dependencies>

      <!--引入spring-boot-starter；所有starter的基本配置-->
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter</artifactId>
      </dependency>

   </dependencies>
</project>
```



```java
package com.atguigu.starter;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "atguigu.hello")
public class HelloProperties {

    private String prefix;
    private String suffix;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getSuffix() {
        return suffix;
    }

    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}
```

```java
package com.atguigu.starter;

public class HelloService {

    HelloProperties helloProperties;

    public HelloProperties getHelloProperties() {
        return helloProperties;
    }

    public void setHelloProperties(HelloProperties helloProperties) {
        this.helloProperties = helloProperties;
    }

    public String sayHellAtguigu(String name){
        return helloProperties.getPrefix()+"-" +name +"-" + helloProperties.getSuffix();
    }
}
```

```java
package com.atguigu.starter;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConditionalOnWebApplication //web应用才生效
@EnableConfigurationProperties(HelloProperties.class)
public class HelloServiceAutoConfiguration {

    @Autowired
    HelloProperties helloProperties;
    
    @Bean
    public HelloService helloService(){
        HelloService service = new HelloService();
        service.setHelloProperties(helloProperties);
        return service;
    }
}
```

在resource目录下新建 META-INF\spring.factories 文件

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.atguigu.starter.HelloServiceAutoConfiguration
```

将模块安装到仓库中 idea --> maven  --> Lifecycle  --> install  ；按依赖关系，先安装autoconfigurer模块，再安装starter模块

#### 测试动配置

##### 1 创建项目

创建一个新的项目，选中Web模块依赖，选择SpringBoot的版本与自定义的starter版本一致

##### 2 引入自定义starter依赖

在pom文件中依赖 自定义的starter：atguigu-spring-boot-starter

##### 3 Controller测试

```java
@RestController
public class HelloController{
    
    @Autowired
    private HellService helloService;
    
    @GetMapping("/hello")
    public String hello(){
        return helloService.sayHelloAtguigu("hello");
    }
    
}
```

##### 4 配置文件

```properties
atguigu.hello.prefix=atguigu
atguigu.hello.suffix=world
```



### P72 尚硅谷_结束语

##### SpringBoot核心技术 后八章

视频地址：https://www.bilibili.com/video/BV1KW411F7oX?t=10

##### 官方springboot整合示例代码

http://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples

# Spring Boot 高级篇



## 九、SpringBoot与缓存



## 十、SpringBoot与消息



## 十一、SpringBoot与检索



## 十二、SpringBoot与任务



## 十三、SpringBoot与安全安全



## 十四、SpringBoot与分布式



## 十五、SpringBoot与开发热部署



##  十六、SpringBoot与监控管理

