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
tar -zxvf apache-activemq-5.16.0-bin.tar.gz
```

4、进入解压后的apache-activemq-5.14.5的bin目录

```
cd apache-activemq-5.16.0/bin
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

8、查看activemq状态

```
./activemq status
```



#### 2.2 测试安装

浏览器访问 http://192.168.0.108:8161/    **默认用户名/密码 admin/admin**

请求地址 tcp://ip:61616  (用于java代码访问消息中间件)

如 无法正常启动，则尝试以下方法：



#### 2.3 故障处理

##### 停止防火墙

```bash
systemctl stop firewalld.service           #停止firewall
systemctl disable firewalld.service        #禁止firewall开机启动
```

##### 调试模式启动activemq

如果使用命令 ./activemq status 查看状态为 ActiveMQ not running

```bash
cd apache-activemq-5.16.0/bin
./activemq console
```

java.net.BindException: 地址已在使用

```
Failed to bind to server socket: amqp://0.0.0.0:5672?maximumConnections=1000&wireFormat.maxFrameSize=104857600 due to: java.net.BindException: 地址已在使用 (Bind failed)
```



```
# 1-- lsof -i
lsof -i:端口号
lsof -i:5672

# 2-- netstat -tunlp|grep
netstat -tunlp|grep 端口号
netstat -tunlp|grep 8161
netstat -tunlp|grep 61616
```

如果提示 -bash: lsof: 未找到命令

```
yum install lsof
```

##### lsof -i:5672 发现 rabbitmq占用了 5672端口

说明与RabbitMQ端口冲突

修改 apache-activemq-5.15.0\conf\\**activemq.xml**文件，搜索端口号5672，将其改为5673，保存后重新启动activemq即可！

```
cd conf
vim activemq.xml
```

/5672  进行搜索

改为5373 :wq 保存退出



##### 修改外网访问

./activemq status 查看状态为 ActiveMQ is running (pid '22785')

但是浏览器访问 http://192.168.0.108:8161/ 却无法访问

原因：ActiveMQ运行在Jetty上，Jetty默认设置为本机访问，不允许外网访问

```bash
cd ../conf
ll
vim jetty.xml
```



```xml
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
    <!-- the default port number for the web console -->
    <property name="host" value="127.0.0.1"/>
    <property name="port" value="8161"/>
</bean>
```

改为

```xml
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
    <!-- the default port number for the web console -->
    <property name="host" value="0.0.0.0"/>
    <property name="port" value="8161"/>
</bean>
```

重启activemq 生效

```bash
cd ../bin
./activemq restart
```



##### **注意 ：如果在服务上运行 注意网络安全问题；强烈建议修改activemq  jetty的默认密码，**



### 03、原生JMS API操作ActiveMQ

#### PTP模式（生产者）

##### (1) 引入pom依赖

```xml
<dependency>
	<groupId>org.apache.activemq</groupId>
    <artifactId>activemq-all</artifactId>
    <vrsion>5.11.2</vrsion>
</dependency>
```

##### (2) 编写生产消息的测试了QueueProducer

##### 步骤：

```properties
1、创建连接工厂
2、创建连接
3、打开连接
4、创建session
5、创建目标地址（Queue:点对点消息 ，Topic:发布订阅消息）
6、创建消息生产者
7、创建消息
8、发送消息
9、释放资源
```

##### 代码：

```java
/**
 * 点对点模式下的--- 生产者
 */
public class PTPT_Producer {
    public static void main(String[] args) throws JMSException {

        //1、创建连接工厂
        ConnectionFactory factory = new ActiveMQConnectionFactory("tcp://192.168.0.108:61616");
        //2、创建连接
        Connection connection = factory.createConnection();
        //3、打开连接
        connection.start();
        //4、创建session
        //第一个参数 是否开启事务
        //第二个参数 消息的确认机制  有四个值
        // 0=SESSION_TRANSACTED
        // 1=AUTO_ACKNOWLEDGE 自动确认
        // 2=CLIENT_ACKNOWLEDGE
        // 3=DUPS_OK_ACKNOWLEDGE
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        //5、创建目标地址（Queue:点对点消息 ，Topic:发布订阅消息）
        Queue queue = session.createQueue("queue01");
        //6、创建消息生产者
        MessageProducer producer = session.createProducer(queue);
        //7、创建消息
        TextMessage textMessage = session.createTextMessage("text message");
        //8、发送消息
        producer.send(textMessage);
        System.out.println("消息发送完毕！");
        //9、释放资源
        session.close();
        connection.close();
    }
}
```



#### PTP模式（消费者）

##### 步骤：

```properties
1、创建连接工厂
2、创建连接
3、打开连接
4、创建session
5、指定目标地址（Queue:点对点消息 ，Topic:发布订阅消息）
6、创建消息消费者
7、创建消息监听器
```

