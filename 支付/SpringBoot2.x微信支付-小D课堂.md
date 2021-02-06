# SpringBoot2.x项目实战微信支付教程

> 课程地址：https://www.bilibili.com/video/BV1q7411P7ge

### 1、 SpringBoot整合微信支付开发在线教育视频站点介绍



### 2、中大型公司里面项目开发流程

- 产品调研

	- 需求评审
	- UI设计（产品/设计/前端后端测试运营）
	- 开发（前端开发 后端开发）API文档 Mock数据 架构设计 数据库 编码 单元测试
	- 联调 前后端联调
	- 自测
	- 项目/模块提测  冒烟测试 黑盒白盒测试 压力测试 压测qps
	- BugFix 修复bug
	- 回归测试
	- 运维和开发部署上线
	- 上线测试
	- 灰度发布
	- 全量发布
	- 维护和运维



### 3、在线教育战斗需求分析和架构设计

​	3.1  需要开发的功能

​			首页视频列表

​			视频详情

​			微信扫码登录

​			下单微信支付

​			我的订单列表

​	3.2 架构设计

​		前后端分离

​		动静分离

​		后端：集群部署+支持水平扩展 Nginx负载均衡

​		技术选择：

​				后端：IDEA + SpringBoot2.0 +Mybatis +缓存+HttpClient +MySQL

​				前端：HTML5 + BootStrap + Jquery

​		测试要求：

​				首页和视频详情页qps 单机要求500+

### 4、在线教育后台数据库设计

##### 4.1 数据库表设计

ER图

- 实体对象：矩形

- 属性：椭圆

- 关系：菱形

##### 4.2 数据库表

- video表	视频表

- video_order表 用户视频表

- user表 用户表

- comment表  课程描述

- chapter表	课程章

- episode 	课程节

##### 4.3 字段冗余

​	什么是自动冗余

​	什么时候选择字段冗余

​	优缺点

##### 4.4 Mysql测试数据导入

##### 4.5 没有一成不变的架构，没有通用的设计方案

​	一定要与业务结合

### 5、项目环境搭建

##### 5.1 创建SpringBoot项目

​	springboot 版本2.0.3

​	项目名称：project-video-pay

​	

##### 5.2 引入依赖

​	spring-boot-starter-web

​	spring-boot-starter-data-redis

​	mybatis-spring-boot-starter

​	mysql-connector-java

​	spring-boot-starter-test	

​	PS：如果一开始没有创建连接数据库，可以先不加 mysql和mybatis的依赖

##### 5.3 创建Controller

​	TestController 输出 HelloWorld

##### 5.4 启动项目hello wold

访问http://localhost:8080

### 6、IDEA导入SpringBoot项目

导入上面使用 Eclipse创建的工程



### 7、热部署

### 8、基本目录结构

#### 9、开源工具的选择优缺点和抽象方法的建议

### 10、MySQL逆向工程

使用IDEA自动生成Java类

1、IDEA连接数据库

2、通过IDEA生成实体类

​	选中一张表，右键---> Scripted Extensions ---> 选择Generate POJO.clj或者Generate POJOS.groovy，选择存放的路径，完成

​	自定义包名

​	常用类型java.util.Date

### 11、配置文件的注入

### 20、微信扫码登录