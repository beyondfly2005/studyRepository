> https://www.bilibili.com/video/BV1gW411W76m

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

##### 1、SpringBoot简介

###### 概念

SpringBoot来简化Spring开发，约定大于配置，去繁从简，just run就能创建一个独立的，产品级别的应用

###### 背景

J2EE笨重的而开发，繁多的配置，低下的开发效率，复杂的部署流程、第三方技术集成难度大

###### 解决

Spring全家桶时代 SpringBoot提供了一站式解决方案 SprigCloud分布式解决方案。

###### 优点

快速场景独立运行的Spring项目以及与主流框架集成

使用嵌入式的eServlet容器，应用无需打成war包

starter自动依赖于版本控制

大量的自动配置，简化开发，也可以修改默认值

无需大量配置xml 无代码生成 开箱即用

准生成环境的运行时监控

与云计算的天然集成

###### 缺点

入门容易 精通难

排查错误较难

##### 2、微服务

2014 Martin Flowler 微服务是一种架构风格，一个应用一组小型服务的集合，可以通过http方式进行交互。

单体应用All In One

优点：

开发develop 测试test 简单

部署简单deploy

水平扩展简单scale

微服务的每一个功能都是一个可独立替换的和独立升级的软件单元。

服务要有多小，如何规划拆分粒度（服务微化）

##### 3、入门环境准备

前置知识：

- Spring框架的使用经验

- 熟练使用Maven进行项目构建和依赖管理
- 熟练使用Eclipse或者IDEA

环境约束

- jdk 1.8
- maven 3.x
- IntellJ IDEA 2017
- SpringBoot 1.5.9.RELEASE  推荐使用2.0以上

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

##### 4、springboot hello-world

4.1 

4.2 导入SpringBoot依赖

4.3 编译一个主程序 启动springboot应用

4.4 编写业务逻辑 service controller

4.5 运行主程序测试

4.6 简化部署jar方式

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

##### 5、starter 场景启动器

##### 6、HelloWorld细节：自动配置

SpringBoot 将所有的功能场景都抽取处理，做成一个个的starter启动器，只需要在项目里面引入哲学starter相关场景的所有依赖都会导入进来，需要什么功能

### 二、SpringBoot配置

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