> 地址： https://www.bilibili.com/video/BV1Gb411T7Ha

# 一、基础知识

## 1.  分布式基础理论

### 1.1 什么是分布式系统

《分布式系统原理与范型》定义：

“分布式系统是如果独立计算机的集合，这些计算机对于用户来说，就像单个相关系统”，分布式系统（distrivuted sytem）是建立在网络之上的软件系统。

随着或联网的发展，网站应用的不断扩大，常规的垂直应用系统架构已经无法应对，分布式服务架构以及流动计算机架构势在必行，急需一个治理系统确保架构有条不紊的演进。

### 1.2 发展演变

![](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=4130954518,1824593826&fm=15&gp=0.jpg)

- **单一应用架构**

  当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

  随着网址访问压力的增大，初期可以用多个服务器部署多套 nginx做负载均衡

- **垂直应用架构**

  界面与业务逻辑分离

  应用不可能完全独立，应用之间需要交互

- **分布式架构**

  按业务进行拆分，

  本地进程内通讯  -> RPC远程过程调用

- **流动计算架构**

  随着微服务的越来越多，服务器节点随之增多，如果将更多的服务器资源分配给压力更大的业务业务服务系统

### 1.3 RPC

![img](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=962964185,1176401230&fm=11&gp=0.jpg)

- 什么是RPC

- RPC的两个核心模块：通讯、序列化

- RPC框架有哪些

  阿里dubbo、谷歌gRPC、HSF(Heigh Speed Service Framework)

## 2.  Dubbo核心概念

### 2.1 简介

##### Dubbo的发展历史

​	阿里巴巴开源的

​	11年 开源到github

​	14年10月 2.4.11版本

​	停更

​	当当网 dubboX

​	网易考拉dubbokey

​	17年 

​	18年dubbox与dubbo合并 并发布了2.6

​	18年2月15日 dubbo开源贡献给Apache组织



##### **Dubbo的优良特性**

- 面向接口代理的高性能RPC调用

- 智能负载均衡

- 服务自动注册与发现

- 高度可扩展能力

- 运行期流量调度

  通过配置路由规则，轻松实现灰度发布

- 可视化的服务治理与运维

### 2.2 设计架构

## 3.  Dubbo 环境搭建

![](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2070965499,2376100404&fm=26&gp=0.jpg)

**Provider服务提供者**

​	服务提供供方，服务提供者在启动时，向注册中心注册自己提供的服务。
**Consumer服务消费者**

​	调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
**Registry注册中心**

​	注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者
**Monitor监控中心**

​	服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

### 3.1 windows安装Zookeeper

##### 1、下载zookeeper

​		https://archive.apache.org/dist/zookeeper/zookeeper-3.4.13/

##### 2、解压zookeeper

​	解压运行zkServer.cmd ，初次运行会报错，没有zoo.cfg配置文件

##### 3、修改配置文件

​	复制conf/zoo_example.cfg 并命名为 zoo.cfg

​	创建目录data ，并修改zoo.cfg文件中的 dataDir=../data

##### 4、启动Zookeeper

```bash
zkServer.cmd
```

##### 5、测试zkClient.cmd

```
get /
ls /
create -e /atguigu 123456
ls /
get /atguigu
```



### 3.2 windows安装监控中心

Dubbo OPS Dubbo运维 (dubbo-adming管理控制台 、 dubbo-monitor监控中心)

##### 安装管理控制台

##### 1、git下载源码

```bash
git clone https://github.com/apache/dubbo-admin.git
cd dubbo-admin

```

##### 2、修改配置文件zk注册中心地址

修改src/main/resources/application.properties文件

```properties
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 将zk注册中心地址修改为实际地址
```

##### 3、Maven编译项目

```bash
cd dubbo-ops\dubbo\dubbo-admin
mvn clean package
cd dubbo-admin/target

```

##### 4、运行jar包启动管理控制台

```bash
java -jar dubbo-admin-0.1.jar
```

##### 5、打开管理控制台 测试

```
浏览器打开
localhost:7001
```



### 3.3 Linux安装Zookeeper

##### 1、下载zookeeper

​		https://archive.apache.org/dist/zookeeper/zookeeper-3.4.13/

##### 2、上传压缩文件到服务器	

```bash
cp  zookeeper-3.4.13.tar.gz  /usr/local/
```

