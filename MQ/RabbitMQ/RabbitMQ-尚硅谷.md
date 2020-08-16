> 视频：https://www.bilibili.com/video/BV17A411e7a2?p=1

RabbitMQ 3.8.1

## 0、课程内容

##### AMQP和JMS

##### RabbitMQ介绍

##### 下载与安装

##### RabbitMQ管理界面

##### 五种消息模型

- 基本消息类型

- Work消息模型

- 订阅模型-Fanout

- 订阅模型-Direct 

  定向订阅

- 订阅模式Topic

  主题订阅

##### 持久化

##### Spring AMQP



## 1、RabbitMQ

### 1.1 搜索与商品服务的问题

假如我们完成了**商品详情**和**搜索系统**的而开发，我们思考一下，是否存在问题？

- 商品的原始数据保存在**数据库**中，增删改查都在数据库中完成
- 搜索服务数据来源是**索引库**，如果数据库商品发送变化，索引库数据是否及时更新

如果在后台修改了商品的价格，搜索页面依然是旧的价格，这样显然不对，那么该如何解决？

- **方案1**：每当后台对商品做了增删改操作，**同时**要修改**索引库**数据

  通过商品模块将某个商品的价格从99改为了199，存储到了 MYSQL数据库中

  相应的搜索服务需要将ElasticSearch数据库中的商品价格改为199

  数据一致性问题

- **方案2**：搜索服务对外提供操作接口，后台在商品增删改查后，调用接口

  用户修改价格 -> 商品服务 -> 保存到mysql数据库

  ​							商品服务-> 同时远程调用 搜索服务 -> 保存daoElasticSearch

  以上两种方式都有同一个严重问题：就是代码耦合，后台服务中需要嵌入搜索和商品页面服务，违背了微服务的`独立`原则。

  所以，会通过另外一种方式来解决这个问题：消息队列。

  商品服务 修改价格的同时，发送消息到MQ 其他服务需要订阅MQ的消息，进行相应操作

  实现了服务之间的解耦，

### 1.2 消息队列（MQ）

##### 1.2.1 什么是消息队列

  MQ全称为Message Queue, 消息队列（MQ）是一种应用程序对应用程序的通信方法。应用程序通过读写出入队列的消息（针对应用程序的数据）来通信，而无需专用连接来链接它们。消息传递指的是程序之间通过在消息中发送数据进行通信，而不是通过直接调用彼此来通信，直接调用通常是用于诸如远程过程调用的技术。排队指的是应用程序通过 队列来通信。队列的使用除去了接收和发送应用程序同时执行的要求。

数据一致性：最终一致性 并非强一致性性。

MQ的主要作用

  - 解耦
  - 异步
  - 流量削峰

- 实现高可用 高伸缩 最终一致性

消息队列是典型的：生产者、消费者模型。生产者不断向消息队列中生产消息，消费者不断的从队列中获取消息。因为消息的生产和消费都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入，这样就实现了生产者和消费者的解耦。



##### 1.2.2 AMQP和JMS

MQ是消息通信的模型，并不是具体实现，现在实现MQ的有两种主流方式：AMQP和JMS

**AMQP**

AMQP，即Advanced Message Queuing Protocol高级消息队列协议，一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件不同产品，不同的开发语言等条件的限制。Erlang中的实现有RabbitMQ等。

**JMS**

JMS即Java消息服务（Java Message Service）应用程序接口，是一个Java平台中关于面向消息中间件（MOM）的API(应用程序接口)，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。Java消息服务是一个与具体平台无关的API，绝大多数MOM提供商都对JMS提供支持。

定义了产生、发送、接收消息的规范

**两者间的区别和联系**

- JMS是定义了统一的接口，来对**消息操作进行统一**；AMQP是通过规定协议来**统一数据交互的格式**
- JMS限定了必须使用Java语言；AMQP只是协议，不规定实现方式，因此是跨语言的。
- JMS规定了两种消息模型；而AMQP的消息模型更加丰富

##### 1.2.3  常见的MQ产品

- ActiveMQ： 基于JMS APACHE
- RabbitMQ：基于AMQP协议，erlang语言开发，稳定性好
- RocketMQ：基于JMS，阿里巴巴产品，目前交由Apache基金会
- Kafka：分布式消息系统：高吞吐量、处理日志、Scala和Java编写，Apache项目

