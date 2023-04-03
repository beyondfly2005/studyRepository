###### 1.1.2消息队列

- 消息Message

- 队列Queue

- 消息队列MQ

  消息+队列 保存消息的队列，消息的传输过程中的容器，主要提供生产、消费接口供外部调用做数据的存储和获取。

###### 1.1.3消息队列的分类

​	MQ主要分为两类：点对点p2p，

- 点对点P2P

- 发布订阅

- 

###### 1.1.4. p2p和发布订阅MQ比较

- 共同点

消息生产者发布消息发送到queue中

- 不同点

p2p模型包括 消息队列 发送者 接收者

pub/Sub包含：消息队列主题，发布者

###### 1.1.5. 消息队列的使用场景

- 解耦 各个系统之间通过消息系统统一的接口交换数据， 无需了解彼此的存在
- **冗余** 部分消息系统具有消息持久化能力，可以规避消息处理前丢失的风险
- 扩展 消息系统是统一的数据接口，各系统可独立扩展
- 峰值处理能力 消息系统可顶住峰值流量，业务系统可更加处理能力从消息系统中获取并处理对应量的请求
- 可恢复性
- 异步通信

###### 1.1.6 常见的消息系统

- RabbitMQ Erlang编写，支持多协议AMQP、XMPP、SMTP、STOP。支出负载均可、数据持久化、同时支出点对点和发布/订阅模式
- Redis
- ZeroMQ轻量级 
- ActiveMQ JMS实现，支持点对点 支持持久化 XA分布式事务
- Kafka/Jafka 高性能跨语言的分布式发布/订阅消息系统，数据持久化，全分布式，同事支持在线和离线处理
- MetaQ/RocketMQ 纯java实现，发布订阅消息系统，支持本地事务和XA分布式事务



##### 1.2. Kafaka 简介

###### 1.2.1 简介

​	Kafka是一个分布式的发布-订阅消息系统，它最初由LinkedIn（领英）公司发布。使用Scala语音变形，于2010年12月份开业，成为Apache的顶级项目，Kafka是一个高吞吐量的、持久化的、分布式发布订阅消息系统。它主要用于处理活跃live的数据()登录、浏览、点击、分享、喜欢等用户行为产生的数据）。

> 官方文档
>
> http://kafka.apachecn.org
>
> http://kafka.apache.org

三大特点：

- 高吞吐量

- 持久化

- 分布式

  基于分布式的扩展和容错机制；kafka的数据都会复制到几台服务器上，当一台故障失效时

1.2.2. 设计目标

- 高吞吐率 每秒100万太哦消息的读写
- 消息持久化
- 完全分布式
- 同时适应在线流处理和离线批处理

1.2.3. kafka核心概念

​	一个MQ需要哪些部分？生产、消费、消息类别、存储等等。

Kafka服务：

- Topic： 主题
- Broker 消息服务器代理
- Partition：Topic物理上的分组，一个topic在braoker被分为一个或多个partition，分期在创建topic的时候指定。
- Message：消息

Kafka服务相关

- Producer 生产者
- Consumer 消费者
- Zookeeper 协调kafka的正常运行

##### 2.Kafka的分布式安装

2.0.服务器准备

​	三台服务器

2.1.版本下载

​	安装包 http://archive.apache.org/dist/kafka/1.1.1/

​	源码包

2.2.安装过程

​	安装包上传到服务器

```bash

cd /usr/local

# 上传安装包

# 解压
tar -zxvf /opt/soft/kafka_2.1.1-1.1.1.tgz -C /opt/apps
# 重命名
mv /opt/apps/kafka_2.1.1-1.1.1/ /opt/apps/kafka
# 添加环境变量
vim /etc/profile.d/hadoop-etc.sh

export KAFKA_HOME=/opt/apps/kafka
export PATH=$PATH:$KAFKA_HOME/bin

# 编译
source /etc/profile.d/hadoop-etc.sh

# 配置
修改$KAFKA_HOME/config/server.properties
broker.id ## 当前kafka实例的id 必须为整数 一个集群中不可重复
log.dirs=/opt/data/kafka ##生产到kafka中的数据存储的目录，目录需要树洞创建
zookeeper.connect=bigdata01:2181,bigdata02:2181,bigdata03:2181/kafka ## kafka数据在zk中的存储目录

# 同步到其他集群
scp -r kafka/ root@bigdata02:/opt/app/
```