##### 3、解压zookeeper

```bash
cd /usr/local/
tar -zxvf zookeeper-3.4.13.tar.gz
# 修改目录名称
mv zookeeper-3.4.13 zookeeper
```

​	解压运行zkServer.sh ，初次运行会报错，没有zoo.cfg配置文件

##### 4、修改配置文件

```bash
#复制conf/zoo_example.cfg 并命名为 zoo.cfg
cd zookeeper
# 创建目录data
mkdir -P data/zookeeper

cd ../conf
cp cp zoo_sample.cfg zoo.cfg

#并修改zoo.cfg文件中的 dataDir=../data/zookeeper
vim zoo.cfg
# 将
dataDir=tmp/zookeeper
改为
dataDir=../data/zookeeper
:wq 	#保存退出
```

##### 4、启动Zookeeper

```bash
cd /usr/local/zookeeper
./bin/zkServer.sh start
```

相关其他命令

```bash
./bin/zkServer.sh {start|start-foreground|stop|restart|status|upgrade|print-cmd}
```

##### 5、使用zkClient.sh测试

```
get /
ls /
create -e /atguigu 123456
ls /
get /atguigu
```



### 3.4 Linux安装监控中心

##### 安装管理控制台

##### 1、git下载源码

```bash
git clone https://github.com/apache/dubbo-admin.git
cd dubbo-admin

```

##### 2、修改配置文件zk注册中心地址

修改src/main/resources/application.properties文件

```properties
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 将zk注册中心地址修改为实际地址
```

##### 3、Maven编译项目

```bash
cd dubbo-ops\dubbo\dubbo-admin
mvn clean package
cd dubbo-admin/target

```

##### 4、运行jar包启动管理控制台

```bash
java -jar dubbo-admin-0.1.jar
```

##### 5、打开管理控制台 测试

```
浏览器打开
localhost:7001
```



## 4. Dubbo-helloword

### 4.1 需求提出

某个电商系统，订单服务需要调用用户服务获取某个用户的所有地址；

我们现在 需要创建两个服务模块进行测试

| 模块            | 功能       |
| --------------- | ---------- |
| 订单服务web模块   | 创建订单等 |
| 用户服务service模块| 查询用户地址等|

测试预期结果：

订单服务web模块在A服务器，用户服务模块在B服务器，A可以远程调用B的功能。

1、创建用户服务模块user-service-provider

2、创建订单服务模块order-service-consumer

3、抽取API接口层

​	service、bean(entity、domain、model)



### 4.2 工程架构

根据《dubbo服务化最佳实践》

http://dubbo.apache.org/zh-cn/docs/user/best-practice.html

###### 1、分包

建议将服务接口、服务模型、服务异常等均放在 API 包中，因为服务模型和异常也是 API 的一部分，这样做也符合分包原则：重用发布等价原则(REP)，共同重用原则(CRP)。

###### 2、粒度

服务接口尽可能大粒度，每个服务方法应代表一个功能，而不是某功能的一个步骤，否则将面临分布式事务问题，Dubbo 暂未提供分布式事务支持。

###### 3、版本

每个接口都应定义版本号，为后续不兼容升级提供可能，

如： `<dubbo:service interface="com.xxx.XxxService" version="1.0" />`。

建议使用两位版本号，因为第三位版本号通常表示兼容升级，只有不兼容时才需要变更服务版本。

当不兼容时，先升级一半提供者为新版本，再将消费者全部升为新版本，然后将剩下的一半提供者升为新版本。

4、

### 4.3 使用Dubbo改造

#### 服务提供者配置

##### 1、将服务提供者注册到注册中心（暴露服务）

1.1 导入Dubbo依赖(2.6.2) 引入Dubbo客户端curator

zookeeper2.6版之前的zk使用 zkclient作为客户端

zookeeper2.6版之后的zk使用curator作为客户端

```bash
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>dubbo</artifactId>
        <version>2.6.2</version>
    </dependency>
    <dependency>
        <groupId>com.101tec</groupId>
        <artifactId>zkclient</artifactId>
        <version>0.10</version>
    </dependency>

    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>2.12.0</version>
    </dependency>
```

1.2 配置服务提供者的配置文件

