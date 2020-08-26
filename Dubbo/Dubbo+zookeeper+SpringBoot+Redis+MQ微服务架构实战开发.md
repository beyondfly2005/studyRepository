# Dubbo+zookeeper+SpringBoot+Redis+MQ微服务架构实战开发



> 视频地址：https://www.bilibili.com/video/BV1VJ411g7Zq?t=53

## P1 架构演变-分析AllInOne的使用场景及问题

单体项目：所有的功能模块都放在一个项目上

开发流程：开发—>测试—>上线

​					团队开发—>SVN->

业务

![](https://picb.zhimg.com/80/v2-575aba2fab1cb8f54dfb3f000cab0be8_720w.jpg)

##### 1、单体应用架构：

当网站流量很小时，我们只需要将所有功能都集中在一个应用上，这样可以节省部署节点，从而节省成本

垂直应用架构：
当流程逐渐增加，可以将应用根据业务拆分成多个应用，这些应用之间的通信机制一般为RESTful

分布式服务
当垂直应用越来越多，应用之间的交互不可避免，因此，我们可以将核心业务服务抽取出来，作为独立的服务进行部署，供多个垂直应用调用

微服务基础架构：
当服务越来越多，容量的评估，服务资源的管理等问题就需要增加一个服务调度中心来对服务进行管理。
当流量达到一定规模后，更多的问题会出现和需要我们来解决，比如注册中心，限流，熔断等功能，都需要考虑到。
目前，Dubbo，SpringCloud是典型的代表

##### 2、单体应用模式存在的问题

开发效率低、



## P2  架构演变-垂直拆分+水平拆分

拆分的依据：

以业务的边界进行拆分

分布式集群

系统之间的通信方式一般采用Http---Apache HttpClient

## P17 设计表需要注意什么

#### 1、项目开发流程



#### 2、开发阶段规范

##### 2.1 编码规范

参照《阿里巴巴开发规范》

##### 2.2 数据库设计规范

**三大范式**

- 第一范式：列不可分，原子性
  - 反例：地址 广东省 深圳市，地址可以再进行拆分
- 第二范式：要有主键，每条记录要有唯一性
  - 主键建议是自增列（前提是：没有做分库分表）
  - 切记：**不用用uuid作为主键**
- 第三范式：不用存在传递依赖（不浪费磁盘空间）
  - 存在传递依赖
    - t_studen(id,name,class_id,class_name)
    - t_class(id,name...)
  - 不存在传递依赖
    - t_studen(id,name,class_id)
    - t_class(id,name...)

**反范式**

- 反的是第三范式

  - 1、考虑的通过空间换时间  浪费一点磁盘空间，加快单表查询速度

    例如商品表（id,name, type_id,type_name）

    商品类别表（type_id,type_name）

    时间业务较大考虑：异步都用上商品类别名称

    单表查询速度 ——>优于   多表关联查询速度

  - 带来的坏处 维护变得复杂

    数据表更

    以前：商品类别名称发送了变化，只需要修改商品类别表

    现在：要多维护一张表的数据，商品表中的类别名称也需要进行同步维护

  - 结论：

    查询的速度变快了，更新的速度编码了

    使用场景：读多写少

  - 2、业务场景的需要，历史快照

    举例：订单表（id,order_no, user_address_id,phone,reciver）

    用户-地址 t_user_address(id,address)

    用户地址会随时变更，不能只用id管理查询，需要记录每次的地址信息

**不用减了物理外键，建立逻辑外键**

	- 物理外键带来的的好处是：数据的正确性一致性，从数据库层面去控制
	- 物理外键带来的坏处是：性能问，每次你对数据的操作都要做检查 
 - 建立逻辑外键来解决正确性的问
   	- 通过程序的手段来实现
      	- 录入商品，选择商品分类

**字段的选择上，尽量考虑合适即可**

​		mysql 五百万

- 整形
  - int
  - bigint
  - tinyint
  - 例如：年龄用tinyint足够 用int浪费空间 
  - int(3)  int(32) 占用的空间是一样的 int(3) 可以存入 int(32)的数字
- ​	字符串
  - varchar
  - char
  - 定长用char 如 密码 手机号码
  - 不定长用varchar
- **浮点型** **（重点面试题）**
  - 忘记fload dubbo
  - 使用bigint decimal
  - 比如不要求严谨的可以使用fload dubbo 比如微博积分
  - 金额 很严谨的 不能使用fload dubbo，fload dubbo 精度有限，很容易出问题
  - 金额 存入 bigint ，存入之前先乘以100 取出时 除以100
  - decimal 是支持小数点 并且是精确的



## P18 搭建项目开发骨架

#### 1、项目技术栈

技术栈：Dubbo+SpringBoot+SpringMVC+Spring+MyBatis+MySQL+JQuery+Thymeleaf

分布式文件系统：FastDFS（淘宝）

搜索引擎：Solr

分布式缓存：Redis

消息中间件：RabbitMQ

#### 2、项目整体架构

```
qf-v7
	qf-v7-api
		qf-v7-product-api
	qf-v7-basic
		qf-v7-common
		qf-v7-entity
		qf-v7-mapper
	qf-v7-service
	qf-v7-temp
	qf-v7-web
```

```
dubbo-mall
	dubbo-mall-api
		dubbo-mall-product-api
	dubbo-mall-basic
		dubbo-mall-common
		dubbo-mall-entity
		dubbo-mall-mapper
	dubbo-mall-service
	dubbo-mall-temp
	dubbo-mall-web
		dubbo-mall-background
```

## P19 使用PowerDesigner设计表

#### 数据库表的设计

商品信息

​	id name price sale_price sale_point stock images desc +5个基本自段

​	垂直拆分 

​	商品的基本信息 	id name price sale_price sale_point stock images

​	商品的描述 id desc  product_id

商品类别

​	id，pid,name,desc,

#### PowerDesigner使用

1、创建PhysicalDataMode物理模型

2、选择数据库DBMS类型 MySQL

3、为模型命名dubbo-mall

4、工作区右侧palette为调色板/工具箱

5、New新建-> physical Diagram 物理图表；可以通过创建多个物理图表来进行数据库模块拆分，避免表太多拥挤在一起

**6、导出SQL**菜单栏Database - Generation Database

7、Navicat 在数据库上 右键 运行SQL 选择导出的SQL执行 创建表



## P20 实现entity+mapper层





## P61 SpringBoot整合FreeMaker

Apache

使用FreeMaker生成html页面



```html
<#if (age>40)>
	大叔级
	<#elesif (age>30)>
	老腊肉
	<#else>
	小鲜肉
<#if>	
```



```
<${msg}
```





## P67 RabbitMQ Server安装

##### 主流MQ产品：

RocketMQ 阿里 贡献给Apache

ActiveMQ ApacheJMS java消息服务

RabbitMQ（AMQP 高级消息协议）

Kafka 大数据



#### 安装RabbitMQ服务器

##### 1、安装erlang语言环境

- 设置安装源

  ```bash
  vim /etc/yum.repos.d/rabbitmq-erlang.repo
  vim /etc/yum.repos.d/rabbitmq-erlang.repo
  ```

  

- 输入如下内容

  基于centos6安装

  ```properties
  [rabbitmq-erlang]
  name=rabbitmq-erlang
  baseurl=https://dl.bintray.com/rabbitmq/rpm/erlang/19/el/6
  gpgcheck=1
  gpgkey=https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
  repo_gpgcheck=0
  enabled=1
  ```

  基于centos7安装

  ```properties
  [rabbitmq-erlang]
  name=rabbitmq-erlang
  baseurl=https://dl.bintray.com/rabbitmq/rpm/erlang/20/el/7
  gpgcheck=1
  gpgkey=https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
  repo_gpgcheck=0
  enabled=1
  ```

  

- 执行安装

  yum -y install erlang
  
- 另一种方式安装Erlang  
  
   安装erlang	

```bash
$ wget https://dl.bintray.com/rabbitmq-erlang/rpm/erlang/19/el/7/x86_64/erlang-19.3.6.13-1.el7.centos.x86_64.rpm

rpm -ivh erlang-19.3.6.13-1.el7.centos.x86_64.rpm
```

- RabbitMQ依赖socat，需要提前安装

```
rpm -ivh socat-1.7.3.2-5.el7.lux.x86_64.rpm
```



##### 2、安装RabbitMQ-Server

	- 上传准备好的安装包
	- 采用rpm -ivh 命令镜像安装即可

```bash
$ wget https://dl.bintray.com/rabbitmq/all/rabbitmq-server/3.7.7/rabbitmq-server-3.7.7-1.el7.noarch.rpm

$ rpm -ivh rabbitmq-server-3.7.7-1.el7.noarch.rpm
```

- 错误

```
错误：依赖检测失败：
	erlang >= 19.3 被 rabbitmq-server-3.7.7-1.el7.noarch 需要
	socat 被 rabbitmq-server-3.7.7-1.el7.noarch 需要
```



```
rpm -ivh --nodeps rabbitmq-server-3.7.7-1.el7.noarch.rpm
```



##### 3、启动RabbitMQ服务

查看状态

```
$ rabbitmqctl status 
```

停止RabbitMQ

```
$rabbitmqctl stop
```

设置开机启动

```
$ systemctl enable rabbitmq-server 
```

启动RabbitMQ

```
$ systemctl start rabbitmq-server
```

##### 4、配置运行远程访问



## P68 管理平台的基本操作

1、 新建工程rabbitmq-java

2、引入rabbitmq-client依赖

```xml
    <dependency>
        <groupId>com.rabbitmq</groupId>
        <artifactId>amqp-client</artifactId>
        <version>4.0.2</version>
    </dependency>
```



## P69 掌握简单队列模式



## P70 掌握work模式-限流+手工回复



## P71 掌握publish模式+交换机+队列绑定



## P72 掌握路由和通配符的方式+加上标识，按需发送

\*  表示一个单词

\# 表示一个或多个单词  \#包含*

## P73 SpringBoot整合RabbitMQ简单的模式

#### SpringBoot整合RabbitMQ

1、引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

2、配置application.properties

```properties
spring.rabbitmq.host=192.168.0.108
spring.rabbitmq.port=5672
spring.rabbitmq.username=admin
spring.rabbitmq.password=admin
spring.rabbitmq.virtual-host=/admin
```

3、编写RabbitConfig

## P74 SpringBoot整合RabbitMQ发布订阅模式



#### 提供了哪些具体的交互队列





#### 使用RabbitMQ改造系统

##### 生产者侧：

1、添加依赖

2、添加配置

3、生产者声明交换机

4、发送者消息到交换机

##### 消费者侧：

1、添加依赖

2、添加配置

3、声明队列，建立队列与交换机的绑定关系

4、创建一个类的方法监听队列，加收消息



## P86 SpringBoot 整合JavaMail 实现发送-上



#### 1 使用场景

- 注册验证，激活账号
- 通过注册邮箱找回密码
- 系统监控告警
- 网站营销



#### 2 邮件发送协议

- 邮件传输协议：SMTP协议和POP3协议

  - POP3是Post Office Protocol3的简称，即邮件协议的第三个版本，它规定怎样将个人计算机连接到Internet的邮件服务器和下载电子邮件的电子协议。它是因特网电子邮件的第一个离线协议标准。POP3允许用户从服务器上把邮件存储到本地主机（即自己的计算机）上,同时删除保存在邮件服务器上的邮件，而POP3服务器则是遵循POP3协议的接收邮件服务器，用来接收电子邮件的。

  - SMTP(Simple Mail Transfer Protocol)即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮件的规则,由它来控制信件的中转方式。SMTP协议属于TCP／IP协议族,它帮助每台计算机在发送或中转信件时找到下一个目的地。通过SMTP协议所指定的服务器,我们就可以把E－mail寄到收信人的服务器上了,整个过程只要几分钟。SMTP服务器则是遵循SMTP协议的发送邮件服务器，用来发送或中转你发出的电子邮件。 
  - SMTP认证，简单的说就是要求必须在提供了账户名和密码之后才可以登录到SMTP服务器，这就使得那些垃圾邮件的散播者无可乘之机。
  - 证件SMTP认证的目的是为了使用户避免受到垃圾邮件的侵扰。

- IMAP协议

  IMAP协议全称是nternet Mail Access Protocol,交互式邮件存取协议，它是跟POP3类似的邮件访问协议之一，不同的是，抬起了IMAP后，您在电子邮件客户端收取的邮件仍保留在服务器上，同时在客户端上的操作都会反馈到服务器上，如：删除邮件，标记已读等，服务器上的邮件也会做相应的动作，所以无论从浏览器登录邮箱或者从客户端软件登录邮箱，看到的邮件状态是一致的。

- 发送邮件：SMTP

- 接收邮件：POP3/IMAP

- JavaMail



#### 3 配置环境

- 引入依赖

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-mail</artifactId>
    </dependency>
```



- 配置服务器信息

```yaml
spring:
  mail: 
    host: smtp.163.com
    username: xx@163.com
    password:
    default-encoding: UTF-8
#自定义邮件发送者    
mail:
  fromAddr: 498601485@qq.com
  
```



#### 4 发送普通邮件

```java
@Service
private class MailServiceImpl implements IMailService{

	@Autowired
	private JavaMailSender mailSender;
	
	@Value("${mail.fromAddr}")
	private from;
	
	@Override
	public void sendSimpleMail(String to,String subject, String content){
		SimpleMailMessage simpleMessage = new SimpleMailMessage();
        simpleMessage.setFrom(from);
        simpleMessage.setTo(to);
        simpleMessage.setSubject(subject);
		simpleMessage.setContent(content);
        mailSender.send(simpleMessage);
	}
}
```



## P87 SpringBoot 整合JavaMail 实现发送-下

#### 5 发送HTML格式的邮件

```java
@Autowired
private JavaMailSender javaMailSender;

@Override
public void sendHtmleMail(String to,String subject, String content) throws MessagingException {
    MimeMessage mimeMessage = javaMailSender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(mimeMessage,true);
    helper.setFrom(from);
    helper.setTo(to);
    helper.setSubject(subject);
	helper.setContent(content,true);
    javaMailSender.send(mimeMessage);
}
```

#### 6 发送带附加的邮件

```

```

#### 7 邮件模板

```

```



## P93 Redis概述

谈谈你对Redise的认识

1、技术解决了什么问题，技术有什么特点，在解决这些问题的时候，它是怎么解决的？

2、面试紧张，问一个具体点的问题



Redis解决了什么问题？

**一、缓存**

对比Hibernate Mybatis缓存；

缓存解决了什么问题？减少了对数据库的交换

什么样的数据适合缓存？变化频率不高的数据+访问频率高的数据

2、Hibernate三级缓存

session级别缓存，自动开启，线程级的缓存

sessionFactory级别，需要配置，配置为第三方的缓存ecache，进程级的缓存

查询缓存，依赖二级缓存，HQL 设置需要缓存 配置bean

3、Mybatis缓存

一级缓存，sqkSession 线程级的缓存 每个session

二级缓存 SQLSessionFactory 进程级的，整个应用缓存

4、Hibernate 和Mybatis都是本地缓存，无法进行分布式缓存。

Redis服务器 用以做分布式缓存更容易

**二、存储信息**

共享Session  session.setAttribute() ,将session 存储到redis 解决了session共享问题，替代原来的



**三、数据类型丰富、解决开发问题**

文章点赞： 数据库表建立一个字段 点击加1，

可以存入Redis内存

**四、热点事件排行**

如对点击量排行

**五、Redis做消息队列**



##### 学习路线

3.1 Redis基础知识 （数据类型+内存管理机制+持久化机制+流水线）

3.2Redis高可用架构+哨兵机制

3.3Redis高性能架构-RedisCluster

3.4 Java操作Redis(Spring SpringBoot)

3.5Redis缓存更新策略

3.6 热点key的重建策略

3.7 应对缓存穿透工具

3.8 应对缓存雪崩的问题

3.9 Redis的分布式锁

## P94 Redis安装及服务启动

### 1、Redis 简介

Redis是一种鉴于键值对的NOSQL数据库，与很多键值对数据库不同的是，Redis中的值可以是String、Hash、List、Set集合、Zset有序集合等多种数据结构。

##### Redis VS Memcached

redis 数据类型更丰富

redis支持持久化

##### Redis的特点

###### 高性能：

Redis将所有数据都存储在内存中，所有它的读写性能非常之高，官方的数据时可以达到10万次/秒

###### 可靠性：

Redis还将内存中的数据利用快照和日志的信息保存到硬盘中，这样可以避免发生断电或机器故障，内存数据丢失问题。

###### 数据类型丰富

字符串、hash、list、set、zset

### 2、Redis应用场景

##### 缓存

##### 计数器应用

##### 保存用户凭证

实现多系统之间的单端登录凭证

##### 消息队列功能

Redis提供了发布订阅功能和阻塞队列功能

总结：

1、前期数据的压力、将经常被访问、但更新不频繁的数据，保存在缓存中、这样可以减少对数据库的操作

2、代替数据的存储功能，保存赞和踩，保存手机验证码、保存用户凭证

### 3、Redis环境搭建

首先将redis的安装包上传到服务器 将其放到usr/local中

第一步：安装gcc环境

```bash
yum -y install gcc-c++
```

第二步：解压redis源码包

```bash
tar -zxvf redis-3.2.6.tar.gz
```

第三步：编译redis源码

```bash
make
```

第四步：安装redis

```bash
make install PREFIX=/usr/local/redis3
```

### 4、启动服务端

启动redis 分为按默认配置启动和使用配置文件启动（官方建议）

1、默认启动方式（非守护线程启动）

```bash
cd /usr/local/redis3
./redis-server
```

持久化

```
退出时，自动创建dump.rdb文件 存储redis信息
```

默认服务端是 启动的

复制配置文件

```
cd /usr/local/redis/redis-3.2.6/
cp redis.conf /usr/local/redis3/bin/
```

修改配置文件

```
vim redis.conf

将daemonize no 改为 daemonize yes 表示以守护线程启动运行
```

以守护线程启动

```
cd /usr/local/redis3
./redis-server redis.conf
```

关闭服务器

```
./redis-cli 
shutdown
```



### 5、启动Redis客户端

```
cd /usr/local/redis3
./redis-cli
```

连接非本地客户端(远程服务器)

```
./redis-cli -h 127.0.0.1 -p 6379
```

### 6、解除本地绑定

redis低版本默认没有这事仅限本机访问，而高版本则有默认本机访问的设置，索引需要将高版本的redis配置为可远程访问，即解除本机绑定IP

```
vim redis.conf
bind 127.0.0.1 改为
#bind 127.0.0.1
```

保存退出后 需要重启redis

```
./redis-cli
SHUNTDOWN
exit 

./redis-server redis.conf
```

配置重启防护墙

```
service iptables restart  ##centos6.X
```



### 7、安全加固-设置Redis的访问密码

开启远程访问后  java连接redis会保错，由于开启远程访问后，出现了安全问题，这时需要设置密码

```bash
cd /usr/local/redis3/bin
vim redis.conf
/requ  # 查找 以requ开头的文字
#requirepass foobared 改为
requirepass 123456  #将密码改为123456
#保存退出 重启redis-server
./redis-cli
SHUTDOWN
exit
./redis-server redis.conf
```

也可通过以下方式重启

```bash
ps -aux  | grep redis
kill -9 5835 # 删掉进程
# 重启进程
./redis-server redis.conf
```

再次使用客户端时，需要使用密码才能访问

```bash
./redis-cli
get name  # 提示NOAUTH Authentication required
AUTH 123456 # 认证密码
get name #认证成功后既可以get 值了
```



### 8、Java连接Redis

1、搭建Maven工程，引入依赖

```xml
<dependency>
	<groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
</dependency>
```

2、使用Jedis提供的API操作Redis

```java
public class JedisTest{
    @Test
    public void jedisTest(){
        Jedis jedis = new Jedis("127.0.0.1",6379);
        jedis.auth("");	//密码
        jedis.set("name","刘德华");
        jedis.set("superStart","刘德华");
        String superStart = jedis.get(“superStart”);
        System.out.println(superStart);
    }
}
```

在Redis没有设置密码之前，使用java连接redis 提示 处于保护模式，没有设置绑定的访问地址，也没有设置密码

解决方案

1、禁用保护模式 disable protected mode

2、只有某一台服务允许连接，其他地址都不允许连接

3、仅测试用 重启redis服务器 使用参数 protected-mode no

4、设置一个密码 



## P94 Redis字符串类型操作

Redis的类型仅指的是 Value的类型，key的类型都是String

Key-------------------------Value

String----------------------String

String-----------------------Hash

String-----------------------List

String-----------------------Set

String-----------------------Zset