##### 1.2.4 RabbitMQ

- RabbitMQ是基于AMQP的一款消息管理系统
  - 官网： http://www.rabbitmq.com/
  - 官方教程：http://www.rabbitmq.com/getstarted.html

- RabbitMQ是一个开源的，在AMQP基础上完成的，可复用的企业消息系统
- 支持主流的操作系统，Linux、Windows、MacOS
- 多种语言开发支持：Java、Python、Ruby、.net、PHP、C/C++、Node.Js 等

##### 1.2.5 MQ三大主要功能

- 解耦
- 异步
- 削峰

##### 1.2.6 RabbitMQ特点

RabbitMQ是一个由Erlang语言开发的AMQP的开源实现。

**1、可靠性（Reliability）**

RabbitMQ使用一些机制来保证可靠性，如持久化、传输ueren、发布确认Ack。

**2、灵活的路由（Flexble Routing）**

在消息进入队列之前，通过 Exchange 来路由消息的。对于典型的路由功能，RabbitMQ 已经提供了一些内置的 Exchange 来实现。针对更复杂的路由功能，可以将多个 Exchange 绑定在一起，也通过插件机制实现自己的 Exchange 。 

**3、 消息集群(Clustering)**

 多个 RabbitMQ 服务器可以组成一个集群，形成一个逻辑 Broker 。 

**4、 高可用(Highly Available Queues)**

 队列可以在集群中的机器上进行镜像，使得在部分节点出问题的情况下队列仍然可用。

**5、多种协议(Multi-protocol)** 

RabbitMQ 支持多种消息队列协议，比如 STOMP、MQTT 等等。

**6、多语言客户端(Many Clients)** 

RabbitMQ 几乎支持所有常用语言，比如 Java、.NET、Ruby 等等。 

**7、 管理界面(Management UI)** 

RabbitMQ 提供了一个易用的用户界面，使得用户可以监控和管理消息 Broker 的许多方面。 

**8、 跟踪机制(Tracing)** 

如果消息异常，RabbitMQ 提供了消息跟踪机制，使用者可以找出发生了什么。



##### 1.2.7 RabbitMQ基本概念

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1597596236319&di=38b18e82fbb5a44e41d8ceabe779f3a7&imgtype=0&src=http%3A%2F%2F201905.oss-cn-hangzhou.aliyuncs.com%2Fcto%2F5362870-1d7a464d8ddb60b72a46b7629c6984a4.jpg)

- **Messge**

  消息，消息是不具名的，它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。
- **Publisher**
  消息的生产者，也是一个向交换器发布消息的客户端应用程序。
- **Exchange**
  交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。
  Exchange有4种类型：direct(默认)，fanout, topic, 和headers，不同类型的Exchange转发消息的策略有所区别

- **Queue**
  消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走。
- **Binding**
  绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表。
- Exchange 和Queue的绑定可以是多对多的关系。
- **Connection**
  网络连接，比如一个TCP连接。
- **Channel**
  信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的TCP连接内的虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成。因为对于操作系统来说建立和销毁TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条TCP 连接。

- **Consumer**
  消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。
- **Virtual Host**
  虚拟主机，表示一批交换器、消息队列和相关对象。虚拟主机是共享相同的身份认证和加密环境的独立服务器域。每个vhost 本质上就是一个mini 版的RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制。vhost 是AMQP 概念的基础，必须在连接时指定，RabbitMQ 默认的vhost 是/ 。
- **Broker**
  表示消息队列服务器实体


### 1.3 RabbitMQ下载和安装

#### 1.3.1 下载

###### 1、下载Erlang的rpm包

​	RabbitMQ是Erlang语言编写，索引Erlang幻想必须要有

​	注意：Erlang环境一定要与RabbitMQ版本匹配 https://www.rabbitmq.com/which-erlang.html

​	rabbitmq-server-3.7.7-1 要求 erlang版本在19.3和21.x之间

​	官网下载地址：http://www.rabbitmq.com/download.html

​	https://www.rabbitmq.com/releases/erlang/  官网只提供17.4-19.04之间的版本，

​	最新版本需要去以下地址下载

​	https://dl.bintray.com/rabbitmq-erlang/rpm/erlang/19

​	https://dl.bintray.com/rabbitmq-erlang/rpm/erlang/20

