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

![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=962964185,1176401230&fm=11&gp=0.jpg)

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

Dubbo-OPS

mva package 打包

**使用监控中心**

消费者和提供者 的配置文件：

```xml
<dubbo:monitor protpcal="registry"></dubbo:monitor>
```



## 6. 整合SpringBoot

##### 1、创建模块

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


##### 3、主程序中启动

​		开启dubbo

##### 4、配置映射规则

##### 5、属性覆盖策略

- 虚拟机参数配置： -Dubbo.protocol.port=20880
- XML文件配置：dubbo.xml
- Properties属性配置：dubbo.properties

# 二、Dubbo配置

### 1、启动时检查

#### 通过spring配置文件配置

关闭某个消费者启动时检查

```
<dubbo:reference interface="xx" check="false"></dubbo:reference>
```

关闭所有消费者的启动时检查(统一规则)

```
<dubbo:consumer check="false"></dubbo:consumer>
```

关闭注册中心启动时检查

```
<dubbo:registry check="false" />
```

#### 通过dubbo.properties配置

```
dubbo.refenence.com.xx.XXService.check=false
dubbo.reference.check=false
dubbo.consumer.check=false
dubbo.registry.check=false
```

#### 通过虚拟机-D参数进行配置

```
java -Dubbo.dubbo.refenence.com.xx.XXService.check=false
java -Dubbo.reference.check=false
java -Dubbo.consumer.check=false
java -Dubbo.registry.check=false
```



### 2、超时检查

接口上设置超时时间

```
<dubbo:reference interface="xx" timeout=3000 ></dubbo:reference>
```

消费者统一设置

```
<dubbo:consumer timeout=3000 > </dubbo:consumer>
```

提供者统一设置

```
<dubbo:consumer timeout="300"><dubbo:consumer>
```

方法上配置超时时间

```
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

### 3、重试次数

重试次数 不包含第一次调用

```
<dubbo:reference retries="3" timeout="3000">
```

多个服务提供商 自动重试同名的其他服务

幂等操作（在幂等操作上尝试尝试次数：查询、删除、修改） 非幂等操作（新增操作）

幂等操作：多次操作，效果一致

非幂等：retries=0 不重试

### 4、多版本

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

### 5、本地存根

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



### 6、SpringBoot方式配置

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

# 三、Dubbo高可用

#### Zookeeper宕机与Dubbo直连

```java
@Reference(url="127.0.0.1:20882")
```

#### Dubbo集群下 负载均衡策略

- Random LoadBalance 随机
- RoundRobin 轮询
- LeastActive最小活跃数
- 一致性哈希
- 

### Dubbo服务降级

### 服务容错



# 四、DUbbo原理


```