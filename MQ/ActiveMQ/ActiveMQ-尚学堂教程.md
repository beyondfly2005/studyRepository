> 视频
>
> https://www.bilibili.com/video/BV1bb411v7fB?p=114
>
> https://www.bilibili.com/video/BV1Tb41127kY?seid=13048121913536985629

#### 

```java
messageConsumer.receive()
messageConsumer.receive(4000L);


receive是同步阻塞方式
//订阅者或接受者调用MessageConsumer的receive()方法接收消息，receive方法在能够接收到消息之前或者超时之前 移植阻塞
//不带时间参数，一直等待
//带时间参数，等待指定的时间间隔后 没有消息就离场 进程结束

    
//通过监听的方式接收消息   
messageConsumer.setMessageLintener(new MessageListener(){
    @Override
    public void onMessage(Message message){
        if(null!=message && message instanceof TextMessage){
            TextMessage textMessage=(TextMessage)message;
            try{
                
            } catch(){
                
            }
        }
    }
});
System.in.read();  //保证控制台不关闭
sessage.close();
session.close();
connect.close();

```

Topic模式队列  Queue模式队列 区别

工作模式：

- queue 消息不会丢弃 一对

- 没有订阅者 消息被丢弃

有无状态

- Topic 模式无状态
- a

### 一、JMS简介

MOM

### 二、ActiveMQ简介

支持多种语言和协议编写客户端，支持的语言：Java，C,C++，C#,Ruby,perl,Python,PHP,

支持的应用协议：OpenWire，Stomp REST



### JAVAEE

13个核心规范工业标准

- 1- JDBC (Java database Connect)

   数据库连接

- 2- JNDI (Jva Naming and Directory Interfaces)

   java的命名和目录接口

- 3- EJB (Enterprise JavaBean)

  企业级JavaBean

- 4- RMI (Remote Method Invoke)

  远程方法调用

- 5- Java IDL (Interface Description Language )/CORBA(Common Object Broker Archiecture)

  接口定义语言/公共对象请求代理程序体系结构

- 6- JSP (Java Server Page)

- 7- Servlet

- 8- XML

- 9- JMS(Java Message Service)

  Java 消息服务

- 10- JTA(Java Transcation API)

  Java 事务API

- 11- JTS (Java Transcation Service)

  Java事务服务

- 12- Java Mail

  Java邮件服务

- 13- JAF(JavaBean Activation Framework)

### JMS

### 常用的MQ比较

ActiveMQ 与其他产品区别

单机吞吐量 万级

技术成熟 Apache产品，很低的消息丢失

Kafka 十万级吞吐量，

RocketMQ

​	