# 企业级消息队列Kafka

> 视频地址：https://www.bilibili.com/video/BV19y4y1b7Uo
>
> 参考文档：
>
> Kafka入门(1)--kafka简介								https://blog.csdn.net/eraining/article/details/108610819
>
> Kafka入门(2)--kafka集群搭建						https://blog.csdn.net/eraining/article/details/108611234	
>
> Kafka入门(3)--kafka基础操作						https://blog.csdn.net/eraining/article/details/108611365
>
> Kafka入门(4)--kafka基准测试						https://blog.csdn.net/eraining/article/details/108611429
>
> Kafka入门(5)--Java编程操作Kafka				 https://blog.csdn.net/eraining/article/details/108611733
>
> Kafka入门(6)--kafka架构								https://blog.csdn.net/eraining/article/details/108611830
>
> Kafka入门(7)--Kafka生产者幂等性与事务	https://blog.csdn.net/eraining/article/details/108611936
>
> Kafka高级(1)--kafka分区与副本机制			https://blog.csdn.net/eraining/article/details/108627627
>
> Kafka高级(2) -- Kafka高级（High Level）API与低级（Low Level）API
>
> ​																		https://blog.csdn.net/eraining/article/details/108628204
>
> Kafka高级(3) -- Kafka监控工具Kafka-eaglehttps://blog.csdn.net/eraining/article/details/108628246
>
> Kafka高级(4) -- Kafka原理							https://blog.csdn.net/eraining/article/details/108628351
>
> Kafka高级(5) -- Kafka中数据清理（Log Deletion）https://blog.csdn.net/eraining/article/details/108628690
>
> Kafka高级(6) -- Kafka配额限速机制（Quotas）https://blog.csdn.net/eraining/article/details/108628796



# 第1章 Kafka入门

学习目标

- 了解消息队列的应用场景
- 能够搭建Kafka集群
- 能够完成生产者、消费者Java代码编写
- 理解Kafka架构，以及Kafka的重要概念
- 理解Kafka的分区和副本机制
- 了解Kafka的事务

## 1、简介

### 1.1 消息队列

#### 1.1.1 什么是消息队列

消息队列，英文名：Message Queue，经常缩写为MQ，从字面上来理解，消息队列是一种用来存储消息的队列。来看一下下面的代码：

```java
// 1. 创建一个保存字符串的队列
Queue<String> stringQueue = new LinkedList<String>();

// 2. 往消息队列中放入消息
stringQueue.offer("hello");

// 3. 从消息队列中取出消息并打印
System.out.println(stringQueue.poll());

```

上述代码，创建了一个队列，先往队列中添加了一个消息，然后又从队列中取出了一个消息。这说明了队列是可以用来存取消息的。

我们可以简单理解消息队列就是**将需要传输的数据存放在队列中**。

#### 1.1.2 消息队列中间件

消息队列中间件就是用来存储消息的软件（组件）。举个例子来理解，为了分析网站的用户行为，我们需要记录用户的访问日志。这些一条条的日志，可以看成是一条条的消息，我们可以将它们保存到消息队列中。将来有一些应用程序需要处理这些日志，就可以随时将这些消息取出来处理。

目前市面上的消息队列有很多，例如：Kafka、RabbitMQ、ActiveMQ、RocketMQ、ZeroMQ等。

##### 1.1.2.1 为什么叫Kafka呢

Kafka的架构师jay kreps非常喜欢franz kafka（弗兰兹·卡夫卡）,并且觉得kafka这个名字很酷，因此取了个和消息传递系统完全不相干的名称kafka，该名字并没有特别的含义。

「也就是说，你特别喜欢尼古拉斯赵四，将来你做一个项目，也可以把项目的名字取名为：尼古拉斯赵四，然后这个项目就火了」

#### 1.1.3 消息队列的应用场景

##### 1.1.3.1 异步处理