##### 代码：

```java

/**
 * 演示点对点模式 - 消息消费者（第二种方案 监听方式）
 */
public class PTP_Consumer2 {

    public static void main(String[] args) throws JMSException {
        //1、创建连接工厂
        ConnectionFactory factory = new ActiveMQConnectionFactory("tcp://192.168.0.108:61616");
        //2、创建连接
        Connection connection = factory.createConnection();
        //3、打开连接
        connection.start();
        //4、创建session
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        //5、指定目标地址（Queue:点对点消息 ，Topic:发布订阅消息）
        Queue queue = session.createQueue("queue01");
        //6、创建消息消费者
        MessageConsumer consumer = session.createConsumer(queue);
        //7、设置消息监听器接收消息
        consumer.setMessageListener(new MessageListener() {
            @Override
            public void onMessage(Message message) {
                if(message instanceof TextMessage){
                    TextMessage textMessage = (TextMessage)message;
                    try {
                        System.out.println("接收到的消息为："+textMessage.getText());
                    } catch (JMSException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
        //注意：在监听器模式下 不要关闭连接，否则接收不到消息
    }
}
```



#### Pub/Sub模式（生产者）

##### 步骤：
```properties
1、创建连接工厂
2、创建连接
3、打开连接
4、创建session
5、创建目标地址（Queue:点对点消息 ，Topic:发布订阅消息）
6、创建消息生产者
7、创建消息
8、发送消息
9、释放资源
```

##### 代码：

```java
public static void main(String[] args) throws JMSException {

        //1、创建连接工厂
        ConnectionFactory factory = new ActiveMQConnectionFactory("tcp://192.168.0.108:61616");
        //2、创建连接
        Connection connection = factory.createConnection();
        //3、打开连接
        connection.start();
        //4、创建session
        //第一个参数 是否开启事务
        //第二个参数 消息的确认机制  有四个值
        // 0=SESSION_TRANSACTED
        // 1=AUTO_ACKNOWLEDGE 自动确认
        // 2=CLIENT_ACKNOWLEDGE
        // 3=DUPS_OK_ACKNOWLEDGE
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        //5、创建目标地址（Queue:点对点消息 ，Topic:发布订阅消息）
        Topic topic = session.createTopic("topic01");
        //6、创建消息生产者
        MessageProducer producer = session.createProducer(topic);
        //7、创建消息
        TextMessage textMessage = session.createTextMessage("text message -- topic");
        //8、发送消息
        producer.send(textMessage);
        System.out.println("消息发送完毕！");
        //9、释放资源
        session.close();
        connection.close();
    }
```

#### Pub/Sub模式（消费者）

##### 步骤：
```properties
1、创建连接工厂
2、创建连接
3、打开连接
4、创建session
5、指定目标地址（Topic:发布订阅消息 / Queue:点对点消息）
6、创建消息消费者
7、创建消息监听器
```

##### 代码：

```java
public static void main(String[] args) throws JMSException {
        //1、创建连接工厂
        ConnectionFactory factory = new ActiveMQConnectionFactory("tcp://192.168.0.108:61616");
        //2、创建连接
        Connection connection = factory.createConnection();
        //3、打开连接
        connection.start();
        //4、创建session
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        //5、指定目标地址（Queue:点对点消息 ，Topic:发布订阅消息）
        Topic topic = session.createTopic("topic01");
        //6、创建消息消费者
        MessageConsumer consumer = session.createConsumer(topic);
        //7、设置消息监听器接收消息
        consumer.setMessageListener(new MessageListener() {
            @Override
            public void onMessage(Message message) {
                if(message instanceof TextMessage){
                    TextMessage textMessage = (TextMessage)message;
                    try {
                        System.out.println("接收到的消息为："+textMessage.getText());
                    } catch (JMSException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
        //注意：在监听器模式下 不要关闭连接，否则接收不到消息
    }
```


### 04、Spring与ActiveMQ整合

#### 消息生产者

##### 1、导入pom依赖

```xml
<dependency>
    <groupId>org.apache.activemq</groupId>
    <artifactId>activemq-all</artifactId>
    <version>5.11.2</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.0.2.RELEASE</version>
 </dependency>
 <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-oxm</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jms</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.jms</groupId>
            <artifactId>javax.jms-api</artifactId>
            <version>2.0.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.xbean</groupId>
            <artifactId>xbean-spring</artifactId>
            <version>3.7</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <scope>test</scope>
            <version>4.12</version>
        </dependency>
```

