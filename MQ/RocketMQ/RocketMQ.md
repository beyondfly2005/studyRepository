> 视频地址：https://www.bilibili.com/video/BV1J4411272o

# 第一章：核心功能

## 1. MQ介绍

### 1.1 为什么要用MQ/MQ作用

MQ=Message Queue 消息队列是一种先进先出的数据结构

其应用场景主要包含以下三个方面

- 应用解耦

- 系统的耦合度越高，容错性就越低。以电商应用为例，用户创建订单后，如果耦合调用库存系统，物理系统‘支付系统，任何一个子系统出现了故障或者因为升级等原因暂停不可用，

- 流量削峰

  应用系统如果遇到系统请求量瞬间猛增，有可能会将系统压垮。有了消息队列可以将大量请求缓存起来，分散到很长一段

  将系统请求缓存到MQ中，系统从缓存中逐个处理请求

  一般情况，为了保证系统的稳定性，如果系统负载超过阈值，就会阻止用户请求，这会影响用户铁艺，而如果使用消息队列将请求缓存起来，等待系统

- 数据分发

  通过消息队列可以让数据在多个系统更加进行流通。数据的产生方不需要关系谁来使用数据，只要将数据发送到消息队列，数据使用方直接在消息队列中直接获取数据即可

### 1.2  MQ的优点和缺点

优点：解耦、削峰、数据分发

缺点：

- 系统的可用性降低

  系统引入外部依赖越多，系统稳定性越差。一旦MQ宕机，就会对业务造成影响。

  如何保证MQ的高可用？

- 系统复杂度提高

  MQ的加入增加了系统的复杂度，以前系统间是同步的远程调用，现在是通过MQ进行异步调用。

  如何保障消息没有被重复消费？怎么处理消息丢失情况？怎么保障消息传递的顺序性？

- 一致性问题

  A系统处理完业务，通过MQ给BCD是哪个系统发消息数据，如果B系统、C系统处理成功，D系统处理调用失败；如何保证消息数据处理的一致性

####  注意事项

#### 1.3各种MQ产品比较

常见的MQ包括Kafka、ActiveMQ、RabbitMQ、RocketMQ

| 特性       | ActiveMQ                                                     | RabbitMQ                                                     | RocketMQ                 | kafka                                                        |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| 开发语言   | java                                                         | erlang                                                       | java                     | scala                                                        |
| 单机吞吐量 | 万级                                                         | 万级                                                         | 10万级                   | 10万级                                                       |
| 时效性     | ms级                                                         | us级                                                         | ms级                     | ms级                                                         |
| 可用性     | 高（主从结构）                                               | 高（主从架构）                                               | 非常高（分布式架构）     | 非常高（分布式架构）                                         |
| 功能性     | 城市的产品，在很多公司得到应用；有较多的文档；各种系统支持较高 | 基于erlang开发，所以并发能力强，性能及其好，延时低，管理界面较为丰富；二次开发困难 | MQ功能比较完备，扩展性佳 | 只支持主要的MQ功能，像一些消息的查询、消息的回溯等功能没有提供，毕竟是为大数据准备的，在大数据领域应用广 |
| 其他       |                                                              |                                                              | 顺序性、事务性支持的较好 |                                                              |
|            |                                                              |                                                              |                          |                                                              |





## 2. RocketMQ环境搭建

RocketMQ是阿里巴巴2016年开园的MQ中间件，使用java语言开发，在阿里内部，RocketMQ支撑了例如双11等高并发常见的消息流转，能够处理万亿级别的消息。

### 2.1 准备工作

#### 2.1.1 下载RocketMQ

RocketMQ最新版本：4.7.0 

*快速开始*

http://rocketmq.apache.org/docs/quick-start/

*下载列表*

http://rocketmq.apache.org/dowloading/releases/

*下载地址*

https://www.apache.org/dyn/closer.cgi?path=rocketmq/4.7.0/rocketmq-all-4.7.0-bin-release.zip

*清华大学下载链接*

https://mirrors.tuna.tsinghua.edu.cn/apache/rocketmq/4.7.0/rocketmq-all-4.7.0-bin-release.zip

#### 2.1.2 环境要求

- Linux 64位系统
- JDK 1.8 64位
- 源码安装需要使用Maven 3.2X
- 下载源码需要使用 GIT
- 二进制文件安装需要使用zip解压工具

```bash
# yum 安装unzip工具
yum install -y unzip zip
```



### 2.2 安装RocketMQ

