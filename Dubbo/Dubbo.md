### SOA和RPC

#### 一 SOA架构

SOA架构



5 使用SOA架构

​	专门访问数据库的服务

​	多个子项目 或者子服务 访问数据库 不能单独访问数据库

6 实现SOA架构时，几种常用实现

​	6.1 WebService作为服务

​	6.2 Dubbo作为服务

​	6.3 DubboX作为服务

​	6.4 服务方就是web项目 调用web项目的控制器

​		使用HttpClient调用其他项目的控制器Controller

#### 二 RPC 远程过程调用

- 英文名称Remote Procedure Call Protocl 
- 中文名称：远程过程调用协议
- PRC解析：客户端通过网络远程调用远程服务器，不知道远程服务器的具体实现，只知道远程服务器提供了什么功能
- RPC最大优点 
  - 数据安全性

#### 三 Dubbo简介

​	一个分布式 高性能 透明化的RPC服务框架，提供服务自动注册、自动发现等高效服务治理方案。

3.1 Dubbo架构图

![](https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2403459333,1166926048&fm=26&gp=0.jpg)

Provider 提供方 服务发布方

Consumer 消费方 调用服务方

Container Dubbo容器 依赖于Spring容器

Registry 注册中心 当Container启动时 把所有可以提供的服务列表到Registry中进行注册。

注册中心作用：告诉Consumer提供了哪些服务，和服务方在哪里。

Monitor 监听器

虚线 异步访问 实现是同步访问

蓝色虚线 启动时完成的功能

所有的角色都可以在单独的服务器上，所有必须遵守特定的协议

红色虚线/红色实线 都是子程序运行过程中执行的功能

运行原理：

0 start 启动容器，相当于在启动Dubbo的Provider

1 启动后会去注册中心进行注册，注册所有可以通过的服务列表

2 在Consumer启动后回去注册中心获取服务列表和提供的地址 进行订阅

3 当provider 有修改后，注册中心会把消息推送给Con

​	使用了观察者模式 或者叫发布订阅模式 注册中心和Consumer之间用了观察者模式

4 根据获取到的Provider地址 真实的调用Provider中的功能

​	在Consumer方使用了代理设计模式，创建一个Provider方类的一个代理对象，通过代理对象获取Provider中真是功能，起到了保护Provider真实功能的作用

5 Monitor 监听器Consumer和Provider每隔一分钟向Monitor发送统计信息，保护 访问次数 频率等

#### 四 Dubbo注册中心

Dubbo支持的注册中心

- Zookeeper

  支持网络集群 dubbo2.3.3以上推荐使用

  稳定性依赖于Zookeeper 受限于Zookeeper

- Redis注册中心

  支持客户端双写的集群模式 性能高

  缺点：对服务器环境要求较高

- Multicast

  免中心话 不需要额外安装软件

  缺点：建议同机房（局域网内使用）

- Simple

  适用于测试环境,不支持集群

#### 五 Zookeeper

分布式协调组件

Zooker常用功能 ：

- 发布订阅功能  把
- 分布式/集群管理功能
- 使用Java语言编写 
- Zookeeper安装

#### 六 Dubbo支持的协议

#### 七 Dubbo中Provider搭建

