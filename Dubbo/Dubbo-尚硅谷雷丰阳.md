> 地址： https://www.bilibili.com/video/BV1Gb411T7Ha

# 一、基础知识

## 1.  分布式基础理论

### 1.1 什么是分布式系统

《分布式系统原理与范型》定义：

“分布式系统是如果独立计算机的集合，这些计算机对于用户来说，就像单个相关系统”，分布式系统（distrivuted sytem）是建立在网络之上的软件系统。

随着或联网的反正，网站应用的不断扩大，常规的垂直应用系统架构已经无法应对，分布式服务架构以及流动计算机架构势在必行，急需一个治理系统确保架构有条不紊的演进。

### 1.2 发展演变

![](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=4130954518,1824593826&fm=15&gp=0.jpg)

- **单一应用架构**

  可以用多个服务器部署多套 nginx做负载均衡

- **垂直应用架构**

  界面与业务逻辑分离

  应用不可能完全独立，应用之间需要交互

- **分布式架构**

  按业务进行拆分，

  本地进程内通讯  -> RPC远程过程调用

- **流动计算架构**

  随着微服务的越来越多，服务器节点随之增多，如果将更多的服务器资源分配给压力更大的业务业务服务系统

### 1.3 RPC

![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=962964185,1176401230&fm=11&gp=0.jpg)

- 什么是RPC

- RPC的两个核心模块：通讯、序列化

- RPC框架有哪些

  阿里dubbo、谷歌gRPC、HSF(Heigh Speed Service Framework)

## 2.  Dubbo核心概念

### 2.1 简介

##### Dubbo的发展历史

​	阿里巴巴开源的

​	11年 开源到github

​	14年10月 2.4.11版本

​	停更

​	当当网 dubboX

​	网易考拉dubbokey

​	17年 

​	18年dubbox与dubbo合并 并发布了2.6

​	18年2月15日 dubbo开源贡献给Apache组织



##### **Dubbo的优良特性**

- 面向接口代理的高性能RPC调用

- 智能负载均衡

- 服务自动注册与发现

- 高度可扩展能力

- 运行期流量调度

  通过配置路由规则，轻松实现灰度发布

- 可视化的服务治理与运维

### 2.2 设计架构

## 3.  Dubbo 环境搭建

![](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2070965499,2376100404&fm=26&gp=0.jpg)

### 3.1 windows安装Zookeeper

复制conf/zoo_example.cfg 并命名为 zoo.cfg

zkServer.cmd

测试zkClient.cmd

```
get /
create -e /atguigu
ls /
get /atguigu
```



### 3.2 windows安装监控中心

安装监控台

application.

```
mvn clean package
```

localhost:7001

Dubbo OPS

### 3.3 Linux安装Zookeeper

### 3.4 Linux安装监控中心



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

5 监控中心

### 5.1 dubbo-admin

​	图形化的服务管理页面，安装时需要制定注册中心地址，接口从注册中心中获取到所有的提供者/消费者进行配置管理

### 5.2 dubbo-monitor-simple

简单的监控中心，用于监控服务调用等信息。

Dubbo-OPS

mva package 打包

**使用监控中心**

消费者和提供者 的配置文件：

```xml
<dubbo:monitor protpcal="registry"></dubbo:monitor>
```



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

# 二、Dubbo配置

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

# 三、Dubbo高可用

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



# 四、DUbbo原理

