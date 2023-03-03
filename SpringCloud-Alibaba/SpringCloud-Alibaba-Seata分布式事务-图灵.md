# Spring Alibaba Alibaba微服务分布式事务组件 Seata

> 图灵程序员 
> https://www.bilibili.com/video/BV1v5411X7C5
 
## P1 分布式事务介绍

### 1、事务简介
    事务Transation  ACID特性 原子性 一致性 隔离性 持久性
- 原子性Atomicity
- 一致性 consitency
- 隔离性 isolation
- 持久性 durability

### 2、本地事务
    Spring Transation 声明式事务



## P2
### 1、什么是Seata
Seata是一款开源的分布式锁事务解解方案，致力于提供高性能和简单易用的分布式事务服务，Seata将为用户提供了AT TCC SAGA 和XA事务模式，为用户打造一站式的分布式解决方案。
AT模式是阿里首推的模式，阿里云上有商用的版本GTS


#### 常用的分布式事务解决方案
- Seata阿里分布式事务框架
- 消息队列
- saga
- XA
- 他们有一个共同的点，都是两阶段2PC。两阶段是指完成整个分布式事务，划分为两个步骤完成。
- 时间上这四种场景的分布式是解决方案对用着这 分布式事务的四种模式 AT TCC SAGA XA

#### 分布式事务理论基础
解决分布式是，也有相对应的规范和协议，分布式事务相关的协议有2PC 3PC

2PC 两阶段提交协议
2PC 两阶段提交 Tow-H Commit
顾名思义，分为两个阶段Prepare和Commit
Prepare 提交事务请求