##### 2、Spring整合ActiveMQ配置 applicationContext-producer.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:amq="http://activemq.apache.org/schema/core"
       xmlns:jms="http://www.springframework.org/schema/jms"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/jms http://www.springframework.org/schema/jms/spring-jms.xsd
       http://activemq.apache.org/schema/core http://activemq.apache.org/schema/core/activemq-core.xsd">

    <!-- 1 创建ActiveMQ连接工厂 -->
    <amq:connectionFactory id="amqconnectionFactory"
                           userName="admi" password="admin"
                           brokerURL="tcp://192.168.0.108:61616" />
    <!-- 2 创建缓存工厂 -->
    <bean id="cachingConnectionFactory" class="org.springframework.jms.connection.CachingConnectionFactory">
        <property name="targetConnectionFactory" ref="amqconnectionFactory"></property>
        <property name="sessionCacheSize" value="10"></property>
    </bean>

    <!-- 3 创建JmsTempate(发送消息的末班工具类对象) -->
    <!-- 3.1发送Queue队列消息 -->
    <bean id="jmsQueueTemplate" class="org.springframework.jms.core.JmsTemplate">
        <!--注入缓存工厂-->
        <property name="connectionFactory" ref="cachingConnectionFactory"></property>
        <!-- 默认值-->
        <property name="pubSubDomain" value="false"></property>
    </bean>
    <!-- 3.2 发送Topic主题消息 -->
    <bean id="jmsTopicTemplate" class="org.springframework.jms.core.JmsTemplate">
        <!--注入缓存工厂-->
        <property name="connectionFactory" ref="cachingConnectionFactory"></property>
        <!-- 默认值-->
        <property name="pubSubDomain" value="true"></property>
    </bean>
</beans>
```

##### 3、编写测试类，实现消息发送

```java
/**
 * 测试Spring与ActiveMQ整合
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext-producer.xml")
public class SpringProducer {

    //点对点模式模板对象
    @Autowired
    @Qualifier("jmsQueueTemplate")
    private JmsTemplate jmsQueueTemplate;

    //发布订阅模式模板对象
    @Autowired
    @Qualifier("jmsTopicTemplate")
    private JmsTemplate jmsTopicTemplate;

    /**
     * 对点点发送 -
     */
    @Test
    public void ptpSender(){
        //参数1 队列名称
        //参数2 MessageCreator接口， 通常使用接口的匿名内部类实现
        jmsQueueTemplate.send("spring_queue", new MessageCreator() {
            //只需要返回发送消息的内容即可
            @Override
            public Message createMessage(Session session) throws JMSException {
                //文本消息
                TextMessage textMessage = session.createTextMessage("spring test message");
                return textMessage;
            }
        });
        System.out.println("消息发送已完成");
    }

    @Test
    public void psSender(){
        jmsTopicTemplate.send("spring_topic", new MessageCreator() {
            @Override
            public Message createMessage(Session session) throws JMSException {
                //文本消息
                TextMessage textMessage = session.createTextMessage("spring test message--topic");
                return textMessage;
            }
        });
        System.out.println("Topic消息发送已完成");
    }
}
```

#### 消息消费者

##### 1、导入依赖

```
同生产者
```



##### 2、编写Spring整合ActiveMQ配置文件 applicationContext-consumer.xml

```xml
    <!-- 1 连接工厂-->
    <amq:connectionFactory
        id="connectionFactory"
        brokerURL="tcp://192.168.0.108:61616"
        userName="admin"
        password="admin" />

    <!-- 2 缓存连接工厂-->
    <bean id="cachingConnectionFactory" class="org.springframework.jms.connection.CachingConnectionFactory">
        <property name="targetConnectionFactory" ref="connectionFactory"/>
        <property name="sessionCacheSize" value="10" />
    </bean>
    <!-- 3 配置消息监听组件扫描-->
    <context:component-scan base-package="com.beyondsoft.listener" />

    <!-- 4 配置监听器-->
    <!--
        destination-type 目标了下 queue 点对点 topic 发布订阅
    -->
    <jms:listener-container connection-factory="cachingConnectionFactory" destination-type="queue">
        <jms:listener destination="spring_queue" ref="queueListener" />
    </jms:listener-container>

    <jms:listener-container connection-factory="cachingConnectionFactory" destination-type="topic">
        <jms:listener destination="spring_topic" ref="topicListener" />
    </jms:listener-container>
```



##### 3、编写测试类

###### 3.1 编写点对点模式监听器

```java
/**
 * 点对点模式下的监听器
 */
@Component
public class QueueListener implements MessageListener {

    @Override
    public void onMessage(Message message) {
        if (message instanceof TextMessage) {
            TextMessage textMessage = (TextMessage) message;
            try {
                System.out.println("queue接口消息：" + textMessage.getText());
            } catch (JMSException e) {
                e.printStackTrace();
            }
        }
    }
}
```

###### 3.2编写发布订阅模式监听器

```java
/**
 * 发布订阅模式下下的监听器
 */
