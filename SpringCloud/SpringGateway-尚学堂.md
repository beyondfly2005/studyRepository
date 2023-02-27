# SpringG ateway网关

> 课程地址
> https://www.bilibili.com/video/BV1JB4y1F7aL


## 一 API网关

#### 1、什么是API网关
API网关作用是把各个服务对外提供的API汇聚起来，让外界看起来是一个统一的接口，同时也可在网关中提提供额外的功能。
总结：网关就是所有项目后端API的统一的入口



#### 2、网关组成

网关 = 路由转发 + 过滤器（编写额外功能）

##### 2.1 路由转发
接收外界请求，通过网关的路由转发，转发到后端的服务器上
如果只有这一个功能，那么看起来和Nginx反向代理服务器很小，外界访问Nginx，有nginx做负载均衡，然后把请求转发到对应的服务器上

##### 2.2 过滤器
网关非常重要的功能就是过滤器
过滤器中默认提供了25个内置功能还支持额外的自定义功能
对我们来说比较常用的功能 有：网关的容错、限流以及请求及相应的额外处理

#### 3 SpringCloud中提供的网关解决方案
##### 3.1 Spring cloud Netflix zuul
属于spring Cloud Netflix下的一个组件，具有灵活、简单的特点，在早期的Spring Cloud中使用的比较多。

##### 3.1 Spring cloud Gateway
由Spring 自己退出的网关产品，完全依赖Spring自家产品，符合Spring战略意义，其版本更新等都由Spring自己把控
目前很多项目中都使用Gateway代替Zuul
在本课程中讲解的也是Gateway

## 二 Spring Cloud 介绍

### 1 简介
Spring Cloud Gateway是Spring Cloud中的二级子项目，提供了微服务挖网关功能，包含权限安全 监控/指标等功能。

### 2 名称解释 

##### 2.1 Route
Route 中文名称为路由，Gateway里面的Rout是主要学习的内容， 一个Gateway项目可以包含多个Route。
一个路由包括：ID、URL、Predicate集合、Filter集合
在Route 中ID是自定义的，URI就是一个地址，剩下的Predicate和Filter ，Route

##### 2.2 Predicate

中文：谓词

 简单理解：路由规则 附加一些条件和内容

##### 2.3 Filter 

过滤器 加功能



## 三 GateWay入门

自醒的、自洽的、内聚的