电商网站中，新的用户注册时，需要将用户的信息保存到数据库中，同时还需要额外发送注册的邮件通知、以及短信注册码给用户。但因为发送邮件、发送注册短信需要连接外部的服务器，需要额外等待一段时间，此时，就可以使用消息队列来进行异步处理，从而实现快速响应。

- 如图: 消息队列应用场景 - 异步处理
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915220400318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

##### 1.1.3.2 系统解耦

- 如图: 消息队列应用场景 - 系统解耦

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915220433634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

##### 1.1.3.3 流量削峰

- 如图: 消息队列应用场景 - 流量削峰
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915220459364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

##### 1.1.3.4 日志处理（大数据领域常见）

- 如图: 消息队列应用场景 - 实时日志处理
  大型电商网站（淘宝、京东、国美、苏宁…）、App（抖音、美团、滴滴等）等需要分析用户行为，要根据用户的访问行为来发现用户的喜好以及活跃情况，需要在页面上收集大量的用户访问信息。
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915220535201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

#### 1.1.4 生产者、消费者模型

Java服务器端开发的交互模型是这样的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915220727295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
Java JDBC来访问操作MySQL数据库，它的交互模型是这样的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915220748238.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
它也是一种请求响应模型，只不过它不再是基于http协议，而是基于MySQL数据库的通信协议。

而如果我们基于消息队列来编程，此时的交互模式成为：生产者、消费者模型。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915220809362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

#### 1.1.5 消息队列的两种模式

##### 1.1.5.1 点对点模式

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915220832340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
消息发送者生产消息发送到消息队列中，然后消息接收者从消息队列中取出并且消费消息。消息被消费以后，消息队列中不再有存储，所以消息接收者不可能消费到已经被消费的消息。

点对点模式特点：

- 每个消息只有一个接收者（Consumer）(即一旦被消费，消息就不再在消息队列中)
- 发送者和接收者间没有依赖性，发送者发送消息之后，不管有没有接收者在运行，都不会影响到发送者下次发送消息；
- 接收者在成功接收消息之后需向队列应答成功，以便消息队列删除当前接收的消息；

##### 1.1.5.2 发布订阅模式

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020091522085942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
发布/订阅模式特点：

- 每个消息可以有多个订阅者；
- 发布者和订阅者之间有时间上的依赖性。针对某个主题（Topic）的订阅者，它必须创建一个订阅者之后，才能消费发布者的消息。
- 为了消费消息，订阅者需要提前订阅该角色主题，并保持在线运行；

### 1.2. Kafka简介

#### 1.2.1 什么是Kafka

Kafka是由Apache软件基金会开发的一个开源流平台，由Scala和Java编写。Kafka的Apache官网是这样介绍Kakfa的。

> Apache Kafka是一个分布式流平台。一个分布式的流平台应该包含3点关键的能力：
>
>1. 发布和订阅流数据流，类似于消息队列或者是企业消息传递系统
>2. 以容错的持久化方式存储数据流
>3. 处理数据流

