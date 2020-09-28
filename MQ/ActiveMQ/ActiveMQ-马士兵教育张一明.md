### ActiveMQ安装

- 下载
- 安装
- Jetty



Broker -服务器

admin / admin

Queues 点对点 模式，只能被一次读取 

Topic 主题订阅模式



异步解耦

将同步变为异步

ACK



MQ 存储、通知



秒杀、日志、通信、解耦、冗余、扩展性、时

可恢复性

顺序保证

过载保护

数据流处理

幂等—重复提交或重复消费，不会对系统数据造成混乱

##### 常用消息中间件

|特性 |ActiveMQ | RabbitMQ | RocketMQ | Kafka |
| ---- | ---- | ---- | ---- |
|      |      |||
| | |||
| | |||
| | |||
|  | |||

#### ActiveMQ安全机制

系统安全认证的

web控制台的

默认任何用户都可以连接到 broker

一个项目一个账号

conf/activemq.xml

在borker节点上添加如下

```xml
    <plugins>
        <simpleAuthenticationPlugin>
            <users>
                <authenticationUser username="${activemq.username}" password="${activemq.password}" groups="users,admins"/>
            </users>
        </simpleAuthenticationPlugin>
    </plugins>
```



```

```



***/conf/credentials.properties*** 

```
activemq.username=system
activemq.password=manager
```



持久化

###### kahaDB存储



###### AMQ方式

只适用于5.3



###### JDBC存储



###### LevelDB存储



###### Memory消息存储



###### JDBC Message Stor with ActiveMQ 