@Component
public class TopicListener implements MessageListener {

    @Override
    public void onMessage(Message message) {
        if (message instanceof TextMessage) {
            TextMessage textMessage = (TextMessage) message;
            try {
                System.out.println("topic接口消息：" + textMessage.getText());
            } catch (JMSException e) {
                e.printStackTrace();
            }
        }
    }
}
```

###### 3.3 编写监听启动类

```java
/**
 * 用来启动消费者监听
 */
public class SpringConsumer {

    public static void main(String[] args) throws IOException {
        //1 加载spring配置文件
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext-consumer.xml");
        //2 启动
        applicationContext.start();
        //3 阻塞方法 让程序一直处于等待状态
        System.in.read();
    }
}
```



### 05、SpingBoot与ActiveMQ整合

#### 消息生产者

##### 1、引入pim依赖

```xml
<!-- spring boot父工程 指定springboot的版本集群整合框架的版本 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.1</version>
</parent>

<properties>
    <java.version>1.8</java.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- springboot 与ActiveMQ的整合依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-activemq</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

##### 2、编写yml配置文件

```yml
server:
  port: 9001

spring:
  application:
    name: activemq-producer # 服务名称

  #springboot与activemq整合
  activemq:
    broker-url: tcp://192.168.0.108:61616
    user: admin
    password: admin
  #指定发送模式
  jms:
    pub-sub-domain: false #指定发送模式 false是点对点, true发布订阅

```

##### 3、编写启动类

```
/**
 * 生产者启动类
 */
@SpringBootApplication
public class ActivemqApplication {

    public static void main(String[] args) {
        SpringApplication.run(ActivemqApplication.class, args);
    }
}
```

##### 4、编写测试类测试消息发送

```java
/**
 * 测试springboot与ActiveMQ整合 消息生产者
 */
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = ProducerApplication.class)
public class SpringBootProducer {

    @Autowired
    JmsTemplate jmsTemplate;

    @Autowired
    JmsMessagingTemplate jmsMessagingTemplate;

    @Test
    public void ptpSender() {
        /**
         * 参数：队列的名称 或主题名称
         */
        jmsMessagingTemplate.convertAndSend("springboot_queue", "spring boot message");
    }
}
```

#### 消息消费者

##### 1、引入pim依赖

```xml
    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- springboot 与ActiveMQ的整合依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-activemq</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>    
```



##### 2、编写yml配置文件

```yml
server:
  port: 9002

spring:
  application:
    name: activemq-consumer # 服务名称

  # springboot与activemq整合
  activemq:
    broker-url: tcp://192.168.0.108:61616
    user: admin
    password: admin
  #指定发送模式
  jms:
    pub-sub-domain: false #指定发送模式 false是点对点

```

##### 3、编写启动类

```java
/**
 * 消息消费者启动类
 */
@SpringBootApplication
public class ConsumerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ConsumerApplication.class, args);
    }
}
```

##### 4、消息监听类（队列监听/主题监听）

```java
/**
 * 监听消息类（既可以用于队列监听，也可以用于主题监听）
 */
@Component
public class MessageListener {
    //**
    @JmsListener(destination = "springboot_queue")
    public void receiveMessage(Message message){
        if(message instanceof TextMessage){
            TextMessage textMessage = (TextMessage)message;
            try {
                System.out.println("接收消息："+textMessage.getText());
            } catch (JMSException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 发布订阅模式切换

###### 1、生产方

修改application.yml

```yml
spring: 
#指定发送模式
  jms:
    pub-sub-domain: false #改为true 指定发送模式 false是点对点 true 是发布订阅模式
```

###### 2、消费方

修改application.yml

```yml
spring: 
#指定发送模式
  jms:
    pub-sub-domain: false #改为true 指定发送模式 false是点对点 true 是发布订阅模式
```



#### 抽取队列/主题名称到配置文件

将队列/主题名称抽取到yml文件

```yml
# 自定义目标队列或主题名称
activemq：
    name: springboot_topic
