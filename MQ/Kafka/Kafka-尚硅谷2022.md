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
Spark 是一个在大学数据开发中常用的组件，可以用于Kafka的生产者，也可以用于Spark的消费者

## 4.1 Spark 生产者

### 1、 Scala环境准备
   Scala 3.8 环境搭建
#### 1.1、安装步骤：   
- 安装JDK1.8
- 下载对用的Scala安装文件Scala-2.12.11.zip
- 解压scala-2.11.zip, 我这解压到E:\software
- 配置Scala的环境变量
- 验证安装成功 命令行 输入 scala

#### 1.2 Scala插件安装

  
### 2、Spark环境准备
- 创建一个maven项目spark-kafka
- 在项目spark-kafka上点击右键，Add Frameworks 勾选scala
- 在main下创建scala文件夹，并右击 Mark Directory as Sources Root => 在Scala下创建package 名为 com.beyond.spark
- 添加配置文件
```pom
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-streaming-kafka-0-10_2.12</artifactId>
  <version>3.0.0</version>
</dependency>
```
- 将log4j.properties文件添加到resources里面，就能更改打印日志的级别为error
```properties
log4j.rootLogger=error,stdout,R
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSSS}  %5p --- [%50t] %800c(line:%5L) : %m%n

log4j.appender.R=org.apache.log4j.RollingFileAppender
log4j.appender.R.File=../log/agent.log
log4j.appender.R.MaxFileSize=1024KB
log4j.appender.R.MaxBackupIndex=1


log4j.appender.R.layout=org.apache.log4j.PatternLayout
log4j.appender.R.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSSS}  %5p --- [%50t] %800c(line:%5L) : %m%n
```

SparkKafkaProducer

```scala
package com.beyond.spark

object SparkKafkaProducer {

   def main(args: Array[String]): Unit = {
        // 0 配置信息
        val properties = new Properties()
        properties.put()
        properties.put()
        properties.put()
        
        //1 创建一个生产者
        val producer = new KafkaProducer[String,String](properties)
        
        //2 发送数据
        for(i<- 1 to 5){
            producer.sen(new Producer())
        }
   }
}
```

#### 3、

## 4.2 Spark 消费者

#### 4.2.1 添加pom文件
```pom
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-streaming-kafka-0-10_2.12</artifactId>
  <version>3.0.0</version>
</dependency>
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-core_2.12</artifactId>
  <version>3.0.0</version>
</dependency>
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-streaming_2.12</artifactId>
  <version>3.0.0</version>
</dependency>
```


创建com.beyond.spark包下创建 scala Object SparkKafkaConsumer

```scala

package com.beyond.spark

object SparkKafkaProducer {

    def main(args: Array[String]): Unit = {
        // 1 初始化上下文环境
        new SparkConf().setMaster("local[*]").setAppName("spark-kafka")
        
        new StreamingContext(conf,)
   }
}

```

# 第三篇 生产调优手册
## 第1章 Kafka硬件配置选择
### 1.1 场景说明
100万日活，没人每天100条日志，每天总共的日志条数 100万*100条 = 1亿条
1亿/24小时/60分/60秒  = 1150条/每秒
每条日志大学 0.6k-2k (取值1k)
1150条/每秒 * 1K ≈ 1m/s
高峰期每秒钟： 按平时的20倍评估 1150条 * 20倍 =2300条
    每秒20Mb/s  极个别情况40M/s

### 1.2 服务器台数选择
    服务器台数 = 2 * (生产者峰值生产速率 * 副本数 /100)  +1
            = 2 *(20m/s * 2 /100) +1
            = 3 

### 1.3 磁盘选择
    kafka 安装顺序读写
    机械硬盘和固态硬盘 顺序读写速度差不多
    随机读写  固态硬盘要明显由于 机械硬盘
    1亿条 *  1k =100G
    100g * 2哥副本 * 保留3天 / 0.7（占用七成预留3成） ≈  约等于 1T  

### 1.4 内存选择
    Kafka内存 分 堆内存（Kafka内部配置） + 页缓存（服务器内存）
    堆内存 一般 生产环境配置10-15G
```bash
    vim kafka-server-start.sh
    
    export KAFKA_HEAP_OPTS="-Xmx1G -Xms1G"  
    改为
    export KAFKA_HEAP_OPTS="-Xmx10G -Xms10G"
```

