### SOA和RPC

#### 一 SOA架构

SOA架构



5 使用SOA架构

​	专门访问数据库的服务

​	多个子项目 或者子服务 访问数据库 不能单独访问数据库

6 实现SOA架构时，几种常用实现

​	6.1 WebService作为服务

​	6.2 Dubbo作为服务

​	6.3 DubboX作为服务

​	6.4 服务方就是web项目 调用web项目的控制器

​		使用HttpClient调用其他项目的控制器Controller

#### 二 RPC 远程过程调用

- 英文名称Remote Procedure Call Protocl 
- 中文名称：远程过程调用协议
- PRC解析：客户端通过网络远程调用远程服务器，不知道远程服务器的具体实现，只知道远程服务器提供了什么功能
- RPC最大优点 
  - 数据安全性

#### 三 Dubbo简介

​	一个分布式 高性能 透明化的RPC服务框架，提供服务自动注册、自动发现等高效服务治理方案。

3.1 Dubbo架构图

![](https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2403459333,1166926048&fm=26&gp=0.jpg)

Provider 提供方 服务发布方

Consumer 消费方 调用服务方

Container Dubbo容器 依赖于Spring容器

Registry 注册中心 当Container启动时 把所有可以提供的服务列表到Registry中进行注册。

注册中心作用：告诉Consumer提供了哪些服务，和服务方在哪里。

Monitor 监听器

虚线 异步访问 实现是同步访问

蓝色虚线 启动时完成的功能

所有的角色都可以在单独的服务器上，所有必须遵守特定的协议

红色虚线/红色实线 都是子程序运行过程中执行的功能

运行原理：

0 start 启动容器，相当于在启动Dubbo的Provider

1 启动后会去注册中心进行注册，注册所有可以通过的服务列表

2 在Consumer启动后回去注册中心获取服务列表和提供的地址 进行订阅

3 当provider 有修改后，注册中心会把消息推送给Con

​	使用了观察者模式 或者叫发布订阅模式 注册中心和Consumer之间用了观察者模式

4 根据获取到的Provider地址 真实的调用Provider中的功能

​	在Consumer方使用了代理设计模式，创建一个Provider方类的一个代理对象，通过代理对象获取Provider中真是功能，起到了保护Provider真实功能的作用

5 Monitor 监听器Consumer和Provider每隔一分钟向Monitor发送统计信息，保护 访问次数 频率等

#### 四 Dubbo注册中心

Dubbo支持的注册中心

- Zookeeper

  支持网络集群 dubbo2.3.3以上推荐使用

  稳定性依赖于Zookeeper 受限于Zookeeper

- Redis注册中心

  支持客户端双写的集群模式 性能高

  缺点：对服务器环境要求较高

- Multicast

  免中心话 不需要额外安装软件

  缺点：建议同机房（局域网内使用）

- Simple

  适用于测试环境,不支持集群

#### 五 Zookeeper

分布式协调组件

Zooker常用功能 ：

- 发布订阅功能  把
- 分布式/集群管理功能
- 使用Java语言编写 
- Zookeeper安装

```bash
# 安装JDK
cd /usr/local/
mkdir jdk
# 上传压缩包

# 解压
tar -zxvf jdk-8u221-linux-x64.tar.gz

vim /etc/profile
export JAVA_HOME=/usr/local/jdk1.8
export PATH=$JAVA_HOME/bin:$PATH
source /etc/profile
java -version
# 安装Zookeeper

#
cd /usr/local/temp
#上传文件

#解压
tar -zxvf zookeeper-3.4.6.tar.gz 

# 复制
cp -r /usr/local/temp/zookeeper-3.4.6 /usr/local/zookeeper

cd /usr/local/zookeeper

mkdir data
cd conf
cp zoo_sample.cfg zoo.cfg

# 修改配置文件
vim zoo.cfg
dataDir=/usr/local/zookeeper/data

# 启动Zookeeper 
cd ../bin 
# 或者
cd /usr/local/zookeeper/bin

# 启动zk
./zkServer.sh start

# 查看zk状态
./zkServer.sh status
# 启动成功显示 Mode: standalone

# 默认端口2181
# centos6.5 
# 使用的iptables 防火墙
vim /etc/sysconfig/iptables
#增加一行
-A INPUT -m state --state NEW -m tcp -p tcp --dport 2181 -j ACCEPT
# 重启防火墙生效配置
service iptables restart


# centos7使用的是firewall

# 查看防火墙状态
systemctl status firewalld
# Active: active (running) 表示运行中

#查看firewall的状态
firewall-cmd --state
# 显示 running 表示运行中

# 查询端口是否开放
firewall-cmd --query-port=2181/tcp
# 开放2181端口
firewall-cmd --permanent --add-port=2181/tcp
# 移除端口
firewall-cmd --permanent --remove-port=2181/tcp

#重启防火墙 生效配置
firewall-cmd --reload
```

#### 六  Dubbo支持的协议

- Dubbo协议

  dubbo的默认协议，采用NIO复用单一长连接，并使用线程池并发处理，适合于小数据量大并发的服务调用，以及服务消费者机器数远大于服务提供者机器数的情况

  缺点：不适合传送大数据量的服务，比如传文件，传视频等，除非请求量很低。

  会采用vsftpd 处理文件 不使用dubbo

- Rmi协议

  JDK提供的协议，远程方法调用协议

  缺点：偶尔调用失败

  优点：JDK原生，不需要进行额外配置（不需要导入jar包）

- Hessian协议

  优点：原生与Hessian互操作，基于http协议

  缺点：需要Hessian支持 需要额外导入jar，并在短连接时开销低

#### 七  Dubbo中Provider搭建

​	接口和实现类分别创建到不同的项目中；

​	实现类依赖于接口；Consumer也依赖于这个接口

​	1- 将服务提供者注册到注册中心(暴露服务)

			- 引入dobbo依赖
			- 引入Zookeeper客户端
			- 配置文件

​	2- 让服务消费者去注册中心订阅服务



zookeeper 客户端

Zookeeper2.5 使用的是zkclient

Zookeeper2.6 使用的是curator



Dubbo-Admin

```bash
# 下载doubbo 运维包的源码
git clone https://github.com/apache/dubbo.git

# 切换到指定tag版本
git checkout -b dubbo-2.6.0

cd dubbo-admin

# 构建war包
mvn  package -Dmaven.skip.test=true

#构建之前 必须配置好 java和 maven的环境变量！！！

# 将dubbo-admin-2.6.0.war上传到tomcat服务器 webapps
cd /usr/local/tomcat/webapps/
mv dubbo-admin-2.6.0.war dubbo-admin.war

#启动tomcat
cd /usr/local/tomcat/bin
./start.sh

# 
cd  /usr/local/tomcat/webapps/dubbo-admin-2.6.0/WEB-INF
vim dubbo.properties

# 检查Zookeeper地址 
# 如有必要，可以修改root或guest密码 
# 和zookeeper注册中心<dubbo:registry address="zookeeper://127.0.0.1:2181" />中的保持一致
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 用户名
dubbo.admin.root.password=root
# 密码
dubbo.admin.guest.password=guest

# dubbo.properties如有变动 需要重启tomcat
cd /usr/local/tomcat/bin
./shutdow.sh
./start.sh

# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放2181端口
firewall-cmd --permanent --add-port=8080/tcp
# 移除端口
firewall-cmd --permanent --remove-port=2181/tcp

#重启防火墙 生效配置
firewall-cmd --reload

# 浏览器访问
http://tomcat-IP:8080/dubbo-admin
http://192.168.198.128:8080/dubbo-admin

#提示输入用户名密码 
root/root

```



