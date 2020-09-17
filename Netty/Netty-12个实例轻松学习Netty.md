> https://www.bilibili.com/video/BV1Q5411a7Hy?seid=9198776770668101058

### 课程目标

- 树立使用id、开发netty
- 掌握Netty开发网络通信系统的流程和步骤
- 掌握
- 掌握Netty的各种解码器
- 掌握Netty如何继承第三方编码
- 

### 为什么用Netty

Netty是一个开源的异步事件驱动的网络应用程序架构，用于快速开发可维护的高性能协议服务器和客户端。

##### Netty的特点:

高并发、高性能、高封装

##### Netty的优势：

使用简单、功能强大、扩展灵活、超强稳定、社区活跃

本课程基于Netty4 

##### Netty能做什么：

- 基本应用：根据各种通信协议，写客户端、服务器端应用
- 进阶应用：实现自己的http服务器、ftp服务器、udp服务、RPC服务器、websocket服务器、Redis的Proxy服务器、MySQL的代理服务器

##### 掌握Netty有什么好处

- 进大厂、高薪水涨工资
- 多款开源框架应用了Netty
  - dubbo
  - RocketMQ
  - Haddop的高性能通信和序列号组件Avro的RPC框架

##### Netty的创始人和卓越贡献人

- 创始人 韩国人

##### 学习路径：

- 开始学习
- 环境准备
- 创建第一个Netty示例
- 长连接及状态监测
- 网络通信
- 四大解码器
- POJO编码解密框架
- 异步事件处理值普通任务和定时任务
- 长耗时业务异步处理

##### 课程特点：

- 先讲实现流程、一步一步敲代码实现功能，接口代码阐述概念及注意事项
- 每个实例都基于实际应用触发，可直接以此为基础

##### 学员基础：

变量、运算符、流程控制、数组、对象、继承、接口



### 开发环境搭建

##### JDK的下载、安装及环境变量设置



##### Maven的下载、安装、环境变量设置



##### IntellJ IDEA的安装、设置



创建Maven工程及配置Netty依赖



### 示例1：传输一个字符串

##### 本节内容：

1、Netty开发的基本流程

2、分别实现客户端、服务器端

3、Netty关键知识点

##### Netty开发的基本流程(客户端)

- 创建Handler


```
1、创建Handler类
2、类添加Shrable注解
3、类继承SimpleChannelInboundHandler
4、重写channelRead0()方法，处理接收到的消息
5、重写execeptionCaught()反复处理异常
```

- 创建程序启动类

```
内容包括两部分：main韩式，连接方法，主要工作在连接方法中，
连接方法中要做的工作：
1、创建EventLoopGroup实例
2、创建Bootstrp实例
3、配置Bootstrap
4、连接远端、生成ChannelFuture实例
5、关闭连接、释放资源
```

  


##### Netty开发的基本流程(服务器端)

- 创建Handler

```
1、创建Handler类
2、类添加Shrable注解
3、类继承hannelInboundHandlerAdapter
4、重写channelRead()方法，处理接收到的消息
5、重写execeptionCaught()反复处理异常
```



- 创建程序启动类

```
内容包括两部分：main韩式，连接方法，主要工作在连接方法中，
连接方法中要做的工作：
1、创建EventLoopGroup实例
2、创建ServerBootstrp实例
3、配置ServerBootstrap
4、绑定端口、生成ChannelFuture实例
5、关闭连接、释放资源
```

##### Netty关键知识点

-   I/O 各种各样的流（文件、数组、缓冲、管道...）的处理（输入输出）；
- Channel  通道 代表一个连接，每个client请求会对应到具体的一个channel
- ChannelPipleline 责任链 每个Channel都有且仅有一个ChannelPipleline与之对应，里面各种各样的Handeler
- handler 用于处理出入站消息及相应的事件，实现我们自己的业务逻辑
- EventLoopGroup IO线程池 负载处理Channel对应的IO事件
- ServerBootstap 服务器端启动辅助对象
-  Bootstrap 客户端启动辅助对象
- ChannelInitalizer channel初始化器
- ChannelFuture 代表IO操作的执行结果，通过事件机制，获取执行记过，通过添加监听器 执行我们想要的操作
- ByteBuf 字节序列 通过ByteBuf操作基础的字节数组和缓冲区



### 示例2：连接及活动状态监测

1、监测新增连接情况

2、监测连接断开请

3、监测连接活动情况

4、监测连接非活动情况

