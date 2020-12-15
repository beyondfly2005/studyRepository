 ### 尚硅谷SpringBoot顶尖教程(springboot之idea版spring boot)

> 课程地址：https://www.bilibili.com/video/BV1gW411W76m

#### 课程内容

##### （基础篇）

一、SpringBoot入门

二、SpringBoot配置

三、SpringBoot与日志

四、SpringBoot与Web开发

五、SpringBoot与Docker

六、SpringBoot与数据访问

七、SpringBoot、启动与数据访问

八、SpringBoot自定义Starters

##### （高级篇）

九、SpringBoot与缓存

十、SpringBoot与消息

十一、SpringBoot与检索

十二、SpringBoot与任务

十三、SpringBoot与安全安全

十四、SpringBoot与分布式

十五、SpringBoot与开发热部署

十六、SpringBoot与监控管理



### 一、SpringBoot入门

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

#### 2、微服务

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

**@SpringBootApplication：** Spring Boot应用标注在某个类上 是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；



**@SpringBootConfiguration：**Spring Boot配置类 ；标注在某个类上，表示这是一个SpringBoot的配置类



**@Configuration** 在配置类上来标注这个注解，表明这是一个配置类 ，也就是配置文件的java实现方式，配置类也是容器中的一个组件



**@EnableAutoConfiguration：** 开启自动配置功能；以前需要自己配置的地方，SpringBoot 帮助我们自动配置



**@AutoConfigurationPackage**

**@Import** 给容器中导入一个组件，导入的组件由AutoConfigurationPackages.Registrar.class

Spring注解版





### 二、SpringBoot配置

#### 1、使用向导快速创建SpringBoot应用



#### 2、配置文件yaml简介



#### 3、配置文件yaml语法



#### 4、配置文件yaml配置文件值的获取



#### 5、配置文件properties编码问题



### 三、SpringBoot与日志



### 四、SpringBoot与Web开发



### 五、SpringBoot与Docker



### 六、SpringBoot与数据访问



### 七、SpringBoot、启动与数据访问



### 八、SpringBoot自定义Starters



### 九、SpringBoot与缓存



### 十、SpringBoot与消息



### 十一、SpringBoot与检索



### 十二、SpringBoot与任务



### 十三、SpringBoot与安全安全



### 十四、SpringBoot与分布式



### 十五、SpringBoot与开发热部署



###  十六、SpringBoot与监控管理