> 视频  https://www.bilibili.com/video/BV1vJ41177j1?p=1



#### 课程大纲

1、ActiveMQ简介

2、ActiveMQ安装

3、原生Jms API操作ActiveMQ

4、Spring与ActiveMQ整合

5、SpringBoot与ActiveMQ整合

6、ActiveMQ消息组成与高级特性

- 事务的处理
- 消息的确认
- 死信队列

7、ActiveMQ企业面试经典问题总结



### 01、ActiveMA入门

#### 消息中间件应用场景

```
异步处理
应用解耦
流量削峰
```

##### 异步处理

场景说明：用户注册，需要执行三个业务逻辑，分表为写入用户表，发注册邮件以及注册短信

> 串行方式

> 并行方式

将注册信息希尔数据库成功后，发送邮件的痛死，发送注册短信，以上三个任务完成后，发回给客户端

> 异步处理

引入消息中间件，将部分的业务逻辑，进行异步处理，改造后的架构如下：



安装以上越多，用户的响应时间相当于是注册信息写入数据库的时间，也就是50ms，注册邮件短信血热消息队列后，直接返回，因此写入消息队列的速度很快，基本可以忽略





#### 常见的消息中间产品对比

| 特性       | ActiveMQ      | RabbitMQ | RocketMQ | Kafka  |
| ---------- | ------------- | -------- | -------- | ------ |
| 开发语言   | java          | Erlang   | Java     | Scala  |
| 单机吞吐量 | 万级          | 万级     | 10万级   | 10万级 |
| 时效性     | 毫秒级        | 微秒级   | 毫秒级   | 毫秒级 |
| 可用性     | 高 (支持主从) |          |          |        |
|            |               |          |          |        |



#### ActivMQ简介与JMS

> 什么是ActiveMQ

官网：http//activemq.apache.org/

ActiveMQ是Apache出品、最流行的，能力强劲的开源消息总线。ActiveMQ是一个完全支持JMS1.1和J2EE1.4规范的JMS Proider实现。我们在本次课程中介绍ActiveMQ的使用

> 什么是JMS？

消息中间件利用高效可靠的下行哦传递机制进行平台无关的数据交流，并给予数据通信来进行分布式系统的集成，他可以在分布式环境下扩展进程间的通信，对象消息中间件，常见的角色大致有Producer（生产者）、Consumer(消费者)。

消息队列中间件是分布式系统中的重要组件，主要用于解决应用解耦、异步消息、流量削峰等问题，实现高性能，高可用，可伸缩和最终一致。

JMS（Java Messageing Service）是Java平台上有关面向消息中间件的技术规范，它便于消息系统中的Java应用程序进行消息交换，并通过提供标准的产生，发送、接收消息的接口简化企业应用的开发。

JMS本身只定义了一些列的接口规范，是一种与长商无关的API，用来访问消息收发系统，它类似于JDBC（Java Database Connectivity）;这里，JDBC是key用来访问许多不同关系数据库的API，而JMS则提供统一于厂商无关的访问方法，以昂文消息收发服务，许多厂商都支持JMS，包括IBM的MQSeries，BEA的WebLogic JMS servic和Progress的SonicMQ，这只是几个例子，JMS使您能够通过消息收发服务（优势成为消息中介程序或路由器）从一个JMS客户机向另一个JML客户机发送消息，消息是JMS中的一种类型对象，它由两部分组成：报头和消息主体。报头由路由信息以及有关该消息的元数据组成。消息主体则携带者应用程序的数据或有效负载。

#### JMS消息模型

消息中间件异步有两种传递模式：点对点模式（P2P） 和 发布订阅模式（Pub/Sub）

- P2P（Point to Point）点对点模型 也称为Queue队列模型
- Publish/Subscribe（Pub/Sub） 发布/订阅模型（也称为Topic主题模型）

##### 点对点模型

> 点对点模型（Point to Point）：即生产者和消费者之间的消息往来