kafka 内存使用情况
```bash
jps  ## 查看kafka进程

jstat -gc 2732 ls 10  ## 查看kafka jc的次数

## 主要查看 YGC


## 查看 堆内存使用情况  
## 2732为 kafka内存
jmap -heap 2732 

```

页缓存 segment (1G) 1G *25% 存入内存
3台服务器 总共 (分区数Leader(10)  *1G *25%)  = 2.5G
(分区数Leader(10)  *1G *25%)  / 3 = 1G 每台服务器设置1G


### 1.5 CPU选择

num.io.threads =8  负责写磁盘的线程数， 整个参数值要占用总核心数的50%
num.replica.fetchers=1 副本拉取线程数， 这个参数栈总核心数的 50% 的1/3
num.network.thread  数据传输线程数 ，这个参数占用总核心数的50%的2/3
预留8核心，  建议32核

### 1.6 网络选择
网络带宽 = 峰值吞吐量 ≈ 20MB/2 选择千兆网络即可
100Mbps单位是bit；10M/s单位是byte  1byte=8bit  100Mbps/8 =12.5M/s
一般百兆网卡 100Mbps 千兆网卡 1000Mbps  万兆网卡1000000Mbps



## 第2章 Kafka生产者

Updating Broker Configs
read-only 只有集群重启时 才进行更新
per-broker 每一个集群节点 
cluster-wide

### 2.1生产者核心参数配置

batch.size 只有数据积累到batch.size之后，sender才会发送数据，默认是16K
linger.ms 如果数据迟迟未达到batch.size, sender 等待linger.ms设置的时间到了之后就会发送数据，
单位ms 默认值是0ms 表示没有延迟

ACK应答：
0  生产者发送过来的数据，不需要等数据落盘应答，一般不使用
1  生产者发送过来的数据 Leader收到数据后应答
-1(all)  生产者发送过来的数据，Leader和ISR队列里面的所有节点收齐数据后应答 -1和all等价

bootstrap.servers 生产者连接


max.in.flight.requests.per.connection 预习最多没有返回ack的次数，默认为5，开启幂等性要保证数值是1-5的数字


### 2.2生产者如何提高吞吐量
buffer.memory 
batch.size 16K改为32K
lig
### 2.3数据可靠性
acks ： -1 可靠性高一些
至少一次 AtLeast

### 2.4 数据去重


### 2.5 数据有序
将数据保存在 单分区内部，有序（ 有条件的 不能乱序）；多分区 分区与分区肩 无需

### 2.6 数据乱序
enable.idempotence 是否开启幂等性 默认true 表示开启幂等性
max.in.flight.requests.per.connection 运行最多没有返回ack的次数，默认为5，开启莫等闲要保证该值是1-5的数字。

## 第3章 Kafka Broker
### 3.1 Broker核心参数配置
replica.lag.time.max.ms 
auto.leader.rebalance.enable 默认是true 一般不建议打开
log.index.interval.bytes 默认4kb kafka里面每当写入了4kb

log.retention.hours 数据默认保存的时间  默认7天
log.retention.minite
log.retention.bytes
log.cleanup.policy
num.io.threads

log.flush.interval.messages
log.flush.interval.ms 每隔多久 刷新

### 3.2 服役新节点

2 生产一个负载均衡计划
3、创建
4、
5、验证副本存储计划

### 3.3 增加分区

分区只能增加 不能减少

### 3.4 增加副本因子

1、创建topic
2、手动增加副本存储

### 3.5 手动调整分区副本存储
1、创建副本存储计划
2、执行副本存储计划
3、验证副本存储计划

### 3.6 Leader Partition负载平衡
一般建议关闭掉 ，生产环境中不建议使用
auto

### 3.7 自动创建主题
1、向一个没有提前创建five主题发送数据
```
bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --list
bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --topic five
bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --describe --topic five
 查看 five主题的分区 
 
```

auto.create.topic.enable 设置为false  一般不建议生产环境 开启默认创建主题

## 第4章 Kafka消费者
### 4.1 核心参数配置
### 4.2 消费者再平衡
### 4.3 消费者事务
### 4.4 消费者如何提高吞吐量 

## 第5章 Kafka总体
### 5.1如何提高吞吐量
### 5.2数据精准一次
### 5.3合理设置分区数
### 


# 第四篇 源码解析
## 第1章 源码环境准备
## 第2章 生产者源码
## 第3章 消费者源码
## 第4章 服务器源码