​		在resources资源文件目录创建provider.xml文件，

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd        http://dubbo.apache.org/schema/dubbo        http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- 1- 指定当前无误的名称 (同样的服务名称相同，不要和其他服务同名) -->
    <dubbo:application name="user-service-provider"  />

    <!-- 2- z指定注册中心地址 -->
    <!--<dubbo:registry address="zookeeper://192.168.13.254:2181" />-->
    <dubbo:registry protocol="zookeeper" address="192.168.13.254:2181" />

    <!-- 指定通讯规则 通讯端口通讯协议 -->
    <dubbo:protocol name="dubbo" port="20880" />

    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="com.beyondsoft.gmall.service.UserService" ref="userService" />

    <!-- 和本地bean一样实现服务 -->
    <bean id="userService" class="com.beyondsoft.gmall.service.impl.UserServiceImpl" />
</beans>
```



#####  2、让服务消费者去注册中心订阅服务服务提供者的地址

#### 服务消费者配置

#####  1、将服务消费者注册到注册中心

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="order-service-consumer"/>
    <!-- zookeeper注册中心 -->
    <dubbo:registry address="zookeeper://192.168.1254:2181"/>

    <!-- 远程接口调用-->
    <dubbo:reference id="userService" check="false" interface="com.beyondsoft.gmall.service.UserService"/>
</beans>
```

##### 2、编写测试类

```java
public class MainApplication {
    public static void main(String[] args) throws IOException {
        ClassPathXmlApplicationContext context =new ClassPathXmlApplicationContext("consumer.xml");
        OrderService orderService = context.getBean(OrderService.class);
        orderService.initOrder("1");
    }
}
```



## 5. 监控中心

### 5.1 dubbo-admin管理控制台

​	图形化的服务管理页面，安装时需要制定注册中心地址，接口从注册中心中获取到所有的提供者/消费者进行配置管理

### 5.2 dubbo-monitor-simple监控中心

简单的监控中心，用于监控服务调用等信息。

Dubbo-OPS  Dubbo运维相关

mva package 打包

**使用监控中心**

消费者和提供者 的配置文件：

```xml
<dubbo:monitor protpcal="registry"></dubbo:monitor>
```



## 6. 整合SpringBoot

##### 1、创建模块

boot-user-service-provider 用户模块 服务提供者

boot-order-service-consumer 订单模块 服务消费者

##### 2、导入依赖

- dubbo-starter
- dubbo 其他不需要

```xml
	<dependency>
	    <groupId>com.alibaba.boot</groupId>
	    <artifactId>dubbo-spring-boot-starter</artifactId>
	    <version>0.2.1.RELEASE</version>
	</dependency>
```

配置application.properties

```properties
#配置应用名称
dubbo.application.name=user-service-provider
#配置注册中心
dubbo.registry.address=127.0.0.1:2181
dubbo.registry.protocol=zookeeper
#通信协议
dubbo.protocol.name=dubbo
dubbo.protocol.port=20880
#连接监控中心
dubbo,monitor.protocol=registry
```

在服务提供者端的服务上 添加@Service 暴露服务

```java
@Service  //@com.alibaba.dubbo.config.annotation.Service
@Service //@org.sprongframework.stereotype.Service //两个冲突 可以将spring的@Service改为 @Componet
public class UserServiceImpl implements UserService{
    
}
```

在服务消费者端的服务商 添加@Reference远程调用服务

```java
@Service //这里是spring的@Service
public class OrderServiceImpl implements OrderService{
	@Reference
    private UserService userService;
    
    public List<UserAddress> initOrder(String userId){
        List<UseAddress> addressList = userService.getUserAddressList(userId);
        return addressList;
    }
}
```



##### 3、主程序中启动

主程序启动类上开启基于注解的Dubbo功能

```java
@EnableDubbo //开启基于注解的Dubbo功能
@SpringBootApplication
public class BootUserServicePrividerApplication{
	public static void manin(String[] args){
        SpringApplication.run(BootUserServicePrividerApplication.class,args);
    }
}
```



##### 4、配置映射规则

##### 5、属性覆盖策略

- 虚拟机参数配置： -Dubbo.protocol.port=20880

- XML文件配置：dubbo.xml

  ```xml
  <dobbo:protocol port="30880"/>
  ```

- Properties属性配置：dubbo.properties

  ```properties
  dobbo.protocol.port=20880
  ```

  