```

使用时

```java
@Value("${activemq.name}")
private String name;
```



### 06、ActiveMQ消息组成与高级特性

#### JMS消息组成详解

##### JMS消息组成格式

整个JMS协议组成结构如下：

| 结构         | 描述                  |
| ------------ | --------------------- |
| JMS Provider | 消息中间件/消息服务器 |
| JSM Producer | 消息生产者            |
| JMS Consumer | 消息消费者            |
| JMS Message  | 消息                  |

JMS Message消息由三部分祖册

- 消息头
- 消息体
- 消息属性

##### JMS消息头

JMS消息头预定义了若干字段用于客户端与JMS提供者之间识别和发送消息，预编译头如下：

- <span style="color:red">红色</span>为重要的消息头

| 名称             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| <span style="color:red">JMSDestination</span>   | 消息发送的Destination目的地，在发送过程中有提供者设置        |
| <span style="color:red">JMSMessageID</span>     | 唯一表示发送者发送的每一条消息，这个字段是在发送过程中由提供者设置的，客户机只能在消息发送后才能确定消息的JMSMessageID |
| <span style="color:red">JMSDeliveryMode</span>  | 消息持久化模式，包含值 `DeliveryMode.PERSISTENT` 或者 `DeliveryMode.NON_PERSISTENT` |
| JMSTimestamp     | 提供者发送消息的时间，由提供者在发送过程中设置。             |
| <span style="color:red">JMSExpiration</span>    | 消息失效的时间。单位毫秒，值 0 表明消息不会过期，默认值为0   |
| <span style="color:red">JMSPriority</span>      | 消息的优先级，由提供者在发送过程中设置。优先级 0 的优先级最低，优先级 9 的优先级最高。0-4为普通消息，5-9为加急消息。ActiveMQ不保证优先级高就一定先发送，只保证了加急消息必须有限元普通消息发送。默认值为4 |
| <span style="color:red">JMSCorrelationID</span> | 通常用来链接响应消息与请求消息，由发送消息的 JMS 程序设置。  |
| JMSReplyTo       | 请求程序用它来指出回复消息应发送的地方，由发送消息的 JMS 程序设置。 |
| JMSType          | JMS 程序用它来指出消息的类型                                 |
| JMSRedelivered   | 消息的重发标志，false 代表该消息是第一次发送，true 代表该消息为重发消息 |

不过需要注意的是，在传递消息时，消息头的值由JMS提供者来设置，因此开发者使用以上SetJMSxxx() 方法分配的值就被胡烈，只有以下几个值是key由开发者设置的

JMSCorrelationID、JMSReplyTo、JMSType



##### JMS消息体

在消息体中，JMS  API定义了五种类型的消息格式，让我们可以以不同的相识发送和接收消息，并提供了对已有消息格式的兼容。不同的消息类型如下：

JMS定义了五种不同的正文格式，以及调用消息类型，允许你发送并接收不同形式的数据，提供现有消息格式的一些级别的兼容性


- TextMessage	一个字符串对象  * 
- MapMessage   一套名称-值对
- ObjectMessage  一个序列化的 Java对象
- BytesMessage  一个字节的数据流
- StreamMessage  Java原始值的数据流

###### TextMessage

发送：

```java

```

接收

```java

```


###### MapMessage   
发送：

```java

```

接收

```java

```

###### ObjectMeassage

传递的对象必须序列化

发送：

```java

```

接收

```java

```



##### JMS消息属性

我们可以给消息设置自定义属性，这些属性主要是提供给应用程序的，，消息属性分词有用，JMS API定义了一些标准属性，JMS服务提供者可以选择性的提供部分标准属性。

```java
message.setStringProperty("Property",property);	//自定义属性
message.setIntgerProperty("Property",property);
```

作用：对消息的标记过滤去重等。



#### 消息持久化

消息持久化是保证消息不丢失的重要方式！！！

ActiveMQ提供了以下三种的消息存储方式：

（1）Memory消息纯粹-基于内存的消息存储

（2）基于日志消息存储方式，KahaDB是ActiveMQ的默认日志存储方式， 它提供了容量的提升和恢复能力。

（3）基于JDBC的消息存储方式：数据存储于数据库（例如：MySQL）中

（此外还有LevelDB存储，AMQ方式--只适用于5.3版本以前）

ActiveMQ持久化流程图：

![img](http://image.mamicode.com/info/202002/20200215150830551305.png)

![img](http://image.mamicode.com/info/202002/20200215150830787618.png)



SpringBoot项目中

###### 基于内存型的消息存储方式配置

```yml
spring:
    jms:
        template:
            delivery-mode: non_persitent #非持久化配置（消息存储在内存里）
```

###### 基于日志型的消息存储方式配置

基于日志存储是springboot的默认的存储方式

```bash
#基于KahaDb的日志存储方式 日志的存放路径
cd /usr/local/apache-activemq6-xxx/data/kahadb
```



```yml
spring:
    jms:
        template:
            delivery-mode: persitent #持久化配置（消息存储在日志文件里）
```

###### 基于JDBC的消息存储方式

**1）配置application,yml连接mysql**

```
spring:
    jms:
        template:
            delivery-mode: persitent #持久化配置（消息存储在日志文件里）
