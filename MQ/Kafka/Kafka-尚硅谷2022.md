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

```bash
#!/bin/bash

case $1 in
"start")
    for i  in hadoop1002 hadoop102 hadoop104
    do
        echo "--- 去顶 ￥i kafka --- "
        ssh &i 

"stop")


"restart")

```
给脚本赋予执行权限
````bash
chmod +777

kf.sh start
 
````

注意：
- 需要在停止kafka之后 再停止zookeeper



## 2.2 Kafka命令行操作
### 2.2.1 主题命令行操作

--topic<String:topic>
--create
--delete
--alter
--list
--describe
--partitions<Integer:>

```bash
## 写一个或者写两个
bin/kafka-topics.sh --bootstrap-server hadoop102:9002
bin/kafka-topics.sh --bootstrap-server hadoop102:9002,hadoop103:9002

## 
bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --list

bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --topic first --create --partitions 1 --repication-facttor 3

bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --list

## 详细查看
bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --topic first --describe

## 修改分区 分区数 只能增加不能减少
bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --topic first --alter --partitions 3

bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --topic first --describe

## 修改副本数 不能通过命令行方式 类似以下
bin/kafka-topics.sh --bootstrap-server hadoop102:9002 --topic first --alter --repliacation-facttor 2



```

### 2.2.2 生产者命令行操作

```bash
bin/kafka-console-producer.sh  --bootstrap-server hadoop102:9002 --topic first

```
### 2.2.3 消费者命令行操作

```bash

bin/kafka-console-consummer.sh --bootstrap-server hadoop102:9002 --topic first
## 消费者 历史数据 默认不能消费

## 消费历史数据
bin/kafka-console-consummer.sh --bootstrap-server hadoop102:9002 --topic first --from-beginning



```

# 第3章 Kafka生产者
## 3.1 生产者消费发送过程
### 3.1.1 发送原理
- main线程中，创建Producer对象
- 调用send(ProducerRecord)发送数据
- Interceptions 拦截器
- 通过Serializer 序列化器，对数据进行序列化，用Kafka自带的
- Partitioner 分区器 用于发送到哪个分区
- 一个分区创建一个队列（双端队列）
- 内存中创建多个对了
- 队列的大小默认32M
- 每个批次大小默认16K
- 发送条件：批次大小达到batch.size(默认16K) 或者 达到linger.ms设置的时间 默认0毫秒
- Send线程  拉取数据
- Selector 把底层链路打通
- 进行同步
- 应答

 应答机制
- 0 生产者发送过来的数据，不需要等数据落盘应答
- 1 生产者发送过来的数据 Leader收到数据后应答
- -1(all) 生产者发送过来的数据 Leader和ISR队列里面的所有节点收齐数据后应答 -1和all等价

 应答
 - 应答成功：
 - 应答失败：不断重试
### 3.1.2 生产者重要参数列表

## 3.2 异步发送API
### 3.2.1 普通异步发送
需求： 创建
所有任务都完成之后 才能


外部数据发送到Kafka队列中，不管集群中有没有落盘

1、创建Kafka工程
kafka-sync

导入依赖
```pom
<dependency>
    <artifictId>kafka-client</artifictId>
    <group></group>
</dependency>
```

```java
public class CustomProducer{
    public static void main(String[] args) {
        //1 创建kafka生产者对象
        Properties properties = new Properties();

        //连接集群
        properties.put(ProducerConfig.BOOTSTARAP_SERVER_CONFIG,"hadoop102:9002,hadoop103:9002");
        
        //指定序列化器
        //properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringsERIALIZER")
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, sTRINGsERIALIZER.CLASS.GETnAME());
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.CLASS.GETnAME());
        new KafkaProducer<String,String>(properties); 
        
        
        
        //2 发送数据
        
        
        //3 关闭资源
    }
}
```
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
liger

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
1、coordinator 辅助消费者组的初始化和分区的分配
coordinator 阶段选择= groupid的hashcode值 % 50(__consumer_offsets的分区数量)
李世荣 groupid的hashcode值=1  1%50 =1 ,那么 __consumr_offsets 主题

ConsumerNetworkClient
Fetch.min.bytes 每批次最小抓取打下哦默认1字节
fetch.max.wait.ms一批数据



### 4.1 核心参数配置


核心参数
fetch.min.bytes 每批次最小抓取数量 默认1字节
fetch.max.wait.ms 一批数据最小值未达到的超时时间 默认500ms
fetch.max.bytes 每批次最大抓取大小 默认50m
max.poll.records 一次拉取数据返回消息的最大条数 默认500条

配置参数
bootstrap.servers
key.deserializer
value.deserializer

group.id
enable.auto.commit 默认设置为true
auto.commit.interval.ms

auto.offset.reset

offsets.topic.num.partitions
heartbeat.interval.ms
session.timout.ms
max.poll.interval.ms
fetch.max.bytes
max.poll.records