# 二、Dubbo配置

## 1、启动时检查

#### 通过spring配置文件配置

关闭某个消费者启动时检查

```xml
<dubbo:reference interface="xx" check="false"></dubbo:reference>
```

关闭所有消费者的启动时检查(统一规则)

```xml
<dubbo:consumer check="false"></dubbo:consumer>
```

关闭注册中心启动时检查

```xml
<dubbo:registry check="false" />
```

#### 通过dubbo.properties配置

```properties
dubbo.refenence.com.xx.XXService.check=false
dubbo.reference.check=false
dubbo.consumer.check=false
dubbo.registry.check=false
```

#### 通过虚拟机-D参数进行配置

```properties
java -Dubbo.dubbo.refenence.com.xx.XXService.check=false
java -Dubbo.reference.check=false
java -Dubbo.consumer.check=false
java -Dubbo.registry.check=false
```



## 2、超时检查

接口上设置超时时间

```xml
<dubbo:reference interface="xx" timeout=3000 ></dubbo:reference>
```

消费者统一设置

```xml
<dubbo:consumer timeout=3000 > </dubbo:consumer>
```

提供者统一设置

```xml
<dubbo:consumer timeout="300"><dubbo:consumer>
```

方法上配置超时时间

```xml
<dubbo:methord timeout="1000">
```

**覆盖策略：方法及有限，接口级次之，全局配置再次之**

**消费方有限，提供方次之**

- 单位：毫秒
- 默认1000毫秒 
- 可以在方法上进行设置
- 重置次数
- 多版本
- 本地存根

## 3、重试次数

重试次数 不包含第一次调用

```xml
<dubbo:reference retries="3" timeout="3000">
```

多个服务提供商 自动重试同名的其他服务

幂等操作（在幂等操作上尝试尝试次数：查询、删除、修改） 非幂等操作（新增操作）

幂等操作：多次操作，效果一致

非幂等：retries=0 不重试

## 4、多版本

一个接口  有两个或者两个以上版本的实现版本

```xml
<dubbo:service interface="XXService" ref="XXService01" version="1.0.0" ref="XXImpl01"></dubbo:service>
<bean id="XXImpl01" class="XXServiceImpl"></bean>

<dubbo:service interface="XXervice" ref="XXService02" version="2.0.0" ref="XXImpl02"></dubbo:service>
<bean id="XXImpl02" class="XXServiceImpl"></bean>
```

调用时

```xml
<dubbo:reference interface="XXService" version="1.0.0">
<dubbo:reference interface="XXService" version="2.0.0">
<dubbo:reference interface="XXService" version="*">  #同时上线
```

## 5、本地存根

在服务消费方，写一个远程接口的本地接口实现（存根实现）

必要要有一个有参构造器，有参构造器的参数是远程接口的代理实现

UserserviceStub.java

##### 实现一个存根

```java
public class UserServiceStub implements UserService {

    private final UserService userService;

    //传入的是 远程代理对象
    public UserServiceStub(UserService userService) {
        this.userService = userService;
    }

    @Override
    public List<UserAddress> getUserAddressList(String userId) {
        if(StringUtils.isEmpty(userId)){
            return userService.getUserAddressList(userId);
        }
        return null;
    }
}
```

##### 配置存根

可以在服务提供方配置

```xml
<dubbo:service interface="com.xx.XXService" stub="true"/>
```

或者

```xml
<dubbo:service interface="com.xx.XXService" stub="com.xx.XXServiceStub"/>
```

也可以在服务消费方配置

```xml
<dubbo:reference interface="xx" stub="com.xx.XXServiceStub" />
```



## 6、SpringBoot方式配置

```java
@Service(timeout="3000" retries="3")
```

```java
@reference(version = "1.0.0",check = true)
```



### 7、SpringBoot与dubbo整合的三种方式

#### 方式一

- 导入dubbo-starter;
-  在application.properties中配置dubbo属性；
- 使用@Service暴露服务；
- 使用@Reference引用服务；
- 需要在主启动类添加@EnableDubbo注解开启dubbo
  - 是为了指定包扫描规则
  - 也可以在application.properties中配置包扫描规则
  - dubbo.base-packages="com.xx.service" 就可以不用再主启动类配置注解@EnableDubbo
#### 方式二

​	如果想保留方法级别的精确配置