[官方文档](http://kafka.apache.org/documentation/#introduction)

我们重点关键三个部分的关键词：

1. Publish and subscribe：发布与订阅
2. Store：存储
3. Process：处理

#### 1.2.2 Kafka的应用场景

我们通常将Apache Kafka用在两类程序：

1. 建立实时数据管道，以可靠地在系统或应用程序之间获取数据
2. 构建实时流应用程序，以转换或响应数据流
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915221107810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
   上图，我们可以看到：
3. Producers：可以有很多的应用程序，将消息数据放入到Kafka集群中。
4. Consumers：可以有很多的应用程序，将消息数据从Kafka集群中拉取出来。
5. Connectors：Kafka的连接器可以将数据库中的数据导入到Kafka，也可以将Kafka的数据导出到数据库中。
6. Stream Processors：流处理器可以Kafka中拉取数据，也可以将数据写入到Kafka中。

#### 1.2.3 Kafka诞生背景

kafka的诞生，是为了解决linkedin的数据管道问题，起初linkedin采用了ActiveMQ来进行数据交换，大约是在2010年前后，那时的ActiveMQ还远远无法满足linkedin对数据传递系统的要求，经常由于各种缺陷而导致消息阻塞或者服务无法正常访问，为了能够解决这个问题，linkedin决定研发自己的消息传递系统，当时linkedin的首席架构师jay kreps便开始组织团队进行消息传递系统的研发。

### 1.3. Kafka的优势

前面我们了解到，消息队列中间件有很多，为什么我们要选择Kafka？

| 特性              | ActiveMQ     | RabbitMQ               | Kafka                | RocketMQ       |
| ----------------- | ------------ | ---------------------- | -------------------- | -------------- |
| 所属社区/公司     | Apache       | Mozilla Public License | Apache               | Apache/Ali     |
| 成熟度            | 成熟         | 成熟                   | 成熟                 | 比较成熟       |
| 生产者-消费者模式 | 支持         | 支持                   | 支持                 | 支持           |
| 发布-订阅         | 支持         | 支持                   | 支持                 | 支持           |
| REQUEST-REPLY     | 支持         | 支持                   | -                    | 支持           |
| API完备性         | 高           | 高                     | 高                   | 低（静态配置） |
| 多语言支持        | 支持JAVA优先 | 语言无关               | 支持，JAVA优先       | 支持           |
| 单机呑吐量        | 万级（最差） | 万级                   | **十万级**           | 十万级（最高） |
| 消息延迟          | -            | 微秒级                 | **毫秒级**           | -              |
| 可用性            | 高（主从）   | 高（主从）             | **非常高（分布式）** | 高             |
| 消息丢失          | -            | 低                     | **理论上不会丢失**   | -              |
| 消息重复          | -            | 可控制                 | 理论上会有重复       | -              |
| 事务              | 支持         | 不支持                 | 支持                 | 支持           |
| 文档的完备性      | 高           | 高                     | 高                   | 中             |
| 提供快速入门      | 有           | 有                     | 有                   | 无             |
| 首次部署难度      | -            | 低                     | 中                   | 高             |

在大数据技术领域，一些重要的组件、框架都支持Apache Kafka，不论成成熟度、社区、性能、可靠性，Kafka都是非常有竞争力的一款产品。

### 1.4 哪些公司再用Kafka

MS微软、AWS亚马逊、IBM、华为、Red Hat、浪潮

### 1.5 Kafka生态圈介绍

Apache Kafka这么多年的发展，目前也有一个较庞大的生态圈。
[Kafka生态圈官网地址](https://cwiki.apache.org/confluence/display/KAFKA/Ecosystem)

### 1.6 Kafka版本

我使用的Kafka版本为2.4.1，是2020年3月12日发布的版本。
可以注意到Kafka的版本号为：kafka_2.12-2.4.1，因为kafka主要是使用scala语言开发的，2.12为scala的版本号。http://kafka.apache.org/downloads可以查看到每个版本的发布时间。

## 2、环境搭建

### 2.1 搭建Kafka集群

1. 将Kafka的安装包上传到虚拟机，并解压

```shell
tar -xzf kafka_2.12-2.4.1.tgz
cd kafka_2.12-2.4.1
```

1. 修改 server.properties

```shell
#21行 指定broker的id
broker.id=0
#60行 指定Kafka数据的位置
log.dirs=/export/servers/kafka_2.12-2.4.1/data
#123行 zookeeper集群地址
zookeeper.connect=node1:2181,node2:2181,node3:2181
123456
```

1. 创建Kafka数据存放位置

```shell
cd /export/servers/kafka_2.12-2.4.1/
mkdir -p data
12
```

1. 将安装好的kafka复制到另外两台服务器

```shell
scp -r kafka_2.12-2.4.1/ node2:$PWD
scp -r kafka_2.12-2.4.1/ node3:$PWD
12
```

1. 修改node2，node3的server.properties配置文件

```shell
 --------------------node2-----------------------
 #21行 指定broker的id
 broker.id=1
 
 --------------------node3-------------------
 #21行 指定broker的id
 broker.id=2
1234567
```

1. 配置环境变量,**三台**服务器都需要配置

```shell
vim /etc/profile

#kafka环境变量
export KAFKA_HOME=/export/servers/kafka_2.12-2.4.1
export PATH=$PATH:$KAFKA_HOME/bin
12345
```

1. 单机启动服务

```shell
# 启动ZooKeeper
nohup bin/zookeeper-server-start.sh config/zookeeper.properties &
# 启动Kafka
nohup bin/kafka-server-start.sh config/server.properties &
# 测试Kafka集群是否启动成功，第一次启动的时候，如果不报错，就代表kafka启动成功
bin/kafka-topics.sh --bootstrap-server node1:9092 --list
123456
```

### 2.3 Kafka一键启动/关闭脚本

为了方便将来进行一键启动、关闭Kafka，我们可以编写一个shell脚本来操作。将来只要执行一次该脚本就可以快速启动/关闭Kafka。

1. 在节点1中创建 /export/onekey 目录
2. 准备slaves配置文件，用于保存要启动哪几个节点上的kafka

```shell
mkdir -p /export/onekey
cd /export/onekey
vim slave

node1
node2
node3
1234567
```

1. 编写kafka-server.sh脚本

- `如果想在任何地方都可以执行,就把这个脚本放在/usr/local/bin下`

```shell
#! /bin/bash
for i in `cat /export/onekey/slave`
do
echo "========== $i ==========" 
if [ $1 == "start" ]
then
ssh $i "source /etc/profile;export JMX_PORT=9988;nohup ${KAFKA_HOME}/bin/kafka-server-start.sh ${KAFKA_HOME}/config/server.properties &" &
fi
if [ $1 == "stop" ]
then
    ssh $i "source /etc/profile;jps |grep Kafka |cut -d' ' -f1 |xargs kill -s 9"
fi
# echo $?
done
1234567891011121314
```

1. 给kafka-server.sh配置执行权限

```shell
chmod u+x kafka-server.sh
1
```

1. 执行一键启动、一键关闭

```shell
#启动
kafka-server.sh start
#关闭
kafka-server.sh stop
1234
```

#### server.properties文件配置详解

##### kafka参数配置详情

| 参数                                           | 说明(解释)                                                   |
| ---------------------------------------------- | ------------------------------------------------------------ |
| broker.id =0                                   | 每一个broker在集群中的唯一表示，要求是正数。当该服务器的IP地址发生改变时，broker.id没有变化，则不会影响consumers的消息情况 |
| log.dirs=/data/kafka-logs                      | kafka数据的存放地址，多个地址的话用逗号分割/data/kafka-logs-1，/data/kafka-logs-2 |
| port =9092                                     | broker server服务端口                                        |
| message.max.bytes =6525000                     | 表示消息体的最大大小，单位是字节                             |
| num.network.threads =4                         | broker处理消息的最大线程数，一般情况下不需要去修改           |
| num.io.threads =8                              | broker处理磁盘IO的线程数，数值应该大于你的硬盘数             |
| background.threads =4                          | 一些后台任务处理的线程数，例如过期消息文件的删除等，一般情况下不需要去做修改 |
| queued.max.requests =500                       | 等待IO线程处理的请求队列最大数，若是等待IO的请求超过这个数值，那么会停止接受外部消息，应该是一种自我保护机制。 |
| host.name                                      | broker的主机地址，若是设置了，那么会绑定到这个地址上，若是没有，会绑定到所有的接口上，并将其中之一发送到ZK，一般不设置 |
| socket.send.buffer.bytes=100*1024              | socket的发送缓冲区，socket的调优参数SO_SNDBUFF               |
| socket.receive.buffer.bytes =100*1024          | socket的接受缓冲区，socket的调优参数SO_RCVBUFF               |
| socket.request.max.bytes =100*1024*1024        | socket请求的最大数值，防止serverOOM，message.max.bytes必然要小于socket.request.max.bytes，会被topic创建时的指定参数覆盖 |
| log.segment.bytes =1024*1024*1024              | topic的分区是以一堆segment文件存储的，这个控制每个segment的大小，会被topic创建时的指定参数覆盖 |
| log.roll.hours =24*7                           | 这个参数会在日志segment没有达到log.segment.bytes设置的大小，也会强制新建一个segment会被 topic创建时的指定参数覆盖 |
| log.cleanup.policy = delete                    | 日志清理策略选择有：delete和compact主要针对过期数据的处理，或是日志文件达到限制的额度，会被 topic创建时的指定参数覆盖 |
| log.retention.minutes=3days                    | 数据存储的最大时间超过这个时间会根据log.cleanup.policy设置的策略处理数据，也就是消费端能够多久去消费数据log.retention.bytes和log.retention.minutes任意一个达到要求，都会执行删除，会被topic创建时的指定参数覆盖 |
| log.retention.bytes=-1                         | topic每个分区的最大文件大小，一个topic的大小限制 =分区数*log.retention.bytes。-1没有大小限log.retention.bytes和log.retention.minutes任意一个达到要求，都会执行删除，会被topic创建时的指定参数覆盖 |
| log.retention.check.interval.ms=5minutes       | 文件大小检查的周期时间，是否处罚 log.cleanup.policy中设置的策略 |
| log.cleaner.enable=**false**                   | 是否开启日志压缩                                             |
| log.cleaner.threads = 2                        | 日志压缩运行的线程数                                         |
| log.cleaner.io.max.bytes.per.second=None       | 日志压缩时候处理的最大大小                                   |
| log.cleaner.dedupe.buffer.size=500*1024*1024   | 日志压缩去重时候的缓存空间，在空间允许的情况下，越大越好     |
| log.cleaner.io.buffer.size=512*1024            | 日志清理时候用到的IO块大小一般不需要修改                     |
| log.cleaner.io.buffer.load.factor =0.9         | 日志清理中hash表的扩大因子一般不需要修改                     |
| log.cleaner.backoff.ms =15000                  | 检查是否处罚日志清理的间隔                                   |
| log.cleaner.min.cleanable.ratio=0.5            | 日志清理的频率控制，越大意味着更高效的清理，同时会存在一些空间上的浪费，会被topic创建时的指定参数覆盖 |
| log.cleaner.delete.retention.ms =1day          | 对于压缩的日志保留的最长时间，也是客户端消费消息的最长时间，同log.retention.minutes的区别在于一个控制未压缩数据，一个控制压缩后的数据。会被topic创建时的指定参数覆盖 |
| log.index.size.max.bytes =10*1024*1024         | 对于segment日志的索引文件大小限制，会被topic创建时的指定参数覆盖 |
| log.index.interval.bytes =4096                 | 当执行一个fetch操作后，需要一定的空间来扫描最近的offset大小，设置越大，代表扫描速度越快，但是也更好内存，一般情况下不需要搭理这个参数 |
| log.flush.interval.messages=None               | log文件”sync”到磁盘之前累积的消息条数,因为磁盘IO操作是一个慢操作,但又是一个”数据可靠性"的必要手段,所以此参数的设置,需要在**“***\*数据可靠性\****"**与"性能"之间做必要的权衡.如果此值过大,将会导致每次"fsync"的时间较长(IO阻塞),如果此值过小,将会导致**"fsync”**的次数较多,这也意味着整体的client请求有一定的延迟.物理server故障,将会导致没有fsync的消息丢失. |
| log.flush.scheduler.interval.ms =3000          | 检查是否需要固化到硬盘的时间间隔                             |
| log.flush.interval.ms = None                   | 仅仅通过interval来控制消息的磁盘写入时机,是不足的.此参数用于控制**“fsync”**的时间间隔,如果消息量始终没有达到阀值,但是离上一次磁盘同步的时间间隔达到阀值,也将触发. |
| log.delete.delay.ms =60000                     | 文件在索引中清除后保留的时间一般不需要去修改                 |
| log.flush.offset.checkpoint.interval.ms =60000 | 控制上次固化硬盘的时间点，以便于数据恢复一般不需要去修改     |
| auto.create.topics.enable =**true**            | 是否允许自动创建topic，若是**false**，就需要通过命令创建topic |
| **default**.replication.factor =1              | 是否允许自动创建topic，若是**false**，就需要通过命令创建topic |
| num.partitions =1                              | 每个topic的分区个数，若是在topic创建时候没有指定的话会被topic创建时的指定参数覆盖 |

#### kafka中Leader,replicas参数配置详情

| 参数                                                | 说明(解释)                                                   |
| --------------------------------------------------- | ------------------------------------------------------------ |
| controller.socket.timeout.ms =30000                 | partition leader与replicas之间通讯时,socket的超时时间        |
| controller.message.queue.size=10                    | partition leader与replicas数据同步时,消息的队列尺寸          |
| replica.lag.time.max.ms =10000                      | replicas响应partition leader的最长等待时间，若是超过这个时间，就将replicas列入ISR(in-sync replicas)，并认为它是死的，不会再加入管理中 |
| replica.lag.max.messages =4000                      | 如果follower落后与leader太多,将会认为此follower[或者说partition relicas]已经失效##通常,在follower与leader通讯时,因为网络延迟或者链接断开,总会导致replicas中消息同步滞后##如果消息之后太多,leader将认为此follower网络延迟较大或者消息吞吐能力有限,将会把此replicas迁移##到其他follower中.##在broker数量较少,或者网络不足的环境中,建议提高此值. |
| replica.socket.timeout.ms=30*1000                   | follower与leader之间的socket超时时间                         |
| replica.socket.receive.buffer.bytes=64*1024         | leader复制时候的socket缓存大小                               |
| replica.fetch.max.bytes =1024*1024                  | replicas每次获取数据的最大大小                               |
| replica.fetch.wait.max.ms =500                      | replicas同leader之间通信的最大等待时间，失败了会重试         |
| replica.fetch.min.bytes =1                          | fetch的最小数据尺寸,如果leader中尚未同步的数据不足此值,将会阻塞,直到满足条件 |
| num.replica.fetchers=1                              | leader进行复制的线程数，增大这个数值会增加follower的IO       |
| replica.high.watermark.checkpoint.interval.ms =5000 | 每个replica检查是否将最高水位进行固化的频率                  |
| controlled.shutdown.enable =**false**               | 是否允许控制器关闭broker ,若是设置为**true**,会关闭所有在这个broker上的leader，并转移到其他broker |
| controlled.shutdown.max.retries =3                  | 控制器关闭的尝试次数                                         |
| controlled.shutdown.retry.backoff.ms =5000          | 每次关闭尝试的时间间隔                                       |
| leader.imbalance.per.broker.percentage =10          | leader的不平衡比例，若是超过这个数值，会对分区进行重新的平衡 |
| leader.imbalance.check.interval.seconds =300        | 检查leader是否不平衡的时间间隔                               |
| offset.metadata.max.bytes                           | 客户端保留offset信息的最大空间大小                           |

#### kafka中zookeeper参数配置详情

| 参数                                  | 说明(解释)                                                   |
| ------------------------------------- | ------------------------------------------------------------ |
| zookeeper.connect = localhost:2181    | zookeeper集群的地址，可以是多个，多个之间用逗号分割hostname1:port1,hostname2:port2,hostname3:port3 |
| zookeeper.session.timeout.ms=6000     | ZooKeeper的最大超时时间，就是心跳的间隔，若是没有反映，那么认为已经死了，不易过大 |
| zookeeper.connection.timeout.ms =6000 | ZooKeeper的连接超时时间                                      |
| zookeeper.sync.time.ms =2000          | ZooKeeper集群中leader和follower之间的同步实际那              |

### 2.2 目录结构分析

| 目录名称  | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| bin       | Kafka的所有执行脚本都在这里。例如：启动Kafka服务器、创建Topic、生产者、消费者程序等等 |
| config    | Kafka的所有配置文件                                          |
| libs      | 运行Kafka所需要的所有JAR包                                   |
| logs      | Kafka的所有日志文件，如果Kafka出现一些问题，需要到该目录中去查看异常信息 |
| site-docs | Kafka的网站帮助文件                                          |



## 3、基础操作

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915222703733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

### 3.1  创建topic

创建一个topic（主题）。Kafka中所有的消息都是保存在主题中，要生产消息到Kafka，首先必须要有一个确定的主题。

```bash
# 创建名为test的主题
bin/kafka-topics.sh --create --bootstrap-server node1:9092 --topic test
# 查看目前Kafka中的主题
bin/kafka-topics.sh --list --bootstrap-server node1:9092
1234
```

### 3.2 生产消息到Kafka

1. 使用Kafka内置的测试程序，生产一些消息到Kafka的test主题中。

```bash
bin/kafka-console-producer.sh --broker-list node1:9092 --topic test
1
```

### 3.3 从Kafka消费消息

使用下面的命令来消费 test 主题中的消息。

```bash
bin/kafka-console-consumer.sh --bootstrap-server node1:9092 --topic test --from-beginning
1
```

### 3.4 使用Kafka Tools操作Kafka

#### 3.4.1 连接Kafka集群

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915222813835.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915222824555.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915222831354.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

#### 3.4.2 创建topic

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915222844576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915222849436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915222855861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)





![在这里插入图片描述](https://img-blog.csdnimg.cn/20200918145309295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
【1】Producer 将消息进行分组分别发送到对应 leader节点；
【2】Leader 将消息写入本地 log；
【3】Followers 从 Leader pull 最新消息，写入 log后向 Leader 发送 ack确认；
【4】Leader 收到所有 ISR中的 Follower 节点的 ACK后，增加 HW，标记消息已确认全部备份完成，最后返回给 Producer 消息已提交成功；
【5】消费端从对应 Leader 节点 poll最新消息并消费，消费完成后将最新的 offset位置提交至 Topic为 _consumer_offsets的主节点中保存。

# 1. Kafka重要概念

## 简单Kafka架构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200916180730790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

## 1.1 broker

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915225027918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
一个Kafka的集群通常由多个broker组成，这样才能实现负载均衡、以及容错

- broker是无状态（Sateless）的，它们是通过ZooKeeper来维护集群状态
- 一个Kafka的broker每秒可以处理数十万次读写，每个broker都可以处理TB消息而不影响性能

## 1.2 zookeeper

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915225045981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
ZK用来管理和协调broker，并且存储了Kafka的元数据（例如：有多少topic、partition、consumer）

- ZK服务主要用于通知生产者和消费者Kafka集群中有新的broker加入、或者Kafka集群中出现故障的broker。
  PS：Kafka正在逐步想办法将ZooKeeper剥离，维护两套集群成本较高，社区提出KIP-500就是要替换掉ZooKeeper的依赖。“Kafka on Kafka”——Kafka自己来管理自己的元数据

## 1.3 producer（生产者）

- 生产者负责将数据推送给broker的topic
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/2020091814465357.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
  初始化 KafkaProducer，会创建一个后台线程 KafkaThread，会循环的判断缓存中的数据是否需要提交。同时会发送消息，主要指定 Topic和 Value，不建议指定 partition和 key防止分区分配的不均匀，扩容不方便等。然后拉取 metadata 它是在 zk中维护的，里面存储了 topic 可用的分区和正在同步的备份。分区器由两种分区策略，第一种是根据 key的 hash值，将相同的 key放在同一个分区，保证相同 key的顺序性。第二种是轮询，key不存在的时候就会轮询的向1、2、3依次存储。当确定了要发送的分区后，会发送到 RecordAccmulator缓存种，key是要发送到指定的分区，value是一个双端队列。然后会判断双端队列是否已满(batch.size)会唤醒 send 发送到 kafka broker中。同时，后端线程也会循环的判断这个双端队列是否已满。还有一种就是根据固定的时间判断是否该发送消息，从而提高 kafka的性能。当发送失败的时候，会将发送失败的消息放到双端队列的最前面。Retry默认是3，一般发送失败是网络等原因或者broker有问题，因此这个值不建议设置比较大。

## 1.4 consumer（消费者）

- 消费者负责从broker的topic中拉取数据，并自己进行处理
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200918144838360.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
  【1】Consumer 是如何协调消费分区数据的：Consumer 初始化时都会初始化一个 Consumer 协调器，发送心跳检测；Consumer 协调器与 Kafka 对应 Broker 的 group协调器进行通讯，并获取正在消费的所有 Consumer 信息，并将信息发送给主 Consumer协调器，Consumer Leader计算当前所有 Consumer应该对应消费哪个的分区数据（减少broker的压力）。
  【2】Consumer 的 offset是如何管理的：Kafka 内部维护了一个_consumer_offsets用于存储 offset；consumer 消费完数据时会将当前消费分区最新的 offset ack到 _consumer_offsets(分区有50个)中。
  【3】Consumer group 如何 rebalance的：group 协调器检测有 consumer节点挂掉，会将当前存在的 consumer 发送给 consumer 协调器，主协调器计算 consumer 应对应消费哪个分区数据。计算完成将结果发回至 group协调器。group 协调器与 consumer 协调器通信，通知对应 consumer节点是否需要消费，以及接着消费 offset位置。

## 1.5 consumer group（消费者组）

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020091522512278.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)
consumer group是kafka提供的可扩展且具有容错性的消费者机制

- 一个消费者组可以包含多个消费者
- 一个消费者组有一个唯一的ID（group Id）
- 组内的消费者一起消费主题的所有分区数据

## 1.6 分区（Partitions）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915225144354.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

- 在Kafka集群中，主题被分为多个分区

## 1.7 副本（Replicas）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915225211372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

- 副本可以确保某个服务器出现故障时，确保数据依然可用
- 在Kafka中，一般都会设计副本的个数＞1

## 1.8 主题（Topic）

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020091522524938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

- 主题是一个逻辑概念，用于生产者发布数据，消费者拉取数据
- Kafka中的主题必须要有标识符，而且是唯一的，Kafka中可以有任意数量的主题，没有数量上的限制
- 在主题中的消息是有结构的，一般一个主题包含某一类消息
- 一旦生产者发送消息到主题中，这些消息就不能被更新（更改）

## 1.9 偏移量（offset）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915225320506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VyYWluaW5n,size_16,color_FFFFFF,t_70#pic_center)

- offset记录着下一条将要发送给Consumer的消息的序号
- 默认Kafka将offset存储在ZooKeeper中
- 在一个分区中，消息是有顺序的方式存储着，每个在分区的消费都是有一个递增的id。这个就是偏移量offset
- 偏移量在分区中才是有意义的。在分区之间，offset是没有任何意义的

# 2. 消费者组

Kafka支持有多个消费者同时消费一个主题中的数据。我们接下来，给大家演示，启动两个消费者共同来消费 test 主题的数据。

1. 首先，修改生产者程序，让生产者每3秒生产1-100个数字。
2. 接下来，同时运行两个消费者。
3. 同时运行两个消费者，我们发现，只有一个消费者程序能够拉取到消息。想要让两个消费者同时消费消息，必须要给test主题，添加一个分区。

```bash
# 设置 test topic为2个分区
bin/kafka-topics.sh --zookeeper 192.168.88.100:2181 -alter --partitions 2 --topic test
12
```

1. 重新运行生产者、两个消费者程序，我们就可以看到两个消费者都可以消费Kafka Topic的数据了