### 4.2 消费者再平衡

heartbeat.interval.ms
session.timout.ms Kafka消费者coordinator之间的超时时间，默认45ms 超过该值，该消费者被溢出，消费者组执行再平衡
max.pool.interval.ms  消费者处理消息的最大时长，默认是5分钟，超过该值，该消费者被溢出，消费者执行再平衡
partition.assignment.strategy 消费者分区分配策略，默认策略是：Range + CooperativeSticky ;
    Kafka可以同事使用多个分区分配策略，可以选择的策略包括：
    Range RoundRobin Sticky黏性 CooperativeSticky

### 4.3 指定Offset消费
kafkaConsumer.seek(topic, 1000);

### 4.4 指定消费时间消费

```java
    HashMap<TopicPartition, Long> timestampToSearch=new HashMap<>();
    timestampTToSearch.put(TopicPartition,System.currentTimeMillis(()-1*24*3600*10000));
    kafkaConsumer.offsetsForTimes(timestampToSearch);
```

### 4.5 消费者事务
    项目讲解时 再进行讲解

### 4.6 消费者如何提高吞吐量

- 增加分区
bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --alter --topic first --partitions 3

- 修改参数
  - fetch.max.bytes 
  默认Default 524288000（50M） 消费者获取服务器端一批消息最大的字节数。如果服务器端一批次的数据大于该值（50M）
仍然可以拉取回来这批数据，因此，这不是一个绝对的最大值。一批次的大小手message.max.bytes(broker config)或者
max.message.bytes(topic config)影响
  - max.poll.records 一次拉取数据返回消息的最大条数，默认是500条


## 第5章 Kafka总体

### 5.1如何提高吞吐量
如何提生吞吐量？
1、提升生产吞吐量
- buffer.memory 发送消息的缓冲区大小，默认值是32M 可以增加到64M
- batch.size 默认是16K 
- linger.ms 这个值默认是0 一般设置5-100毫秒 
- compression.type 默认是none 不压缩，但是会加大producer端的开销
2、增加分区
3、消费者提高吞吐量
- 调整fetch.max.bytes 大小 默认是50M
- 调整max.poll.records大小  默认500条
4、增加下游消费者处理能力

### 5.2数据精准一次
1、生产者角度
- acks 设置为-1  acks=-1 保证数据不丢
- 幂等性 enable.idempotence=true + 事务
2、 broker服务端角度
- 分区副本数大于等于2 -- replication-factor=2
- ISR队列里应答的最小副本数量大于等于2  min.insync.replicas=2
3、消费者
- 事务 + 手动提交offset （enable.auto.commit=false）
- 消费者输出的目的地必须支持事务MySQL Kafka
- 
### 5.3合理设置分区数
- 创建一个只有一个分区的topic
- 测试这个topic的producer吞吐量和consumer吞吐量
- 假设他们的值分别是TTp和TTc 单位可以是MB/s
- 然后假设总的目标吞吐量是Ttt 那么分区数= TTtt/min(Tp,Tc);
  例如producer吞吐量=20m/s consumer吞吐量=50m/s 期望吞吐量100m/s
  分区数= 100/20=5分区
  分区数一般设置为  3-10个
  分区数不是越多越好 也不是越少越好，需要搭建完成集群，进行压测，再调整分区个数

### 5.4 单条日志大于1M
如果大于1M  kafka会被卡死
message.max.bytes 默认1M  broker端接收每个批次消息最大值
max.request.size 默认1M 生产者发往broker每个请求消息的最大值，针对topic级别设置消息体的大小
replica.fetch.max.bytes 默认值1M 副本同步数据，每隔批次消息最大值
fetch.max.bytes 默认值Default 52428800（50M） 消费者获取服务器端一批消息最大的字节数。如果服务器端一批次的数据大于该值（50M）
仍然可以拉取回来这批数据，因此，这不是一个绝对的最大值。一批次的大小手message.max.bytes(broker config)或者
max.message.bytes(topic config)影响

### 5.5 服务器挂了
在生产环境中，如果某个Kafka节点挂掉
正常处理方法
- 1 先尝试重启一下，如果能正常启动，那直接解决
- 2 如果重启不行，考虑增加内存 增加CPU 网络带宽
- 3 如果Kafka整个节点误删，如果副本数大于等于2，可以安装服役新节点的方式重新服役一个新节点，并执行负载均衡

### 5.5 集群压力测试

1 Kafka压力测试
用Kafka官方自带的脚步 对Kafka进行压测
- 生产者压测 kafka-producer-perf-test.sh
- 消费者压测 kafka-consumer-perf-test.sh
测试环境准备
- 三台服务器
- 一台hadoop105 不需要启动 作为客户端

2 Kafka Producer 压力测试
- 创建一个test topic 设置3个分区3个副本
```
bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --create  --replication-factor 3  --partition3 --topic test
```
查看
```
bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --lsit
```

