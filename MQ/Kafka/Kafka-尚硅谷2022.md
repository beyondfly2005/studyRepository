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


# 第1章 Kafka概述
## 1.1 定义
Kafka定义
Kafka传统定义
Kafka 是一个分布式的基于发布订阅模式的消息队列 Message Queue 主要用于大数据实时处理领域
发布订阅：消息的发布者不会讲消息直接发送给特定的订阅者，而是将发不得消息分为不同的类别，订阅者只接收 感兴趣的消息

Kafka的最新定义
Kafka是一个开源的分布式事件流平台Event Streaming Platform，呗熟悉俺家公司用用于高性能数据管道、流分析、数据集成和关键人物应用。


## 1.2 消息队列
目前企业中比较场景的消息队列产品主要有 Kafka ActiveMQ RabbitMQ RocketMQ
在大数据场景 主要采用 Kafka作为消息对了，在JavaEE开发中主要采用 ActiveMQ RabbitMQ RocketMQ

### 1.2.1 传统消息队列的应用场景
 传统的消息队列的主要应用场景包括：缓存/削峰、解耦和异步通信。

缓存/削峰： 有助于控制和优化数据流经过系统的速度，解决生产消息和消防消息的处理速度不一致的情况。

解耦：运行你独立地扩展或修改两边的处理过程，只要确保他们遵守统一的接口约束

异步通信： 运行用户把一个消息放入队列，但并不立即处理它，然后在需要的似乎再去处理他们

- 同步通信 死磕 每一步都需要完成
- 异步通信 只关注主要事件 完成

### 1.2.2 消息队列的两种模式
#### 点对点模式
消费者主动拉取数据，消息收到后清除消息

#### 发布订阅模式
- 可以有多个Topic主题 浏览 电子 手册 评论等
- 消费者消费数据之后 不删除数据
- 每个消费者相互独立，都可以消费到数据

Producer 生产者
Consumer 消费者

## 1.3 Kafka 基础架构

1、为扩展方便，并提高吞吐量，一个topic分为多个partition
2、配合分区的设计，提出消费者组的概念，组内每个消费者并行消费
每一个分区的数据 只能由同一个消费者 进行消费
3、为提高可用性，为每个partition增加若干副本，类似NameNode HA
副本分为Leader 和Flower之分  消费对象只能是Leader中的信息，如果Leader挂掉 会重新通过ZK选举Leader
4、Zookeeper记录节点运行的基本状态，记录Leader相关信息
ZK中记录谁是leader ，Kafka2.8.0之后也可以配置不采用ZK

Kafka 有逐渐去Zookeeper之势


# 第2章 Kafka快速入门
## 2.1 安装部署
### 2.1.1 集群规划
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

### 2.1.2 集群部署

使用 Kafka2.8.0
```bash
cd /opt/software
tar -zxvf kafka_2.12-3.0
```


borker.id=0  # 唯一标识 不能重复
log.dirs=/opt/module/kafka/datas  ## 不建议放到临时目录
zookeeper.connect=hadoop102:2181,hadoop103:2381,hadoop104:2381/kafka





### 2.1.3 集群启停脚本

## 2.2 Kafka命令行操作
### 2.2.1 主题命令行操作
### 2.2.2 生产者命令行操作
### 2.2.3 消费者命令行操作

# 第3章 Kafka生产者
## 3.1 生产者消费发送过程
### 3.1.1 发送原理
### 3.1.2 生产者重要参数列表
## 3.2 异步发送API
### 3.2.1 普通异步发送
### 3.2.2 带回调含糊的异步发送
## 3.3 同步发送API
## 3.4 生产者发送分区
### 3.4.1 分区好处
### 3.4.2 生产者发送消息的分区策略
### 3.4.3 自定义分区策略

## 3.5 生产经验—生产者如何提高吞吐量
## 3.6 生产经验—数据可靠性
## 3.7 生产经验—数据去重
## 3.8 生产经验—数据有序
## 3.9 生产经验—数据乱序


# 第4章 Kafka Broker
## 4.1 Kafka Broker工作流程
## 4.1.1 Zookeeper存储的Kafka信息
## 4.1.2 Kafka Broker总体工作流程
## 4.1.3 Broker重要参数


## 4.2 生产经验——节点服役和退役

## 4.2.1 服役新节点
## 4.2.2 退役旧节点


