> 课程地址：https://www.bilibili.com/video/BV1eE41187Ug



## 1、微服务基础知识

#### 1.1 系统架构的演变

#### 1.2 分布式核心知识

#### 1.3 常用的微服务架构

##### 1.3.1 SpringCloud

SpringCloud是一系列框架的有序集合，它利用SpringBoot的开发便利性巧妙的简化了分布式系统基础设施的开发，如服务注册发现、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用SpringBoot的开发风格做到一键启动和部署，SpringCloud并没有重复制造轮子，它只是将目前各家公司开发的比较成熟、经得起时间考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和维护的分布式系统开发工具包。

##### 1.3.2 ServiceComb 

​	Apache ServiceComb是业界第一个APache微服务顶级项目，是一个开源微服务解决方案，致力于帮助企业，用户和开发者将企业应用轻松微服务化上云，并实现微服务应用的高效运维管理。其提供一站式开源微服务解决方案，融合SDK框架级，零侵入ServiceMesh场景并支持多语言。

##### 1.3.3 ZeroC ICE

ZeroC IceGrid是ZeroC公司的杰作，继承了CORBA的血统，是新一代面向对象的分布式系统中间件，作为一种微服务架构，它基于RPC框架发展而来，具有良好的性能与分布式能力。

## 2、SpringCloud概述

#### 2.1 微服务中的相关概念

#### 2.2 SpringCloud的介绍

#### 2.3 SpringCloud的架构

##### 2.3.1 SpringCloud中的核心组件

**Spring Cloud**的本质是在Spring Boot的基础上，增加了一对微服务相关的规范，并对应用的上下文(Application Context) 进行了功能增强，既然SpringCloud是规范，那么久需要去实现，目前Spring Cloud规范已有Spring官方，Spring Cloud Netflix，Spring Cloud Alibaba等实现。通过组件化的方式，Spring Cloud将这些实现整合到一起构成全家桶式的微服务技术栈。

**Spring Cloud Netflix组件**

| 组件名称 | 作用           |
| -------- | -------------- |
| Eureka   | 服务注册中心   |
| Ribbon   | 客户端负载均衡 |
| Feign    | 声明式服务调用 |
| Hystrix  | 客户端容错保护 |
| Zuul     | API服务网关    |

**Spring Cloud Alibaba组件**

| 组件名称 | 作用           |
| -------- | -------------- |
| Necos    | 服务注册中心   |
| Sentinel | 客户端容错保护 |

**Spring Cloud原生及其他组件**

| 组件名称 | 作用           |
| -------- | -------------- |
| Consul   | 服务注册中心   |
| Config   | 分布式配置中心 |
| Gateway  | 服务网关       |
| Zipin    | 服务中心       |



##### 2.3.2 SpringCloud的体系结构



## 3、案例搭建



## 4、服务注册Eureka基础