- 在/opt/module/kafka/bin目录下有这两个文件 我们来测试一下
```bash
kafka-producer-perf-test.sh --topic test  --record-size 1024 --num-records 1000000  --throughput 10000  --producer-props
bootstrap.servers=hadoop102:9092,hadoop103:9092,hadoop104:90992 batch.size=166384 linger.ms=0
```
```bash
kafka-producer-perf-test.sh --topic test  --record-size 1024 --num-records 1000000  --throughput 10000  --producer-props
bootstrap.servers=hadoop102:9092,hadoop103:9092,hadoop104:90992 batch.size=332768 linger.ms=0
```
参数说明
- record-size 1024字节 1K大小
- num-records 总共发送的条数
- throughput 10000  
- producer-props
- producer-props 后面可以配置生产者相关参数 batch.size 配置为16K  linger.ms 零毫秒  

测试：
batch.size 配置为16K  linger.ms=0 零毫秒  9.76MB/sec
batch.size 配置为32K  linger.ms=0 零毫秒  9.76MB/sec
batch.size 配置为4K  linger.ms=0 零毫秒  3.81MB/sec
不是越多越好 也不是越小越好

##### 4 调整 linger.ms时间
batch.size 配置为4K  linger.ms=50 零毫秒  3.83MB/sec  linger在生产环境中 会出现等待数据 拼凑为4k的


##### 5 compression 压缩
不采用压缩 3.83Mb/s
compression.tye=snappy 3.77M/s  
compression.tye=zstd  5.68M/s
compression.tye=gzip  5.90M/s
compression.tye=lz4   3.72M/s
日志大的时候，压缩效果

##### 6 调整缓存大小
默认生产者缓存大小32M  调整为64M
buffer.memory=33554432 默认值        
buffer.memory=67108864    3.76MB/sec

#### Kafka Consumer压力测试

```bash
kafka-consumer-perf-test.sh  bootstrap-server hadoop102 
```
3 
4 调整fetch.max. 

# 第四篇 源码解析
## 第1章 源码环境准备
### 1.1 源码下载地址
http://kafka.apache.org/downloads

### 1.2 安装JDK和Scala
Kafka是由两种语言编写的，Java和Scala
需要再Windows本地安装JDK8或者JDK8以上的版本
需要再Windows本地安装Scala2.12

### 1.3 加载源码
将kafka-3.0.0.src.tgz源码包,解压到非中文目录下，
打开IDEA File Open 源码包解压的位置

### 1.4 安装gradle
Gradle是类似于maven的代码管理工具，安卓程序管理通常是使用Gradle
IDEA自动帮你下载，下载的时间比较长 网络慢 需要1天时候，有VPN需要几分钟 

## 第2章 生产者源码

发送流程

生产者做的事情是：把外部数据发送到Kafka集群
- 首先创建main线程，
- 在main线程中，创建了一个Kafka Producer，Kafka Producer调用send(ProducerRecord)方法，进行发送数据
- 数据经过拦截器Interceptors，对数据进行加工
- 之后经过序列化器Serializer 对数据进行key和value的序列化
- 通过分区器Partitioner  把数据发往对应的缓冲区，每批次数据大小是16K
- 接下来 由Send线程发送数据，发送数据是由条件的，bathSize如果达到16k  或者lingerMS时间到了，都可以发送数据 
- 

### 2.1 初始化
#### 2.1.1 程序入口
#### 2.1.2 生产者main线程初始化

- key和value的序列化
- 拦截器  
  - 拦截器可以有多个 组成连接器链
- 控制单条日志的大小 默认是1M
- buffer memory 缓冲器大小 默认32M
- 压缩相关处理
- accumulate 缓冲区大小  默认32M
- batch.size 默认166K
- 压缩方式accumulator 
- lingerMS 与Integer最大值 取最小的 防止lingerMS的值 超过 Integer的最大值
- 内存池 Buffer.totalMemorySize
- 连接上Kafka集群地址
- 获取元数据
- 生产者 两个线程：一个main线程， 一个sender线程
- new Sender，
  - 参数 maxRequest  请求个数 默认5
  - 请求超时时间 默认30秒
- Kafka客户端对象的创建
  - 参数
  - clientId客户端ID
  - maxInfLi缓存请求的个数 默认5个
  - 重试时间  
  - RECONNECT_BACK总的重试时间
  - akcs 0 生产者发送过 不需要应答 1 leader收到，应答  -1 leader和isr队列中，所有都收到
  - 

#### 2.1.3 生产者sender线程初始化

### 2.2 发送数据到缓冲器
#### 2.2.1 发送总计流程
#### 2.2.2 分歧旋转
#### 2.2.3 发送消息大小校验
#### 2.2.4 内存池

### 2.3 sender线程发送数据


## 第3章 消费者源码
## 第4章 服务器源码