​	https://dl.bintray.com/rabbitmq-erlang/rpm/erlang/21

​	https://dl.bintray.com/rabbitmq-erlang/rpm/erlang/22

​	https://dl.bintray.com/rabbitmq-erlang/rpm/erlang/23

```bash
wget https://dl.bintray.com/rabbitmq-erlang/rpm/erlang/21/el/7/x86_64/erlang-21.3.8.16-1.el7.x86_64.rpm
```



##### 2、下载socat的rpm包

​	rabbitmq依赖于socat，所以需要下载socat

​	socat下载地址：

​	http://repo.iotti.biz/CentOS/7/x86_64/socat-1.7.3.2-5.el7.lux.x86_64.rpm

```bash
wget http://repo.iotti.biz/CentOS/7/x86_64/socat-1.7.3.2-5.el7.lux.x86_64.rpm 
```

​	socat 也可以使用yum方式安装

```bash
yum install socat
```



##### 3. 下载RabbitMQ的rpm包

​	RabbitMQ 下载地址

​	http://www.rabbitmq.com/download.html

​	rabbitmq-serer-3.8.1-1.el7.noarch.rpm

```
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.6/rabbitmq-server-3.8.6-1.el7.noarch.rpm
```



#### 1.3.2 安装

##### 1、安装Erlang、Socat、RabbitMQ

本次以 erlang-21.3.8.x rabbitmq-server-3.8.1 为例，进行安装

erlang依赖的socat-1.7.3.2

```
rpm -ivh erlang-19.3.6.13-1.el7.centos.x86_64.rpm
rpm -ivh socat-1.7.3.2-5.el7.lux.x86_64.rpm
rpm -ivh rabbitmq-server-3.8.6-1.el7.noarch.rpm
```



docker 方式安装

```
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

##### 2、启动管理插件

​	启动RabbitMQ后台管理系统插件

```bash
rabbitmq-plugins enable rabbitmq_management
```

##### 3、启动RabbitMQ

```bash
systemctl start rabbitmq-server.service
systemctl status rabbitmq-server.service
systemctl restart rabbitmq-server.service
systemctl stop tabbitmq-server.service
```

设置开机启动

```bash
systemctl enable rabbitmq-server.service
```

##### 4、查看进程

```bash
ps -ef|grep rabbitmq
```



#### 1.3.3 测试

- 关闭防火墙

  ```bash
  # 关闭防火墙
  systemctl stop firewalld.service
  
  # 查看防火墙状态
  systemctl status firewalld.service
  ```

- 在web浏览器中输入地址

  http://虚拟机ip:15627

  ```bash
  http://192.168.0.110:15627
  ```

  管理控制台端口15672 

  java程序连接RabbitMQ使用端口5672

- 输入默认账号密码

  guest账号：guest/guest

  guest用户默认不允许远程连接 需要使用虚拟机内的浏览器访问



##### 添加自定义账号

- 添加管理员账号密码

  ```
  rabbitmqctl add_user admin admin
  ```

- 分配账号角色

  ```
  rabbitmqctl set_user_tags admin administrator
  ```

- 修改密码

  ```
  rabbitmqctl change_password admin 123456
  ```

- 查看用户列表

  ```
  rabbotmqctl list_users
  ```

- 使用新账号登录

  admin账号 允许远程连接

##### 管理页面标签介绍

- overview 概览

- connections 连接情况

  无论生产者还是消费者，都需要与RabbitMQ建立连接后才可以完成消息的生产和消费，这里可以查看连接情况

- channels 通道，建立连接后，会形成通道，消息的投递获取依赖通道

- Exchanges 交换机，用来实现消息的路由

- Queues 队列，即消息队列，消息存放在队列中，等待消费，消费后被移除队列

- 端口：

  - 5267： RabbitMQ 的编程运用客户端连接端口 ：如使用java连接rabbitmq
  - 15672 ：RabbitMQ 管理界面端口
  - 25672：RabbitMQ 集群的端口

#### 1.3.4 卸载

```
rpm -qa | grep rabbitmq
rpm -e rabbitmq-server
```



### 1.4 管理界面

##### 1.4.1 添加用户

##### 1.4.2 创建Virtual Hosts虚拟主机

##### 1.4.3 设置权限



## 2、五种消息模型









## 3、Spring AMQP