```

2）修改activemq.xml

位于 apache-activemq-5.16/conf/activemq.xml

```xml
<!-- 配置数据库练级池 -->
<bean name="mysql-ds" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
    <property name="driverClassName" value="com.mysql.jdbc.Driver" />
    <property name="url" value="jdbc:mysql://192.168.66.133:2206/db_activemq" />
    <property name="username" value="root" />
    <property name="password" value="123456" />
</bean>

<!--JDBC Jdbc用于master/slave模式的数据库分享 -->
<persistenceAdapter>
    <jdbcPersistenceAdapter dataSource="#mysql-ds"/>
</persistenceAdapter>
```

3） 拷贝mysql及durid数据源的jar包到activemq的lib目录下

4）mysql中创建对应的数据库

5）重启activemq



#### 消息事务

消息事务，是按照消息传递原子性的一个重要特性，和JDBC的事务特征类似。

一个事务性发送，其中一组消息要么能够全部保证到达服务器，要么都不到达服务器。

生产者、消费者与消息服务器直接都支持事务性；

ActionMQ的是主要偏向在生产者的应用。

ActionMQ消息事务流程图：

![img]

##### 一、生产者一方事务：

###### 方式一：原生JMS事务

```java
/**
     * 事务性消息发送--方案一 采用原生JMS API
     */
    @Test
    public void sendMessageTx() {
        Session session = null;
        try {
            //获取连接工厂
            ConnectionFactory connectionFactory = jmsMessagingTemplate.getConnectionFactory();
            //创建连接
            Connection connection = connectionFactory.createConnection();
            //创建sesison  参数一师范开启事务
            session = connection.createSession(true, Session.AUTO_ACKNOWLEDGE);
            //创建消息队列
            Queue queue = session.createQueue(queue_name);
            //创建生产者
            MessageProducer producer = session.createProducer(queue);
            for (int i = 0; i < 10; i++) {
                //模拟异常
                if(i==6){
                    int num = i/0;
                }
                TextMessage textMessage = session.createTextMessage("消息——"+String.valueOf(i+1));
                producer.send(textMessage);
            }
            //一旦开启了是事务f发送，必须使用commit方法进行事务提交 否则消息无法到达MQ服务器
            session.commit();
        } catch (JMSException e) {
            e.printStackTrace();
            //消息事务的会馆
            try {
                session.rollback();
            } catch (JMSException jmsException) {
                jmsException.printStackTrace();
            }
        }
    }
```

###### 方式二、基于Spring JMS事务管理器方案

1）编写ActiveMQ配置类 ActiveMQCoinfg

```java
/**
 * ActiveMQ配置类
 */
@Configuration
public class ActiveMQConfig {

    /**
     * 添加Jms事务管理器
     * @return
     */
    @Bean
    public PlatformTransactionManager createPlatformTransactionManager(ConnectionFactory connectionFactory){
        return new JmsTransactionManager(connectionFactory);
    }
}
```

2）编写Service 并在方法上添加@Transactional

```java
@Service
public class MessageService {

    @Autowired
    private JmsMessagingTemplate jmsMessagingTemplate;

    @Value("${activemq.queue_name}")
    private String queue_name;

    @Transactional(rollbackFor = Exception.class)   //对消息发送加入事务管理，对数据库事务也同时生效
    public void sendMessage(){
        for (int i = 0; i < 10; i++) {
            //模拟异常
            if(i==6){
                int num = i/0;
            }
            jmsMessagingTemplate.convertAndSend(queue_name, "消息--"+String.valueOf(i+1));
        }
    }
}
```

3）编写测试类进行测试

```java
    @Autowired
    private MessageService messageService;
    /**
     * 事务性消息发送--方案二 基于Spring JMS事务管理器方案
     */
    @Test
    public void sendMessageTx2() {
        messageService.sendMessage();
    }
```



##### 二、消费者一方事务

###### 消费方事务机制

消费方是对单条消息的事务控制，接收成功做事务提交，在消息处理过程中，如果有逻辑业务处理失败，会进行消息事务回滚。

一旦事务回滚，MQ服务器会重发该消息，总共最多可被重发6次（默认参数设置下），如果在6次重发之后，消息仍然无法被正常消费，MQ服务器会把该失败消息存储到DLQ死信队列。

###### 代码示例

```java
/**
 * 监听消息类（既可以用于队列监听，也可以用于主题监听）
 */
@Component
public class MessageListener {

