### 畅购商城项目

> https://www.bilibili.com/video/BV1GE411G7Hg
>
> https://www.bilibili.com/video/BV1e7411G7eu

#### 课程内容

第1天 ：畅购商城工程结构

第2天：

第3天：商品的增删改查 代码生成器 

第4天：Lua脚本 OpenResty的作用、Canal同步工具

第5天：Docker下安装Elasticsearche、DSL语句操作

第6天：

第7天：Thymeleaf模板引擎、商品搜索页的页面渲染、详情页的展示

第8天：微服务网关 JWT鉴权 Gateway网关鉴权

第9天：Oauth2.0第三方授权认证

第10天：OOauth2.0+页面权限控制，及各个微服务务之间权限认证

第11天：结算和下单操作：下单 修改库存 修改积分

第12天：微信支付

第13天：分布式事务 FastEasyAndRollBack

第14天：秒杀 Redis缓存 Redis单线程  服务雪崩：队列机制

第15天：削峰及时解决方案 队列机制有序消费用户请求

第16天：项目集群搭建 Eureka集群 Redis集群 

### 第1章 框架搭建

学习目标

- 了解电商
- 了解畅购架构
- 了解畅购工厂结构
- 畅购工厂搭建
- 商品微服务搭建
- 品牌增删改查

重点： 后4个：了解工程结构  搭建

#### 1、走进电商

##### 1.1 电商行业分析

行业特点：前景好

##### 1.2 电商系统技术特点

- 技术新

- 技术范围广

  分布式存储

- 分布式

- 高并发、集群、负载均衡、高可用

- 海量数据

- 业务复杂

- 系统安全

##### 1.3 主要电商模式

###### B2B

​	阿里巴巴、慧聪网

###### C2C

​	淘宝、易趣、

###### B2C

​	唯品会、乐蜂网

###### C2B（消费者到企业）

​	海尔商城、尚品宅配

###### O2O(Online to Offline线上到线下)

​	美团、饿了么

###### F2C （Factory to Customer 从厂家到消费者）

###### B2B2C（从商品提供商到电子商务企业到消费者）

​	京东商城 天猫商城

#### 2、畅购-需求分析设计

##### 2.1 需求分析

##### 2.2 系统设计

###### 2.2.1 前后端分离

###### 2.2.2 技术架构

前端：Vue.js Node.js Lua Html5 ElementUI Thymeleaf

运维：

微服务技术栈：

SprignBoot OAth2.0 JWT

Spring-AMQP

持久层：Mybatis+TKMapper SpringDataEs SpringDataRedis

数据库&消息队列：MySQL RabbitMQ MySQL读写分离

支付接口：微信支付

###### 2.2.3 系统架构图



### 第2章 分布式文件存储FastDFS



### 第3章 商品发布



### 第4章 Lua、Canal实现广告缓存



### 第5章 商品搜索

###### 学习目标

- Elasticssearch安装
- IK分词
- Kibana的使用 DSL语句
- DS导入商品搜索数据
- 关键词搜索 能够实现搜搜流程代码的编写
- 分词统计搜索

#### 1、Elasticssearch安装





### 第6章 商品搜索

学习目标



### 第7章  Thymeleaf、Rabbitmq实现静态页

###### 学习目标

- Thymeleaf的介绍(模板引擎)
- Thymeleaf的入门
- Thymeleaf语法及标签
- 商品详情页静态化工程搭建
- 商品详情页静态化功能实现

#### 1、Thymeleaf介绍

其他模板引擎

```
JSP		SpringBoot已经默认不支持
Velocity 	比较久远 用的较少
FreeMarker	
Thymeleaf	

```



#### 2、Springboot整合thymeleaf

实现步骤为：

- 创建一个springboot项目
- 添加thymeleaf的起步依赖
- 添加springewb的起步依赖
- 编写htmol使用thymeleaf的语法
- 编写Controller设置变量的值到model中

#### 3、Thymeleaf 语法简介

#### 4、搜索页面渲染



### 第8章 微服务网关和JWT令牌

##### 学习目标

- 掌握微服务网关的系统搭建
- 里阿杰什么是微服务网关以及他的作用（回顾微服务网关的路由过滤规则）
- 掌握系统中心微服务的搭建（用户微服务）
- 掌握用户密码加密存储bcrypt
- 了解JWT鉴权的介绍
- 掌握JWT鉴权的使用
- 掌握JWT令牌存储用户登录信息，在微服务网关中识别登录信息（用户的身份）
- 掌握网关使用JWT进行校验
- 掌握网关限流



### 第9章 Spring Security Oauth2 JWT

#### 1.用户认证分析



#### 1.1 用户认证与授权



#### 1.2 单点登录

#### 1.3 第三方账号登录

###### 1.3.1第三方登录介绍

###### 1.3.2 第三方登录优点

- 方便快捷
- a
- 降低了

###### 1.3.3 第三方认证



#### 2. 认证技术方案

##### 2.1 单点登录技术方案

单点登录的特点：

```properties
1、认证系统为独立的系统
2、各个系统
```



java中有很多认证框架都可以实现单点登录

```pr
1、Apache Shiro
2、CAS
3、Spring Security CAS
4、OAuth2.0
```



##### 2.2 OAuth2认证

OAuth(开放授权)是一个开放标准，允许用户

```
OAuth2.0属于一个开放的授权认证标准，并不强调受信任
```

###### 2.2.1 Oauth2.0认证流程



###### 2.2.2 Oauth2.0 在项目中的应用



### 第10章 购物车



### 第11章 订单



### 第12章 微信支付



### 第13章 秒杀



### 第14章 秒杀



### 第15章 分布式事务



### 第16章 集群高可用