## 4.3 Kafka副本
## 4.3.1 副本基本信息
## 4.3.2 Leader选举流程
## 4.3.3 Leader和Follower故障处理细节
## 4.3.4 分区副本分配
## 4.3.5 生产经验——手动调整分区副本存储
## 4.3.6 生产经验——Leader Partition负载均衡
## 4.3.7 生产经验——增加副本因子

## 4.4 文件存储
## 4.4.1 文件存储机制
## 4.4.2 文件清理策略

## 4.5 高效读写数据







# 第5章 Kafka消费者
## 5.1 Kafka消费方式
## 5.2 Kafka消费者工作流程
## 5.3 消费者API
## 5.4 生产经验——分区的分配以及再平衡
### 5.4.1 Range以及再平衡
### 5.4.2 RoundRobin以及再平衡
### 5.4.3 Sticky以及再平衡
## 5.5 offset位移
### 5.5.1 offset的默认维护位置
### 5.5.2 自动提交offset
### 5.5.3 手动提交offset
### 5.5.4 指定offset消费
### 5.5.5 指定时间消费
### 5.5.6 漏消费和重复消费

## 5.6 生产经验——消费者事务
## 5.7 生产经验——数据积压(消费者如何提高吞吐量)

# 第6章 Kafka-Eagle监控
## 6.1 MySQL环境准备
## 6.2 Kafka环境准备
## 6.3 Kafka-Eagle安装
## 6.4 Kafka-Eagle页面操作

# 第7章 Kafka-Kraft模式
## 7.1 Kafka-Kraft架构
## 7.2 Kafka-Kraft集群部署
## 7.3 Kafka-Kraft集群启动停止脚本

## 4.5 高效读写数据

# 第二篇 外部系统集成
# 第1章 集成Flume
# 第2章 集成Flink

# 第3章 集成SpringBoot
SpringBoot是一个在JavaEE开发中非常常用的足迹，可以用于Kafka的生产者，也可以用于Kafka的消费者

## 3.1 SpringBoot生产者
SpringBoot可以作为生产者将消息发送到Kafka

准备
- 安装lombok插件 可以快速生成get set方法
- 创建Springboot程序项目 springboot-kafka
- 添加依赖
 - lombok
 - Spring Web
 - Messaging : Spring for Apache Kafka

ProducerController类

```java
@ResttConttroller
public class ProducerController{
    
    @Autowired
    KafkaTemplate<String, String > kafka;
    
    @RequestMapping("/message/send")
    public String data(String msg){
        kafka.send("topic-name", msg);
        
        return "ok";
    }
}

```

application.properties
```properties

## 连接kafka集群
spring.kafka.bootstrap-servers=hadoop102:90992,hadoop103:990092


## key和value的序列化
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer


```

命令行方式创建一个消费者 以进行测试
```bash
bin/kafka-console-consumer.sh --bootstrap-server hadoop102:9092 --topic first
```

## 3.2 SpringBoot消费者

项目准备
- 安装lombok插件
-

配置文件
```properties

## 指定kafka地址
spring.kafka.bootstrap-servers=hadoop102:90992,hadoop103:990092

## key和value的反序列化
spring.kafka.producer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.producer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer

# 指定消费者组的group_id
spring.kafka.consumer.group-id=atguigu
```

java
```java

public class KafkaConsumer{
    
    @KafkaListener(topic="topic-name")
    public void consumerTopic(String msg){
      System.out.println("收到消息："+msg);
                
    }
}

```


# 第4章 集成Spark
Spark 是一个在大学数据咖啡中常用的组件，可以用于Kafka的生产者，也可以用于Spark的消费者

1、 Scala环境准备
   Scala 3.8 环境搭建
1.1、安装步骤：   
- 安装JDK1.8
- 下载对用的Scala安装文件Scala-2.12.11.zip
- 解压scala-2.11.zip, 我这解压到E:\02_software
- 配置Scala的环境变量
- 
  
2、Spark环境准备
- 创建一个maven项目spark-kafka
- 在项目spark-kafka上点击右键，Add Frameworks 勾选scala
- 在main下创建scala文件夹，并右击 Mark Directory as Sources Root => 在Scala下创建
- 验证安装成功 命令行 输入 scala

3、

# 第三篇 生产调优手册
# 第1章 Kafka硬件配置选择
# 第2章 Kafka生产者
# 第3章 Kafka Broker
# 第4章 Kafka消费者
# 第5章 Kafka总体

# 第四篇 源码解析
# 第1章 源码环境准备
# 第2章 生产者源码
# 第3章 消费者源码
# 第4章 服务器源码