```bash

# 上传RocketMQ压缩包

# 解压压缩包
unzip rocketmq-all-4.7.0-bin-release.zip 

# 目录介绍
# benchemark demo 示例
# bin 可执行文件
# conf 配置文件
# lib 依赖的jar包
mv rocketmq-all-4.7.0-bin-release/ rocketmq/

```



### 2.3 启动RocketMQ

1. 启动NameServer

```bash
# 启动NameServer
nohup sh bin/mqnamesrv &
# 使用以上命名会报错 nohup: 忽略输入并把输出追加到"nohup.out"
# 需要改用如下命名启动
nohup sh bin/mqnamesrv >/dev/null 2>&1 &
nohup sh bin/mqnamesrv >/dev/null 2>&1 &
#或者 
cd bin
nohup sh mqnamesrv > /dev/null 2>&1 &
# 查看启动日志
tail -f ~/logs/rocketmqlogs/namesrv.log
```

2. 启动Broker

```bash
# 启动Broker
nohup sh bin/mqbroker -n localhost:9876 &
nohup sh bin/mqbroker -n localhost:9876 >/dev/null 2>&1 &
# 查看启动日志
tail -f ~/logs/rocketmqlogs/broker.log

# 用jps命令查看java进程
jps
# java进程列表中 看不到mqbroker 也看不到broker.log日志


```

3. 问题描述

   RocketMQ默认的虚拟机内存较大，启动Broker如果因为内存不足失败，需要便捷如下两个配置文件，修改JVM内存大小

```bash
vim runbroker.sh
vim runserver.sh
```

​	参考设置

```bash
# 将
JAVA_OPT="${JAVA_OPT} -server -Xms8g -Xmx8g -Xmn4g "
# 改为
JAVA_OPT="${JAVA_OPT} -server -Xms8g -Xmx8g -Xmn4g "
# 或参考设置如下
JAVA_OPT="${JAVA_OPT} -server -Xms256m -Xmx256m -Xmn256m --XX:MetaspaceSize=128 -XX:MaxMetaspaceSize=320M "
```

### 2.4 关闭RocketMQ

```bash
# 1 关闭NameServer
sh bin/mqshutdown namesrv
# 2 关闭Broker
sh bin/mqshutdown broker

```

### 2.5 测试RocketMQ

#### 2.5.1 发送消息

```bash
# 设置环境变量
export NAMESRV_ADDR=localhost:9876
# 使用安装包的Demo发送消息
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
```
#### 2.5.2 接收消息

```bash
# 设置环境变量
export NAMESRV_ADDR=localhost:9876
# 接收消息
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
```



## 3. RocketMQ高可用集群搭建

### 3.1 集群各角色介绍

- Producer : 消息的发送者；例如：发信人
- Consumer：消息的接收者；例如：收信人
- Broker：暂存和传输消息；例如：邮局
- NameServer：管理Broker：例如：邮局的管理机构
- Topic：区分消息的种类；一个发送者可以发送消息给一个或者多个Topic；一个消息的接收组合可以订阅一个或者多个Topic消息
- Message Queue：相当于Topic的分区；用于并行发送和接收消息

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592848045407&di=9c7dcd6ccd4742906e75c3274737b026&imgtype=0&src=http%3A%2F%2Fspider.nosdn.127.net%2Fed9f12482124e95923bf8dabab8d76be.jpeg)

### 3.2 集群搭建方式

#### 3.2.1 集群特点

- NameServer 是一个几乎无状态的节点，可以集群部署，节点之间无任何信息同步。
- Broker部署相对复杂，Broker分别为Master与Slave，一个Master可以对应多个Slave，但是一个Slave只能对应一个Maser；Master与Slave的多赢关系通过制度相同的BrokerName，不同的BrokerId来订阅，BrokerId为0 表示Master，非0表示Slave。Master也可以部署多个，每个Broker与NameServer集群中的所有节点建立长连接，定时注册Topic信息到所有的NameServer.
- Producer 与NameServer集群中的其中一个节点（随机选择）建立连接，定期从NameServer取Topic路由信息，并向提供Topic服务的Master建立长连接，且定时向Master发送心跳。Producer完全无状态，可以集群部署。
- Consumer与NameServer集群中的其中一个节点（随机选择）建立连接，定期从NameServer取Topic路由信息，并向提供Topic服务的Master、Slave建立长连接，且定时向Master、Slave发送心跳，Consumer既可以从Master订阅消息，也可以从Slave订阅消息，订阅规则由Broker决定。

#### 3.2.2 集群模式

**1）单Master模式**

