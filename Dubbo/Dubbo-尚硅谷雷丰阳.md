> 地址： https://www.bilibili.com/video/BV1Gb411T7Ha

# 基础知识

## 1.  分布式基础理论

### 1.1 什么是分布式系统

《分布式系统原理与范型》定义：

“分布式系统是如果独立计算机的集合，这些计算机对于用户来说，就像单个相关系统”，分布式系统（distrivuted sytem）是建立在网络之上的软件系统。

随着或联网的反正，网站应用的不断扩大，常规的垂直应用系统架构已经无法应对，分布式服务架构以及流动计算机架构势在必行，急需一个治理系统确保架构有条不紊的演进。

### 1.2 发展演变

- 单一应用架构

  可以用多个服务器部署多套 nginx做负载均衡

- 垂直应用架构

  界面与业务逻辑分离

  应用不可能完全独立，应用之间需要交互

- 分布式架构

### 1.3 RPC

- 什么是RPC

  

## 2.  Dubbo核心概念

### 2.1 

特性

- 面向接口代理的高性能RPC调用
- 只能负载均衡
- 服务自动注册于发现
- 高度可扩展能力
- 运行期流量调度
- 

### 2.2 设计架构

## 3.  Dubbo 环境搭建

### 2.3 搭建Zookeeper

### 2.4 监控中心

Dubbo OPS

## 4. Dubbo-helloword

### 4.1 需求提出




### 4.2 工程架构

### 4.3 使用Dubbo改造

 - 1、将服务提供者注册到注册中心（暴露服务）
   	- 1.1 导入Dubbo依赖(2.6.2) 引入Dubbo客户端curator
      	- 1.2 配置服务提供者的配置文件
	- 2、让服务消费者去注册中心订阅服务服务提供者的地址
	- 3、

## 5. 监控中心

### 5.1 dubbo-admin

​	图形化的服务管理页面，安装时需要制定注册中心地址，接口从注册中心中获取到所有的提供者/消费者进行配置管理

### 5.2 dubbo-monitor-simple

简单的监控中心，用于监控服务调用等信息。

Dubbo-OPS

mva package 打包

**使用监控中心**

消费者和提供者 的配置文件：

<dubbo:monitor protpcal="registry"></dubbo:monitor>



## 6. 整合SpringBoot

创建模块

导入依赖

	- dubbo-starter
	- dubbo 其他不需要

主程序中 开启dubbo

属性覆盖策略

- 虚拟机参数配置： -Dubbo.protocol.port=20880
- XML文件配置：dubbo.xml
- Properties属性配置：dubbo.properties

## Dubbo配置

	- 启动时检查
 - 超时检查
   - 默认1000毫秒 
   - 
	- 重置次数
	- 多版本
	- 本地存根
	- 

### SpringBoot与dubbo整合的三种方式

- 导入dubbo-starter; 在application.properties中

- 保留dubbo配置文件，不在在application.properties中配置任何相关的的东西；在启动类中增加注解：@ReportResource(location="class:provider.xml")

- 使用注解API方式 配置使用配置类

  将每一个组件创建到容器中
```java

  config.MyDobboConfig{

      public ApplicationConfig applicationConfig(){

  			ApplicationConfig application =new ApplicationConfig();

  			application.setName("boot-user-service-provider");

  			return applicationConfig;

  	}

  	

  	public RegistryConfig registryConfig(){
  		RegistryConfig registry 
  	}

   	public ProtocolConfig protoConfig(){
  		ProtoConfig
  	}

  	public ServerConfig<UserService> userServiceConfig(UserService userService){
        
    }

  }
```

## Dubbo高可用

#### Zookeeper宕机与Dubbo直连

```java
@Reference(url="127.0.0.1:20882")
```

#### Dubbo集群下 负载均衡策略

- Random LoadBalance 随机
- RoundRobin 轮询
- LeastActive最小活跃数
- 一致性哈希
- 

### Dubbo服务降级

### 服务容错

