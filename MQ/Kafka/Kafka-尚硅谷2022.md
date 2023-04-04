# Kafka 3.x教程 
> 从入门到调优，深入全面
> https://www.bilibili.com/video/BV1vr4y1677k

## P1 课程内容

### 第一篇 入门 
- 第1章 Kafka概述
- 第2章 Kafka快速入门
- 第3章 Kafka生产者
- 第4章 Kafka Broker
- 第5章 Kafka消费者
- 第6章 Kafka-Eagle监控
- 第7章 Kafka-Kraft模式

### 第二篇 外部系统集成
- 第1章 集成Flume
- 第2章 集成Flink
- 第3章 集成SpringBoot
- 第4章 集成Spark

### 第三篇 生产调优手册
- 第1章 Kafka硬件配置选择
- 第2章 Kafka生产者
- 第3章 Kafka Broker
- 第4章 Kafka消费者
- 第5章 Kafka总体

### 第四篇 源码解析
- 第1章 源码环境准备
- 第2章 生产者源码
- 第3章 消费者源码
- 第4章 服务器源码


课程特点
- 新：针对Kafka 3.0.0版本
- 全：几乎涵盖了所有关于Kafka相关内容
- 细：安装出书标准编写，一行一行手敲代码
- 快：60页PPT动画 快速掌握技术

获取方式：
- 公众号 回复 大数据
- 老学员 谷粒学院 
- B站

技术基础要求
- 熟悉JavaSE基础
- 熟悉Linux
- 熟悉Idea开发工具
 
引出：
前端埋点记录用户浏览 点赞 收藏 评论等细信息~。
日志服务器  通过 Flume 监控文件日志变更 发送到Hadoop中
日常 Flume 采集速度 效益100m/S
618 双11活动Flume 采集速度 大于200m/s


Kafka定义
Kafka传统定义
Kafka 是一个分布式的基于发布订阅模式的消息队列 Message Queue 主要用于大数据实时处理领域
发布订阅：消息的发布者不会讲消息直接发送给特定的订阅者，而是将发不得消息分为不同的类别，订阅者只接收 感兴趣的消息

Kafka的最新定义
Kafka是一个开源的分布式事件流平台Event Streaming Platform，呗熟悉俺家公司用用于高性能数据管道、流分析、数据集成和关键人物应用。


1.2 消息队列
目前企业中比较场景的消息队列产品主要有 Kafka ActiveMQ RabbitMQ RocketMQ
在大数据场景 主要采用 Kafka作为消息对了，在JavaEE开发中主要采用 ActiveMQ RabbitMQ RocketMQ

1.2.1 传统消息队列的应用场景
 传统的消息队列的主要应用场景包括：缓存/削峰、解耦和异步通信。

缓存/削峰： 有助于控制和优化数据流经过系统的速度，解决生产消息和消防消息的处理速度不一致的情况。

解耦：运行你独立地扩展或修改两边的处理过程，只要确保他们遵守统一的接口约束

异步通信： 运行用户把一个消息放入队列，但并不立即处理它，然后在需要的似乎再去处理他们

- 同步通信 死磕 每一步都需要完成
- 异步通信 只关注主要事件 完成

1.2.3 消息队列的两种模式
 点对点模式
消费者主动拉取数据，消息收到后清除消息

发布订阅模式
- 可以有多个Topic主题 浏览 电子 手册 评论等
- 消费者消费数据之后 不删除数据
- 每个消费者相互独立，都可以消费到数据

Producer 生产者
Consumer 消费者


Kafka 基础架构

1、为扩展方便，并提高吞吐量，一个topic分为多个partition
2、配合分区的设计，提出消费者组的概念，组内每个消费者并行消费
   每一个分区的数据 只能由同一个消费者 进行消费
3、为提高可用性，为每个partition增加若干副本，类似NameNode HA
  副本分为Leader 和Flower之分  消费对象只能是Leader中的信息，如果Leader挂掉 会重新通过ZK选举Leader 
4、Zookeeper记录节点运行的基本状态，记录Leader相关信息
   ZK中记录谁是leader ，Kafka2.8.0之后也可以配置不采用ZK

Kafka 有逐渐去Zookeeper之势


## 第2章 Kafka快速入门

2.1 安装部署
2.1.1 集群规划
- Hadoop102
  - ZK
  - kafka
- Hadoop103
  - ZK
  - kafka
- Hadoop104
  - ZK
  - kafka

> 官网地址 ： kafka.apache.org/downloads

kafka是由 两种代码写的
- 生产者消费者 Java写的
- Broker是用 Scala写的

使用 Kafka2.8.0
cd /opt/software
tar -zxvf kafka_2.12-3.0



borker.id=0  # 唯一标识 不能重复
log.dirs=/opt/module/kafka/datas  ## 不建议放到临时目录
zookeeper.connect=hadoop102:2181,hadoop103:2381,hadoop104:2381/kafka