这种方式风险较大，一旦Broker重启或者宕机是，会导致整个服务不可用，不建议线上系统使用与，可以本地测试或开发使用，不是集群模式。

**2）多Master模式**

一个集群无Slave，全是Master，例如2各Master或者3各Master，这种模式的优缺点如下：

- 优点：配置简单，单个Master宕机或重启维护队应用无影响， 在磁盘配置为RAID10时，及时机器宕机不可恢复情况下，有RAID磁盘非常苛刻，消息也不会丢失（异步刷盘丢失少量信息，同步刷盘一条也不会丢），性能最高；
- 缺点：单台机器宕机期间，这个机器上违背消费的消息在机器恢复之前不可订阅，消息实时性会受到影响。

**3）多Master多Slaver模式(同步)**

每个Master配置一个或多个Slave，有多对Master-Slaver，HA采用异步复制方式，主备有短暂消息延迟（毫秒级），这种模式的优缺点如下：

- 优点：即使磁盘损坏，消息丢失的非常少，且消息实时性不会受到影响，同时Master宕机后，消费者仍然可以从Slave消费，而且此时过程对应用透明，不需要人工干预，性能同多Master模式集合一样；
- 缺点：Master宕机，磁盘损坏情况下回丢失少量消息

**4) 多Master多Slaver模式(异步)**

每个Master配置一个或多个Slave，有多对Master-Slaver，HA采用同步双写方式，即只有主备都写成功，才向应用返回成功， 这种模式的优缺点如下：

- 优点：数据与服务都唔单点故障，Master宕机情况下，消息无延迟，服务可用性与数据可用性都非常高；
- 缺点：性能比异步复制模式略低（大约10%左右）；发送单个消息的RT会略高，且不去版本在主节点宕机后，备机不能自动切换为主机。

### 3.3 双主双从集群搭建

#### 3.3.1 总体架构

消息高可用采用2m-2s （同步双写）方式

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592851247698&di=4049e18bb78f162566eba252dd70b442&imgtype=0&src=http%3A%2F%2Fimages3.10qianwan.com%2F10qianwan%2F20180731%2Fb_0_201807310409538312.jpg)

#### 3.3.2 集群工作流程

1. 启动NameServer，NameSer起来后监听端口，等待Broker、Producer、Consumer连上了，相当于一个路由控制中心
2. Broker启动，跟所有的NameServer保存长连接， 定时发送心跳包。心跳包中包含当前Broker信息（IP+端口等）以及存储所有Topic信息，注册成功后，NameServer集群中就有Topic跟Broker的映射关系
3. 收发消息之前，先创建Topic，创建Topci时



3.3.3 服务器环境

3.3.4 Host添加信息

3.3.5 防火墙配置

3.3.6 环境变量配置

3.3.7创建消息痛处路径

3.3.8 broker配置文件

3.3.9 修改启动脚本文件

3.3.10  服务启动

3.3.11 查看进程张贴

3.3.12 查看日志

### 3.4 mqadmin管理工具

### 3.5 集群监控平台



## 4. 各种消息发送样例

- 创建maven工程
- 导入MQ客户端依赖

```xml
<dependency>
	<groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client</artifactId>
    <version>4.4.0</version>
</dependency>
```

- 消息发送者步骤分析

```properties
1.创建消息生产者producer，并指定生产者组名
2.指定Nameserver地址
3.启动producer
4.创建消息对象，指定主题Topic、Tag和消息体
5.发送消息
6.关闭生产者Producer
```

- 消息接收者步骤分析

```properties
1.创建消费者Consumer，指定消费者组名
2.指定Nameserver地址
3.订阅主题Topic和Tag
4.设置回调函数，处理消息
5.启动消费者consumer
```



### 4.1 基本样例

#### 4.1.1 消息发送

##### 1）发送同步消息



##### 2） 发送异步消息

##### 3）单项发送消息

#### 4.1.2 消费消息

##### 1）负载均衡模式

##### 2）广播模式

### 4.2 同步消息

异步偶像

单向消息

顺序消息

批量消息



# 第二章：核心功能

项目背景介绍

功能分析

项目环境搭建

SpringBootDubbo

Zookeeper

MQ

下单功能 保证各个服务的数据一致

确认订单功能，通过消息进行数据分发

整体联调



# 第三章：高级功能和源码分析

## 高级功能部分

### 消息存储和发送

### 消息存储结构

###  刷盘机制

​	同步刷盘

​	异步刷盘

### 消息的同步和异步复制

### 负载均衡

## 源码分析

### 路由中心NameServer

### 消息生产者

### 消息存储

###  消息消费者