    /**
     * 接收消息的方法
     * @param message
     */
    @JmsListener(destination = "${activemq.springboot_queue1}")
    public void receiveMessage(Message message, Session session){
        if(message instanceof TextMessage){
            TextMessage textMessage = (TextMessage)message;
            try {
                System.out.println("接收消息："+textMessage.getText());
                //提交事务
                session.commit();
            } catch (JMSException e) {
                e.printStackTrace();
                //回滚事务
                try {
                    session.rollback(); //一旦事务会馆，MQ服务器会重发消息，一共可被重复6次（默认设置时）
                } catch (JMSException jmsException) {
                    jmsException.printStackTrace();
                }
            }
        }
    }
}
```



#### 消息确认机制

JMS消息只有在被确认之后，才被认为已经被成功地消费了，消息的成功消费通常包括三个阶段：客户接受消息、客户处理消息和消息被确认。**在事务性 会话中，当一个事务被提交的时候，确认自动发生。**在非事务性会话中，消息合适被确认取决于创建会话的应答模式（acknowledgement mode）,该蚕食有以下三个可选值：

| 值                        | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| Session.AUTO_ACKNOWLEDGE  | 当客户成功从reveive方法返回的时候，或者从MessageListener.onMessage方法成功返回的时候，会话自动确认客户收到的消息 |
| Sessin.CLIENT_ACKNOWLEDGE | 客户通过消息的acknowledge方法确认消息，需要注意的是，在这种模式中，确认是在会话层上进行：确认一个被消费的消息将自动去人所有已被会话消费的消息。例如，如果一个消息消费者消费了10个消息，然后确认第5个消息，那么所有10个消息都被确认。 |
| Sessin.DUPS_ACKNOWLEDGE   | 该选择知识会话迟钝确认消息的提交，如果JMS provider失败，那么可能会导致一些重复的消息，如果是重复的消息，那么JMS provider必须把消息头的JMSRedelivered自动设置为true |

注意：**消息确认机制与事务机制是冲突的**， 只能选择其中一种，所以演示消息确认前，先关闭事务

![img]()

##### 自动确认机制

SpringBoot在消费方默认自动开启事务，需要通过编写配置类的方式，关闭消费方事务

```java
/**
 * 配置类
 */
@Configuration
public class ActiveMQConfig {

    @Bean("jmsJmsListenerContainerFactory")
    public DefaultJmsListenerContainerFactory defaultJmsListenerContainerFactory(ConnectionFactory connectionFactory){
        DefaultJmsListenerContainerFactory factory = new DefaultJmsListenerContainerFactory();
        factory.setConnectionFactory(connectionFactory);
        //关闭事务
        factory.setSessionTransacted(false);
        //修改消息的确认模式为：自动确认
        factory.setSessionAcknowledgeMode(Session.AUTO_ACKNOWLEDGE);
        return factory;
    }
}
```

消费者方 在监听器里 指定containerFactory为通过配置类创建的containerFactory

```java
/**
 * 监听消息类（既可以用于队列监听，也可以用于主题监听）
 */
@Component
public class MessageListener {
    
    @JmsListener(destination = "$(activemq.queue_name)", containerFactory = "jmsJmsListenerContainerFactory")
    public void receiveMessage(Message message){
        if(message instanceof TextMessage){
            TextMessage textMessage = (TextMessage)message;
            try {
                System.out.println("接收消息："+textMessage.getText());
            } catch (JMSException e) {
                e.printStackTrace();
            }
        }
    }
}
```

##### 手动确认机制

SpringBoot对Session.CLIENT_ACKNOWLEDGE (值为2)进行了修改，使用这个值仍然会被自动确认； 而如果用原生的JMS可以使用CLIENT_ACKNOWLEDGE 2 进行手动确认


```java
/**
 * 配置类
 */
@Configuration
public class ActiveMQConfig {