- 导入dubbo-starter;

- 保留dubbo配置文件（provider.xml consumer.xml），
- 不用在application.properties中配置任何dubbo相关的的东西；
- 在启动类中增加注解：@ImportResource(locations="classpath:provider.xml")
- 暴露Service不用注解
#### 方式三

- 使用注解API方式 

- 使用配置类来进行配置

  将每一个组件创建到容器中
```java
package com.beyondsoft.gmall.config;

@Configuration
public class MyDobboConfig {

      public ApplicationConfig applicationConfig(){        
      		ApplicationConfig application =new ApplicationConfig();
      		application.setName("boot-user-service-provider");
      		return applicationConfig;
      }
        
      public RegistryConfig registryConfig(){
          RegistryConfig registry = new RegistryConfig();
          registry.setProtocol("zookeeper");
          retistry.setAddress("127.0.0.1:2181");
          return registry;
      }

   	public ProtocolConfig protoConfig(){
  		ProtocolConfig protocolConfig = new ProtocolConfig();
        protocolConfig.setName("dubbo");
        protocolConfig.setPort(20880);
        return protocolConfig;
  	}

  	public ServerConfig<UserService> userServiceConfig(UserService userService){
        ServiceConfig<UserService> serviceConfig = new ServiceConfig<>();
        serviceConfig.setInterface(UserService.class);
        serviceConfig.setRef(userService);
        serviceConfig.setVersion("1.0.1");
        
        MethodConfig methodConfig = new MethodConfig();
        methodConfig.setName("getUserAddressList");
        methodConfig.setTimeout();
        
        List<MethodConfig> listMethodConfig = new ArrayList<MethodConfig>();
        listMethodConfig.set(methodConfig);
        serviceConfig.setMethods(listMethodConfig);
    }
}
```

- 指定Dubbo的Service扫描路径
  - @DubboComponentScan 在主启动类上
  - 跟@EnableDubboConfig 注解是一样的 用哪个都可以
  - 示例：@DubboComponentScan(scanBasePackages=“com.beyondsoft.gmall”)
- 使用@Service暴露服务
- 使用@Reference引用服务

# 三、Dubbo高可用

## 1、Zookeeper宕机与Dubbo直连

zookeeper注册中心宕机后，消费方仍可以使用dubbo暴露的服务

原因有本地缓存，记录着服务方的地址和端口

##### dubbo直连

使用 @Reference(url="127.0.0.1:20882") 绕过注册中心

```java
public class OrderServiceImpl implements OrderService{}
	@Reference(url="127.0.0.1:20882")
	private UserService userService;

	public List<UserAddress> initOrder(String userId){
        ...
    }
}
```

## 2、集群下Dubbo 负载均衡配置

#### 2.1 四种负载均衡机制

​	在集群复杂均衡是，Dubbo提供了多种负载均衡策略，缺省为随机方式调用

- ##### Random LoadBalance 随机

  按权重设置随机概率 ，可以设置权重 weight=100

- ##### RoundRobin LoadBalance 轮询

  轮询，按公约后的权重设置循环比例

- ##### LeastActive LoadBalance 最小活跃数

- ##### ConsitenHash LoadBalance 一致性哈希



#### 	2.2 修改默认的负载均衡机制

​	服务端 设置负载均衡

```xml
<dubbo:service interface="..."  loadbalance="roundrobin" />
```

​	服务消费方 客户端设置负载均衡算法

```xml
<dubbo:reference interface="..."  loadbalance="roundrobin" />
```

​	服务方法 设置负载均衡算法

```xml
<dubbo:service interface="..." >
    <dubbo:method name="..." loadbalance="roundrobin" />
</dubbo:service>
```

#### 2.3 dobbo-admin 调整权重

使用dobbo-admin 可以动态调整权重

倍权

半权



## 3、Dubbo服务降级

#### 什么是服务降级

当服务器压力剧增的情况下，根据实际业务情况以及流量，对一些服务和页面有策略的不处理或者换种简单的处理方式，从而释放服务器资源以保证核心交易正常云南做或高效运行。

可以通过服务降级功能临时屏蔽某个出错的非关键服务，并定于降级后的返回策略。

向注册中心希尔动态配置覆盖规则：

**dubbo支持两种服务降级：**