![img](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=3724840432,3330445248&fm=26&gp=0.jpg)
![img](https://upload-images.jianshu.io/upload_images/8544948-a876c08a9596d50a.png)

每个消息都被发送到特定的消息队列，接受者从队列中获取消息。队列保留着消息，指定他们被消费或者超时。

> 点对点模型的特点

- 每个消息只有一个消费者Consumer(一旦被消费,就不在消息队列中了)
- 发送者和接收者之间没有依赖性，也就是说当发送者发送了消息之后，不管接收者有没有正在运行，它不会影响到消息被发送到队列；
- 接收者成功接收消息后需向队列应答成功。

>  举例



##### 发布/订阅模型

> 发布/订阅（Publish-Subscribe）

包含三个角色：主题（Topic），发布者（Publisher）,订阅者（Subscriber），多个发布者将消息发送到topic，系统讲这些消息投递到订阅此topic的订阅者

![img](https://oscimg.oschina.net/oscnet/ca4db47583947e47425b0fc38eadacafed6.jpg)

![img](https://upload-images.jianshu.io/upload_images/8544948-cd6d95ba83f2287e.png)

发布者发送topic的消息，只有订阅了topic的订阅者才能收到消息。topic实现了发布和订阅，当你发布了一个消息，所有的订阅这个topic的服务都能收到这个消息，所以从1到N个订阅者都能得到这个消息的拷贝。

> 发布/订阅模型的特点：

- 每个消息都可以有多个消费者
- 发布和订阅者之间有时间上的依赖性（先订阅主题，再发送消息，才能接受到消息）
- 订阅者必须保持运行的状态，才能接受到发布者发布的消息



#### JMS编程API

| 要素              | 作用                                                         |
| ----------------- | ------------------------------------------------------------ |
| Destination       | 表示消息所走通道的目标定义，用来用来定义消息从发送端发出后要走的通道，而不是接收方。Destination属于管理类对象 |
| ConnectionFactory | 顾名思义，用于创建连接对象，ConnectionFactory属于管理类的对象 |
| Connection        | 连接接口，所负责的重要工作时创建Session                      |
| Session           | 会话接口，这是一个非常重要的对象，消息发送者、消息接收者以及消息对象本身，都是通过这个会话对象创建的 |
| MessageConsumer   | 消息的消费者，也就是订阅消息并处理消息的对象                 |
| MessageProducer   | 消息的生产者，也就是用来发送消息的对象                       |
| XXXMessage        | 指各种类型的消息对象，包括ByteMesage、ObjectMessage、StreamMessage和TextMessage这5种 |

- **ConnectionFactory**

  创建Connection对象的工厂，针对两种不同的jms消息模型，分别有QueueConnectionFactory和TopicConnectionFactory两种。可以通过JNDI来查找ConnectionFactory对象。客户端使用一个连接工厂对象连接到JMS服务提供者，它创建了JMS服务提供者和客户端之间的连接。JMS客户端（如发送者或接受者）会在JNDI名字空间中搜索并获取该连接。使用该连接，客户端能够与目的地通讯，往队列或主题发送/接收消息。

- **Destination**

  消息目的地，即消息生产者的消息发送目标或者说消息消费者的消息来源。对于消息生产者来说，它的Destination是某个队列（Queue）或某个主题（Topic）;对于消息消费者来说，它的Destination也是某个队列或主题（即消息来源）。Destination实际上就是两种类型的对象：Queue、Topic。

- **Connection**

  Connection表示在客户端和JMS系统之间建立的链接。Connection可以产生一个或多个Session。跟ConnectionFactory一样，Connection也有两种类型：QueueConnection和TopicConnection。

- **Session**

  Session是我们操作消息的接口。可以通过session创建生产者、消费者、消息等。Session提供了事务的功能。当我们需要使用session发送/接收多个消息时，可以将这些发送/接收动作放到一个事务中。同样，也分QueueSession和TopicSession。

- **Producer**

  MessageProducer（消息生产者）：消息生产者由Session创建，并用于将消息发送到Destination。同样，消息生产者分两种类型：QueueSender和TopicPublisher。可以调用消息生产者的方法（send或publish方法）发送消息。

- **Consumer**

  MessageConsumer（消息消费者）：消息消费者由Session创建，用于接收被发送到Destination的消息。同样有两种类型：QueueReceiver和TopicSubscriber。可分别通过session的createReceiver(Queue)或createSubscriber(Topic)来创建。当然，也可以通过session的creatDurableSubscriber方法来创建持久化的订阅者。

- **MessageListener**

  消息监听器。如果注册了消息监听器，一旦消息到达，将自动调用监听器的onMessage方法。

- **Message**

  是在消费者和生产者之间传送的对象，也就是说从一个应用程序创送到另一个应用程序。一个消息有三个主要部分：

  - 消息头（必须）：包含用于识别和为消息寻找路由的操作设置。包含如下：
    - JMSDestination ： 消息发送的目的地，主要是指Queue和Topic，由session创建而由生产者的send方法设置。
    - JMSDeliveryMode：传送模式，有持久模式和非持久模式两种。一条持久性的消息应该被传输“一次仅仅一次”，这就意味着如果JMS提供者出现故障，该消息并不会丢失，它会在服务器恢复之后再次传递。一条非持久的消息最多会传递一次，这意味着服务器出现故障，该消息将永远丢失。由session创建由消息生产者的send方法设置。
    - JMSMessageID：唯一识别每个消息的标识，由JMS消息生产者产生。由send方法设置。
    - JMSTimestamp：一个JMS Provider在调用send()方法时自动设置，它是消息被发送和消费者实际接收的时间差。由客户端设置。
    - JMSCorrelationID：用来连接到另外一个消息，典型的应用是在回复消息中连接到原消息。在大多数情况下，JMSCorrelationID用于将一条消息标记为对JMSMessageID标示的上一条消息的应答，不过，JMSCorrelationID可以是任何值，不仅仅是JMSMessageID。由客户端设置。
    - JMSReplyTo：提供本消息回复消息的目的地址,由客户端设置。
    - JMSRedelivered：如果一个客户端收到一个设置了JMSRedelivered属性的消息，则表示可能客户端曾经在早些时候收到过该消息，但并没有签收(acknowledged)。如果该消息被重新传送，JMSRedelivered=true 否则 JMSRedelivered=flase 。由JMS Provider设置。
    - JMSType：消息类型的标识符，由客户端设置。
    - JMSExpiration：消息过期时间，等于Destination的send方法中的timeToLive值加上发送时刻的GMT的时间值。如果timeToLive值等于零，则JMSExpiration被设置为零，表示该消息永不过期。如果发送后，在消息过期时间之后消息还没有被发送到目的地，则该消息被清除。由send方法设置。
    - JMSPriority：消息优先级，从0-9十个级别，0-4是普通消息，5-9是加急消息。JMS不要求JMS Provider严格按照这十个优先级发送消息，但必须保证加急消息要先于普通消息到达，默认是4级。由send方法设置
      一个消息的消息头有这些属性，我们可以按照需要对这个消息的消息进行设计，在将这个消息使用消息生产者的send（）方法发送到消息服务上。
  - 一组消息属性（可选）：包含额外的属性，支持其他提供者和用户的兼容。可以创建定制的字段和过滤器（消息选择器）。
  - 一个消息体（可选）：允许用户创建五种类型的消息：TextMessage（文本消息）、MapMessage（映射消息），BytesMessage（字节消息）、StreamMessage（流消息）和（ObjectMessage）对象消息。消息接口非常灵活，并提供了许多方式来定制消息的内容。

  

### 02、AvtiveMQ的安装

#### 2.1 安装步骤

1、安装JDK

```
建议安装jdk 1.8
```

2、下载压缩包上传到linux系统

3、解压压缩包

```
tar -zxvf apache-activemq-5.14.5-bin.tar.gz
```

4、进入解压后的apache-activemq-5.14.5的bin目录

```
cd apache-activemq-5.14.5/bin
```

5、启动activemq（执行两次：第一次生成配置信息，第二次启动）

```
./activemq start
./activemq start
```

6、停止activemq

```
./activemq stop
```

7、重启activemq

```
./activemq restart
```



##### 测试安装

浏览器访问 http://192.168.0.108:8161/

请求地址 tcp://ip61616  java代码访问消息中间件



### 03、原生JMS API操作ActiveMQ



### 04、Spring与ActiveMQ整合



### 05、SpingBoot与ActiveMQ整合

消息生产者

```xml
<!-- spring boot父工程 指定springboot的版本集群整合框架的版本 -->
<parent>
    <group>org.springframe</group>
    <>
    <version>2.0.1</version>
</parent>
<properties>
</properties>

<dependencies>
    <dempendency>
        
    </dempendency>
</dependencies>
```