    @Bean("jmsJmsListenerContainerFactory")
    public DefaultJmsListenerContainerFactory defaultJmsListenerContainerFactory(ConnectionFactory connectionFactory){
        DefaultJmsListenerContainerFactory factory = new DefaultJmsListenerContainerFactory();
        factory.setConnectionFactory(connectionFactory);
        //关闭事务
        factory.setSessionTransacted(false);
        //修改消息的确认模式为：手动确认
        factory.setSessionAcknowledgeMode(4);
        return factory;
    }
}
```

消费方监听器收到消息需要调用textMessage.acknowledge(); 进行确认

```java
@Component
public class MessageListener {
    //**
    @JmsListener(destination = "$(activemq.queue_name)", containerFactory = "jmsJmsListenerContainerFactory")
    public void receiveMessage(Message message){
        if(message instanceof TextMessage){
            TextMessage textMessage = (TextMessage)message;
            try {
                System.out.println("接收消息："+textMessage.getText());
                //手动消息确认
                textMessage.acknowledge();
            } catch (JMSException e) {
                e.printStackTrace();
            }
        }
    }
}
```



#### 消息投递方式

三种投递方式：

```bash
异步投递
延迟投递
定时投递
```

##### 异步投递

###### 1、异步投递 vs 同步投递

同步发送：

消息生产者使用持久（Persistent）传递模式发送消息的时候，Producer.send()方法会被阻塞，知道broker发送一个去人消息给生产者（ProducerAck），这个确认消息暗示broker以及成功接收到消息并把消息保存到二级存储中。

异步发送：

如果影城新能够容忍一些消息的丢失，那么可以使用异步发送。异步发送不会在收到broker的去人之前一直阻塞Producer.send方法。



想要使用异步，在broker URL中增加jms.alwaysSyncSend=false&jms.useAsyncSend=true属性

1）如果设置了alwaysSyncSend=true系统会忽略useAsyncSend设置的值都采用同步

2）当alwaysSyncSend=false时，“NON_PERSISTENT”(非持久化)，事务中的消息将使用“异步发送”

3）当alwaysSyncSend=false时，如果制定了useAsyncSend=true，“PERSISTENT”类型的消息使用异步发送。如果useAsyncSend=false，"PERSISTENT"类型的消息使用同步发送。

总结：默认情况（alwaysSyncSend=false,useAsyncSend=false），非持久化消息、事务内的消息均采用异步发送；对于持久化消息采用同步发送！！



###### 2、配置异步投递的方式

###### 原生JMS AP

1）在练级上配置

```java
new ActiveMQConnectionFactory("tcp://ip:61616?jms.useAsyncSend=true");
```

2）通过ConnectionFactory

```java
((ActiveMQConnectionFactory)connectionFactory).setUseAsyncSend(true);
```

3）通过connection

```java
((ActiveMQConnection)connection).setUseAsyncSend(true);
```

###### Spring或SpingBoot项目

通过修改JmsTemplate的默认参数实现异步或同步投递

```java
@Configuration
public class ActiveMQConfig{
    //异步投递
    @Autowired
    @Bean
    public JmsTemplate asynJmsTemplate(PooledConnectionFactory pooledConnectionFactory){
        JmsTemplate template = new JmsTemplate(pooledConnectionFactory);
        template.setExplicitQosEnabled(true);
        template.setDeliveryMode(DeliveryMode.NON_PERSISTENT);
        return template;
    }
    
    //同步投递
    @Autowired
    @Bean
    public JmsTemplate asynJmsTemplate(PooledConnectionFactory pooledConnectionFactory){
        JmsTemplate template = new JmsTemplate(pooledConnectionFactory);
        return template;
    }
}

```



###### 异步投递如何确认发送成功？

异步回调接口的两个方法 onSucess() 和 onException()

异步投递丢失消息的场景是：生产者设置UseAsyncSend=true



##### 延迟投递



##### 定时投递



#### 死信队列



### 07、AvtiveMQ企业面试经典问题

#### 问题1：ActieMQ宕机了怎么办？

1）ActiveMQ注册集群方案：Zookeeper集群+Replicatied LevelDB +AvtiveMQ集群

三台ActiveMQ服务器

官网链接：

2）集群信息概览



3）先搭建Zookeeper集群





#### 问题2：如何防止消费方消息重复消费

如果因为网络延迟等原因，MQ无法及时接收到消费方的应答，导致MQ重发，在重试过程中造成重复消费的问题

解决思路：解决消费方的幂等性问题

1）如果消费方是做数据库操作，那么可以吧消息的ID作为标的唯一主键，这样在重发的情况下，会触发主键冲突，从而避免出现脏数据；或者插入之前判断ID是否存在。

2）如果消费方不是在做数据库操作，那么可以借助第三方的应用，例如Redis，来记录消费记录，每次消息被消费完成的时候，把当前消息的ID作为key存入redis，每次消费前，先到redis查询有没有该消息的消费记录。



#### 问题3：如果防止消息丢失？

可以采取以下手段防止消息丢失：

1）在消息生产者和消费者使用事务

2）在消息方采用手动消息确认（ACK）

3）消息持久化，例如JDBC或日志



#### 问题4：什么是死信队列？

##### 什么是死信队列？

DLQ-Dead Letter QUeue，死信队列是用来保存处理失败或者过期的消息



##### 哪些消息有可能会处理失败？

在重发失败的时候，消息可能会处理失败

消息遇到以下情况会被重发：

 	- 事务开启了手动调用rollback

​	- 开启了事务但是没有提交

​	- 手动消息确认 调用了 recover方法



##### 什么样的消息会进入死信队列 

或者说 消息在什么样的情况下会进入死信队列？

当一个消息被重复超过6次（默认为6次）都失败 会进入死信队列。



##### 如何让每个队列的消息都有自己的死信队列

在默认情况下，有一个默认的死信队列存储了所有队列的失败消息

可以修改配置 让每个队列拥有自己的死信队列。



##### 消息的重发配置?

重发间隔时间、重发次数 等都可以通过配置文件来进行调整