- mock=force:return+null 强制返回为null
- mock=fail:return+null 调用失败后返回为null

如何设置？

​	屏蔽：利用dubbo管理控制台dubbo-admin，在消费者侧，进行屏蔽

​	容错：在调用失败后，返回为空，在dubbo-admin 消费者侧 点击 容错按钮

## 4、集群容错

#### 4.1 Dubbo支持的集群容错

- ##### Failover Cluster 失败自动切换

- ##### Failfast Cluster 快速失败

  只发起异常调用，失败立即报错，通常用于非幂等性的写操作，比如新增记录

- ##### Fallback Cluster 失败自动恢复

- ##### Forking Cluster 并行调用多个服务器

  并行调用多个服务器，只要有一个成功即返回，通常用于实时性要求较高的读操作，但是需要浪费更多服务器资源，可以通过forks="2" 来设置最大并行数

- ##### Broadcast Cluster 广播调用所有提供者

  逐个调用

#### 4.2 集群模式配置

可以在服务提供方和服务消费方配置集群模式

```xml
<dubbo:server cluster="failsafe" />
或者
<dubbo:reference cluster="failsafe"/>
```




## 5、整合Hystrix服务容错

##### 1、配置spring-cloud-starter-netflix-hystrix

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
    <artifictId>spring-cloud-starter-netflix-hystrix</artifictId>
    <version>1.4.4.RELEASE</version>
</dependency>
```

##### 2、主启动类增加@EnableHystrix启动Hystrix starter

```java
@SpringBootApplication
@EnableHystrix
public class ProviderApplication{
	...
}
```

##### 3、配置Provider端

在Dubbo的Provider上增加@HystrixCommond配置，这样调用会经过Hystrix代理

```java
@HystrixCommond
```

##### 4、配置Consumer消费者端

```
@Reference
UserService userService;

@HystrixCommand(fallbackMethod="initOrderFallback")
public List<UserAddress> initOrder(String userId){
	List<UserAddress> addressList = userService.getUserAddressList(userId);
	return addressList;
}
//出错时调用
public List<UserAddress> initOrderFallback(String userId){
	//return null;
	//或者
	return Arrays.asList(new UserAddress(10,"测试地址")，“1”，“测试”,“测试”,"Y");
}
```



# 四、DUbbo原理

## 1、RPC原理

![](https://images2015.cnblogs.com/blog/522490/201510/522490-20151003120412386-363334260.png)

一次完整的RPC调用流程（同步调用）如下：

1）服务消费方（client）的调用是以本地调用方式调用服务；

2）client stub（代理）接收到调用后负责将方法、参数等组装成能够进行网络传输的消息体；

3）client stub找到服务地址，并将消息发送到服务端；

4）server stub收到消息后进行解码；

5）server stub根据解码结果调用本地的服务；

6）本地服务执行并将结果返回给server stub；

7）server stub将返回结果打包成消息并发送至消费方；

8）client stub接收到消息，并进行解码；

9）服务消费方得到最终结果。

## 2、Netty通信原理

Netty是一个异步事件驱动的网络应用程序框架，用于快速开发可维护的高性能协议服务器和客户端，它极大地简化并简化了TCP和UDP套接字服务等网络编程。

**BIO（Blocking IO阻塞式IO）**

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1597461509341&di=84f3e293b22a4ef1ac4a45efeb9b1bb4&imgtype=0&src=http%3A%2F%2Fimg2020.cnblogs.com%2Fblog%2F292888%2F202007%2F292888-20200701091344790-1141479678.png)

**NIO（Non-Blocking IO非阻塞式IO）**

多路复用模型

![](http://dl2.iteye.com/upload/attachment/0095/0309/21c0d590-0547-386e-ab24-ce1cf7bf3283.png)


```
Selector 一般称为选择器 也可以翻译为 多路复用器
监听状态：Connect连接、Accept接手就绪、Read读就绪、Write写就绪
```

**Netty基本原理**





## 3、Dubbo原理

##### 3.1 Dubbo原理 -框架设计

![](http://dubbo.apache.org/docs/zh-cn/dev/sources/images/dubbo-framework.jpg)

##### 3.2 Dubbo原理-启动解析、加载配置



##### 3.3 Dubbo原理-服务暴露

##### 3.4 Dubbo原理-服务引用

##### 3.5 Dubbo原理-服务调用



