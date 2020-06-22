#### 为什么要使用MQ

- 解耦
- 削峰
- 异步

存在的的问题

- 系统之间接口耦合比较严重
- 面对大流量并发时，容易被冲垮
- 等待同步存在性能问题

达到的目标

- 系统解耦，新的模块接进来时，代码改动最小
- 设置流量缓冲池，可以让后端系统安装自身的吞吐能力进行消费，不被冲垮，能够削峰
- 强弱依赖树立能将非关键调用链路

#### MQ的定义

面向消息的而中间件MOM能够很好的



#### ActiveMQ安装和下载

官网：http://activemq.apache.org

编程语言java



#### 常用的MQ消息中间件

- RabbitMQ  编程语言erlang
- RocketMQ 编程语言java 阿里
- ActiveMQ 编程语言 java
- Kafka 编程语言java/scala

#### MQ的主要功能

- 实现高可用、高性能、可伸缩、易用、和安全的企业级面向消息的服务系统
- 异步消息的消息和处理
- 控制消息的消费处理
- 可以和spring springboot整合简化代码
- 配置集群容错的MQ集群

#### ActiveMQ的安装

**官网下载 ** active.apache.mq

##### 安装

```bash
#上传到 /opt  目录
tar -zxvf apache-activemq-5.15.13-bin.tar.gz
```



目录

- bin
- conf 配置
- data 数据
- doc 文档
- example 范例
- lib 

 启动

```bash
cd bin
./activemq start

# 默认端口61616

# 三种查看进程的方法

# 1 grep
ps -ef| grep activemq

# 屏蔽掉当前的查找进程 grep activemq
ps -ef| grep activemq|grep -v grep

# 2查询端口是否被占用 被占用 active启动
netstat -anp|grep 61616
# 3 lsof
lsof -i:61616

./activemq stop
./activemq restart

# 带日志的启动方式
./activemq start > /usr/local/activemq/run_activemq.log


# 前台图形化界面
http://192.168.198.128:8161/

# 保证网络是通的和防火墙

# 查看防火墙状态
systemctl status firewalld
# Active: active (running) 表示运行中

#查看firewall的状态
firewall-cmd --state
# 显示 running 表示运行中

# 查询端口是否开放
firewall-cmd --query-port=8161/tcp
# 开放2181端口
firewall-cmd --permanent --add-port=8161/tcp
# 移除端口
firewall-cmd --permanent --remove-port=2181/tcp

#重启防火墙 生效配置
firewall-cmd --reload


# 前台图形化界面
http://192.168.198.128:8161/
# 默认的用户名密码 admin/admin
```

#### JAVA 调用MQ

```java

# idea 新建maven 工程
#     

# 所需要的jar包
    <dependency>
        <groupId>org.apache.activemq</groupId>
        <artifactId>activemq-all</artifactId>
        <version>5.15.9</version>
    </dependency>

    <dependency>
        <groupId>org.apache.xbean</groupId>
        <artifactId>xbean-spring</artifactId>
        <version>3.16</version>
    </dependency> 


```



JDBC操作数据库的通用步骤

- 1 注册驱动

- 2 建立连接

- 3 创建运行SQL的statument

- 4 执行sql处理运行结果

- 5 释放资源

ActiveMQ的通用步骤

- 1 
- 2 创建连接

#### MQ弊端：



 