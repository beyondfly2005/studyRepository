> 视频  

https://www.bilibili.com/video/BV18E411x7eT?p=73

> 文档

https://www.cnblogs.com/h--d/category/1047453.html

SpringCloud中文文档

官方文档

https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/

中文文档

http://www.bookstack.cn/read/spring-cloud-docs/docs-index.md

## 1. SpringCloud微服务架构简介

## 2. Boot和Cloud版本选型

## 3. 关于Cloud各组件的停更/升级/替换

## 4. 微服务架构编码构建

### 项目工程搭建

```
<dependencyManagement>
提供了一种版本依赖版本好的方式
一般用在父工程 

子模块复用父pom的版本号；如果子模块指明了不同于父pom的版本号，会使用子项目中的版本号

只负责定义，指定版本号，并不引入

```

mavne





```bash
1-建module
2-改POM
3-写YML
4-主启动类
5-业务类
```

#### SpringBoot IDEA热部署

```bash
1- pom添加devtool （Adding devtools to yourproject）
2- 父工程中增加maven插件 （Adding plugin to your pom.xml）
3- 开启自动编译构建 Enabling automatic build
	Settings -> Build Execution DEployment -> Compiler 选项ADBC开头的选择都打勾
4- 	热注册开启（Update the value of） ctrl+alt+shift+/  -> registry..  ->  	
	compiler.auto.alow.when.app.running 打勾
	actionSystem.assertFocusAccessFromEdt 打勾
5- 重启IDEA生效
```

#### 消費者订单模块

#### 工程重构

```bash
系统中有重复的部分  entities 
新建 cloud-api-commons 工程
pom
entities
maven 命令 mvn clean install
两个项目改成
	删除entities
	增加对api-common工程的依赖

```

## 5. Eureka服务注册与发现

#### Eureka基础知识

```bash
常用的
Eureka
Zookeeper
Consul
Nacos
```
##### 什么是Eureka
Eureka是一项基于REST（代表性状态转移）的服务，主要在AWS云中用于定位服务，以实现负载均衡和中间层服务器的故障转移。我们称此服务为Eureka Server。Eureka还带有一个基于Java的客户端组件Eureka Client，它使与服务的交互更加容易。客户端还具有一个内置的负载均衡器，可以执行基本的循环负载均衡。在Netflix，更复杂的负载均衡器将Eureka包装起来，以基于流量，资源使用，错误条件等多种因素提供加权负载均衡，以提供出色的弹性。

Git地址：https://github.com/Netflix/eureka

服务注册中心
服务注册与发现

##### 1- 什么是服务治理

SpringCloud封装了NetFlix公司开发的Eureka模块来实现服务治理
在传统的RPC远程调用框架中，管理每个服务于服务之间的依赖关系比较复杂，，所以需要使用服务治理，用于管理服务之间的依赖关系，可以实现服务调用、负载均衡、容错等，实现服务注册与发现

##### 2- 什么是服务注册

Eureka采用了CS的设计架构，EurekaServer作为服务注册功能的服务器，它是服务注册中心
而系统中的其他微服务，使用Eureka的客户端连接到Eureka Server并维持心跳连接，这样系统的维护人员就可以通过Eureka来监控系统中各个微服务是否正常运行。

在服务注册于发现中， 有一个注册中心，当服务器启动的时候，会把当前自己服务器的信息，比如服务器地址通讯地址以别名方式注册到中心上，另一方（消费者|服务提供者）以改别名方式去注册中心上获取到时间的服务通讯地址，然后再实现本地RPC调用。RPC远程调用框架核心设计思想：在于注册中心，因为使用注册中心管理每个服务与服务直接的一个依赖关系（服务治理概念）。在任何RPC远程框架中，都会有一个注册中心（存放服务地址相关信息（接口地地址））

**Eureka架构图**

![](https://img2020.cnblogs.com/blog/851491/202004/851491-20200405113917606-952855327.png)

**Dubbo的架构图**

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1596628442753&di=f17edc77dccaa201ce620a784edc8d51&imgtype=0&src=http%3A%2F%2Fimages3.10qianwan.com%2F10qianwan%2F20180710%2Fb_0_201807101923179783.jpg)

##### 3- Eureka的两大组件

Eureka的两个组件：Eureka Server和EurekaCliend

###### Eureka Server提供注册服务

Eureka Server提供服务注册服务，各个微服务节点通过配置启动后，会在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

###### Eureak Client 用于连接到服务

Eureka Client通过注册中心进行访问是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的、使用轮询（round-robin）负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳（默认周期为30秒）。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳。Eureka Server将会从服务注册表中把这个服务节点移除（默认90秒）

4-

#### 单机Eureka构建步骤

###### 搭建Eureka注册中心

###### 搭建支付模块、服务提供者

###### 搭建订单模块、服务消费

#### 集群Eureka构建步骤

#### Actuator微服务信息完善

#### 服务发现Discoverry

#### Eureka自我保护


```

#### Eureka停更

Eureka（Discontinued） 停更

​```bash
http://github.com/Netflix/eureka/wiki
```

## 6. Zookeeper服务注册与发现

#### Zookpe代替Eureka

```
Zookeeper是一个分布式协调工具，可以实现注册中心的功能
需要关闭Linux服务器防火墙后启动zookeeper服务器
zookeepe服务器取代Eureka服务器，zookeeper作为服务注册中心
```

zookeeper操作

```
# 进入zookee安装目录
cd /usr/local/zookeeper

# 停止防火墙
systemctl stop firewwalld

# 查看防火墙状态
systemctl status firewwalld

# 查看本机ip(zookeeperip)
ifconfig

# 客户端测试与zookeeper是否连通
ping ip

# 进入zookeeper程序目录 
cd bin

# 启动zookeeper
./zkServer.sh start

# 进入zookeeper配置目录
# cd conf

# 从配置信息zookeeper查看端口号
# clientPort=2181

# 进入zookeeper客户端
./zkCli.sh 

# 查看根下的节点
ls /

# 查看zookeeper节点信息
get zookeeper

# 查看zookeeper子节点信息
ls /zookeeper
```

Zookeeper 节点

- 临时节点

- 持久节点

 细分

- 带序号的临时节点



zookeeper使用的是临时节点，检测不到服务心跳信号，就会删掉服务注册信息

  

## 7. Consul服务注册与发现

官网 http://consul.io

- 服务发现
- 配置管理

GasguCoro公司用Go语言开发的

服务治理、配置中心、控制总线

广域网集群

- 服务注册于发现
- 检测检查
- KV存储
- 多数据中心
- 可视化Web界面

#### Consul   怎么玩

https://www.springcloud.cc/spring-cloud-consul.html

#### 安装 Consul

learn.hashicorp.com/consul/getting-started/install.html

consul.exe文件，双击运行 查看版本号

```bash
consul --version
consul agent -dev
http://localhost:8500

```

CentOS 安装

> 参考手册 
>
> https://www.jianshu.com/p/266f4c8c265c
>
> https://blog.csdn.net/a312586670/article/details/105337943/

```bash
cd /usr/local
wget https://releases.hashicorp.com/consul/1.7.2/consul_1.7.2_linux_amd64.zip
# yum -y install wget

unzip  consul_1.7.2_linux_amd64.zip
mv consul /usr/local

# 查看是否安装成功
./consul

# 启动
./consul agent -server -bind=192.168.198.128 -client=0.0.0.0 -bootstrap-expect=1 -data-dir=/usr/data/consul_data -node=server1
./consul agent -server -bind=192.168.198.128 -client=0.0.0.0 -bootstrap-expect 1 -data-dir=/tmp/consul -node=n1 -ui
# 或者
./consul agent -dev -ui -node=consul-dev -client=192.168.198.128

# 或者
./consul agent -data-dir /tmp/node0 -node=node0 -bind=192.168.198.128 -datacenter=dc1 -ui -client=192.168.198.128 -server -bootstrap-expect 1

# 浏览器访问UI界面
http://ip:8500
http://92.168.198.128:8500

# 如果不能访问
# 一定要停止防火墙 停止防火墙
systemctl stop firewalld

```



## 8. Ribbon 客户端负载均衡工具

Ribbon目前进入维护状态

替换方案：SpringCloud Loadbalancer

LBalace

Nginx LVS 硬件F5

- 进程内的LB

- 集中式的LB 

Nginx是服务器端负载均衡，Ribbon是本地负载均衡 它是一个类库 消费方

负载均衡+RestTemplate工具调用

Ribbon是一个软负载均衡组件





netflix-eureka-client 集成了ribbon 可以不用在pom文件 添加

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
    <artifactId> spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
```



#### Ribbon核心组件IRule

##### Ribbon其他负载均衡算法 

IRule接口，实现类

- RoundRobbinRule 轮询

- RandomRule 随机

- RetryRule  重试

- TimeRule

- BestAvailableRule

- Availability
- ZoneAvoidanceRule 默认规则，复合判断

#####  负载规则如何替换

- **注意配置细节**

  自定义配置类不能放到 @ComponentScan包或子包下

  @SpringBootApplication 注解自带依赖@ComponentScan注解

- 定义 com.beyodnsoft.myrule  MySelf 规则类


```java
@Configuration
public class MySelfRule(){
    
    @Bean
    public IRule myRule(){
        return new RandomRule();	//改为随机
    }
}
```


- 主启动类添加@RibbonClinet

```
@RibbonClient(name="CLOUD-PAYMENT-SERVICE",configuration = MySelfRule.class)
```

- 测试

```
http://localhost/consumer/payment/get/1
```



#### Ribbon负载均衡算法

##### 原理：

```
负载均衡算法：Rest接口的第几次请求% 服务器集群总数据量=实际服务器位置的下标，每次服务重新启动后rest接口从1开始。
List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
如： List[0] instance = 127.0.0.1:8002
    List[1] instance = 127.0.0.1:8001
    
8001 + 8002 组合成为集群，他们一共2台服务器，集群总是2，按照轮询算法原理：

第1次请求时：1 % 2=1 对应下标位置为1，获得服务器地址为127.0.0.1:8001
第2次请求时：2 % 2=0 对应下标位置为1，获得服务器地址为127.0.0.1:8002
第3次请求时：3 % 2=1 对应下标位置为1，获得服务器地址为127.0.0.1:8001
第4次请求时：4 % 2=0 对应下标位置为1，获得服务器地址为127.0.0.1:8002
以此类推......

```



##### 源码：

```java
public Interface IRule{
    Server choose(Object key);
    void setLoadBalancer(ILoadBalancer lb);
    ILoadBlancer getLoadBalancer();
}

public abstract class AbstractLoadBalancerRule implements IRule, IClientConfigAware {
    private ILoadBalancer lb;
    public AbstractLoadBalancerRule() {
    }
    public void setLoadBalancer(ILoadBalancer lb) {
        this.lb = lb;
    }
    public ILoadBalancer getLoadBalancer() {
        return this.lb;
    }
}

public class RoundRobinRule extends AbstractLoadBalancerRule {
    public Server choose(ILoadBalancer lb, Object key) {
        if
}

```

###### 自旋锁



##### 手写负载均衡

```java
@Componet
public class MyLb implements LoadBalancer{
    
    private ;
    
    public final int getAndIncrement(){
        int current;
        int next;
        do {
            current = this.atomicInteger.get();
            next = current>=21474836447 ? 0 :current + 1;
        } while(!this.atomicInteger.compareAndSet(current,next));
        System.out.println("******第几次访问，次数next:"+next);
        return next;
    }
	

    @OVerride
    public ServiceIntance intances(List<ServiceInstance> serviceInstances){
        int index = getAndIncrement() % serviceInstances.size();
        return serviceInstances.get(index);
    }
}
```

## 9. Feign和OpenFeign服务接口调用

#### 概述

##### OpenFeign是什么

Feign是一个声明式的WebService客户端，使用Feign能让编写WebService客户端更加简单。

它的使用方法是顶一个服务接口然后在上面添加注解，Feign也支持可插拔式的编码器和解码器，SpringCloud对Feign进行了封装，使其支持了SpringMVC标准注解和HttpMessageConverters。Feign可以与Eureka和Ribbon组合使用以支持负载均衡。

###### GitHub地址

https://github.com/spring-cloud/spring-cloud-openfeign

##### Feign能干什么

Feign旨在使编写Java Http客户端变得更加容易

前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模板化的调用方法，但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多次调用，所以通常会针对每个微服务自行封装一些客户端类来包装这些依赖的调用，，所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义，在Feign的实现下，我们只需要创建一个接口并使用注解方式来配置它（以前是Dao接口上面标准Mapper注解，现在是在一个微服务接口上标注一个Feign注解接口即可），即可完成对服务提供方的接口绑定，简化了使用SpringClod Ribbon时，自动封装服务调用客户端的开发量

Feign继承了Ribbon

利用Ribon维护了Payment的服务列表信息，并通过轮询实现了客户端的负载均衡，而与Ribbon不同的是，通过Feign只需要定义服务绑定接口且以声明式的方法，优雅而简单的实现了服务调用。



##### Feign和OpenFeign两者的区别

| Feign                                                        | OpenFeign                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Feign是SpringCloud组件中的一个轻量级RESTFul风格的HTTP服务客户端.Feign内置了Ribbon，用来做客户端负载均衡，去调用服务注册中心的服务，Feign的使用方式是：使用Feign的注解定义接口，调用这个接口，就可以调用服务注册中心的服务 | OpenFeign是SpringCloud在Feign的基础上支持了SpringMVC的注解，如@RequestMapping等等。OpenFeign的@FeignClient可以解析SpringMVC的@RequestMapping注解下的接口，并通过低昂太代理的方式产生实现类，实现类中做负载均衡并调用其他服务。 |
| <dependency><groupId>org.springframework.cloud</groupId><artifictId>spring-cloud-starter-feign</artifictId></dependency> | <dependency><groupId>org.springframework.cloud</groupId><artifictId>spring-cloud-starter-openfeign</artifictId></dependency> |

#### OpenFeign使用步骤

##### 接口+注解@FeignClient

##### 代码示例

新建cloud-consumer-feign-order80 ； Feign用在消费端



##### POM增加Feign依赖

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>
```



##### 编写YML

```yaml
server:
  port: 80

spring:
  application:
    name: cloud-order-service-openfeign

eureka:
  client:
    register-with-eureka: true  #表示是否将自己注册进EurekaServer 默认为true
    fetch-registry: true # 是否从EurekaServer抓取已有的注册信息 默认为true 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    service-url:
      #defaultZone: http://localhost:7001/eureka
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
```



##### 主启动类增加@EnableFeignClient

```java
@SpringBootApplication
@EnableEurekaClients
@EnableFeignClient
public class OpenFeignMain80 {

    public static void main(String[] args) {
        SpringApplication.run(OpenFeignMain80.class,args);
    }
}
```



##### 业务类复制调用接口并增加@FeignClient

```java
@Component
@FeignClient(value="CLOUD-PAYMENT-SERVICE")
public interface PaymentService {

    @PostMapping(value = "/payment/create")
    public CommonResult  create(@RequestBody Payment payment);

}
```



##### 测试

##### 总结



#### OpenFeign超时控制

```
默认Feign客户端只等待1秒钟，但是服务处理需要1秒钟，导致Feign客户端不想等待了，直接报错，为了避免这样的情况，有时候我们需要设置Feigin客户端的超时控制。
报错：Read Timeout

在yml中开始配置
```

```
#设置feigin客户端超时时间（OpenFeign默认支持ribbon）
ribbon:
	ReadTime: 5000			#只的是建立连接所用的时间，适用于网络状况的情况下，两端所用的时间
	ConnectTimeout: 5000	#指的是建立连接后从服务器读取可用资源所用的时间
```



#### OpenFeign日志增加

##### 什么是日志打印功能

Feign提供了日志打印功能，我们可以通过配置调整日志级别，从而连接Feign中Http请求的细节；

简单来说就是对Feigin接口的调用情况进行监控和输出。

##### Feign日志级别

- NONE：默认的，不显示任何日志
- BASIC：仅记录请求方法、URL、响应状态码及执行时间
- HEADERS：除了BASIC中定义的信息外，还有请求和响应的头信息
- FULL：除了HEADERS中定义的信息之外，还有请求和响应的正文及元数据。

##### 配置日志bean

```java
@Configuration
public class FeignConfig{
    @Bean
    Logger.level FeignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```

##### YML文件中配置开启Feign客户端日志

```yaml
logging:
  level:
    # feign日志以什么级别监控哪个接口
    com.beyondsoft.springcloud.service.PaymentFeignService: debug
  
```

## 10. Hystrix断路器

#### Hystrix概述

##### 分布式系统面临的问题 - 服务雪崩

分布式系统面临的问题

复杂的分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免的失败

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其他的微服务，这就是所谓的“扇出”。如果扇出的某个微服务调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，即所谓的，“雪崩效应”

##### Hystrix是什么

Hystrix是一个用于处理分布式系统的演示和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，Hystrix能够保证在一个依赖出现问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

”断路器“本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），向调用方返回一个符合预期的、可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方法无法处理的异常，这样就保证了服务调用方的线程不会被长时间的占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

##### Hystrix能做什么

- 服务降级

- 服务熔断

- 接近实时的监控

- 限流、隔离

##### Hystrix官网资料

```
http://github.com/Netflix/Hystrix/wiki/How-To-Use
```

##### Hystrix 停更进维

​	停止更新，进入维护期

```
http://github.com/Netflix/Hystrix
```

官网推荐使用 resilience4j 替代

国内推荐Alibaba Sentinel实现熔断与限流

#### Hystrix重要概念

##### 服务降级（fallback）

服务器忙，请稍后再试，不容客户等待并like返回一个友好的提示，fallback

**哪些情况会发生服务降级**

- 程序运行异常
- 超时
- 服务熔断触发降级
- 线程池/信号量打满也会导致服务降级

##### 服务熔断（break）

达到最大服务访问量后，直接拒绝访问，类似保险丝拉闸限电，然后调用服务降级的方法并返回友好提示；

服务降级 -> 进而熔断 -> 恢复调用链路

##### 服务限流（flowlimit）

秒杀，高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行

#### Hystrix案例

##### 构建

###### 新建项目模块

cloud-provider-hystrix-payment8001

###### 修改pom文件

增加hystrix

```xml
<!-- hystrix -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

###### 配置yml文件

```yaml
server:
  port: 8001

spring:
  application:
    name: cloud-provider-hystrix-payment

eureka:
  client:
    register-with-eureka: true  #表示是否将自己注册进EurekaServer 默认为true
    fetch-registry: true # 是否从EurekaServer抓取已有的注册信息 默认为true 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    service-url:
      defaultZone: http://localhost:7001/eureka
      #defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
```

###### 创建主启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class PaymentHystrixMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentHystrixMain8001.class,args);
    }
}
```

###### 创建业务类



###### 正常测试



##### 高并发测试



##### 故障现象和导致原因

zX

#### Hystrix工作流程

#### 服务监控HystrixDashboard





## 11. Zuul路由网关

## 12. Gateway新一代网关

##### 概述简介

###### 什么是Gateway

###### Gateway的能做什么

- 反向代理
- 鉴权
- 流量控制
- 熔断
- 日志监控



###### 微服务架构中网关在哪里

![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=527452945,2036534054&fm=11&gp=0.jpg)



###### 为什么选择Gateway 替代Zuul

- **netflix 公司的zuul 2.0迟迟没有发布**

  zuul1.0进入了维护阶段 而且Gateway是Spring团队研发的 值得信赖

  很多功能zuul没有用起来，Gateway也比zuul更加简单便捷

  Gatway是基于异步非阻塞模型开发的，性能方面更强；虽然Netflix早就发布了Zuul 2.x，但是Spring Cloud貌似没有整合计划，而且Netflix相关组件进入维护期，前景堪忧

  多方面综合考量，Geteway是目前最理想的网关选择。

- **SpringCloud Gatewany具有如下特性：**

  基于Spring Framework5.0  Project Reactor 和Spring Boot2.0进行构建；

  动态路由：能够匹配任何请求属性；

  可以对路由指定 Predicate (断言) 绝Filter（过滤器）

  基础Hystrix的断路器功能；

  集成Spring Cloud服务发现功能

  易于编写Predicate (断言) 绝Filter（过滤器）

  请求限流功能；

  支持路径重写。

- **SpringCloud Gateway与Zuul的区别**

  在SpringCloud Fincheley正式版之前，Spring Cloud退佃的网关是Netflix提供的Zuul:

  1、Zuul1.x 是一个基于阻塞I/O的API Gateway

  2、Zuul1.x基于Servlet2.5使用阻塞架构 它不支持任何长连接（如：WebSocket），zu的设计和Nginx较像，每次I/O都是从工作线程中选择一个执行，请求线程被阻塞到工作线程完成，但是差别是Nginx是C++实现的，Zuul是Java实现的，JVM本身会有第一次加载较慢的情况，是的Zuul的性能相对较差。

  3、Zuul2.x理念更先进，想基于Netty非阻塞和支持长连接，但是SpringCloud目前还没有整合，Zuul 2.x的性能较Zuul 1.x有加大提升，在性能方面，根据官方提供的基准测试，Spring Cloud Gateway的RPS（每秒请求数）是Zuul的1.6倍。

  4、Spring Cloud Gateway建立在Spring Framework5 、Project 和Spring Boot2.0之上，使用非阻塞API。

  5、Spring Cloud Gateway还支持WebSocket，并且与Sping紧密集成，拥用更好的开发体验。

###### Zuul 1.0模型

Spring Cloud 中所集成的Zuul版本，采用的是Tomcat容器，使用的是传统的Servlet IO处理模型。
Servlet 的生命周期是由servlet container进行生命周期管理。
container 启动时构造servlet对象并调用servlet init（）进行初始化
container运行时接受请求，并为每个请求分配一个县城（一般从线程池中获取空闲线程）然后调用service（）
container关闭时调用servlet destory（）销毁servlet

![](https://img-blog.csdnimg.cn/202006041304137.png)

上述模式的缺点：
servler是一个简单的网络IO模型，当请求进入servlet container时，servlet container 就会为其绑定一个线程，在并发不高的场景下这种模型是使用的，但是一旦 高并发（比如使用jemeter压测）线程数量就会上涨，而线程资源代价是昂贵的（上下文切换，内存消耗大）严重影响请求的处理时间，在一些简单业务场景下，不希望为每个request分配一个线程，只需要1个或几个线程就能应对极大并发的请求，这种业务场景下servlet模型没有优势

所以Zuul 1.x是基于servlet之上的一个阻塞式处理模型，即spring实现了处理所有request请求的一个servlet（DispatcherServlet）并由该servlet阻塞式处理处理，所以Spring Cloud Zuul无法摆脱Servlet模型的弊端。

###### Geteway模型

WebFlux是什么？

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020060413103775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604131044545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)

传统的Web框架，比如说：Struts2、SpringMVC等都是基于Servlet API与Servlet容器基础之上运行的。但是，在Servler3.1之后有了异步非阻塞的支持。而WebFlux是一个典型非阻塞异步的框架，它的核心是基于Reactor的相关API实现的。相对于传统的Web框架来说，他可以运行在诸如Netty、Undertow及支持Servlet3.1的容器上。非阻塞时+函数式编程（Spring5必须让你使用java8）

Spring WebFlux是Spring 5.0 引入的新的响应式框架，区别于Springmvc ，它不需要依赖Servlet API，它是完全异步非阻塞的，并基于Reactor来实现响应式流规范。

##### 三大核心概念

**Route(路由)：**
路由是构建网关的基本模块，它由ID，目标URI，一系列的断言和过滤器组成，如果断言为true则匹配该路由

**Predicate（断言）：**
参考的是java8的java.util.function.Predicate开发人员可以匹配HTTP请求中的所有内容（例如请求头或请求参数），如果请求与断言相匹配则进行路由

**Filter(过滤)：**
指的是Spring框架中GatewayFilter的实例，使用过滤器，可以在请求被路由前或者之后对请求进行修改。

总体：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020060413151794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)

##### Gateway工作流程

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604131558140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)
客户端向Spring Cloud Gateway发出请求。然后在Gateway Handler Mapping 中找到与请求相匹配的路由，将其发送到Gateway Web Handler。

Handler再通过制定的Filter过滤器链来将请求发送到我们实际的服务执行业务逻辑，然后返回

过滤器之前用虚线分开是因为过滤器可能会在发送代理请求之前“pre”或之后“post” 执行业务逻辑

Filter在“pre类型的过滤器可以做参数校验、权限校验、流量控制、日志输出、协议转换等。

在“post”类型的过滤器中可以做响应内容、响应头的修改、日志的输出、流量监控等有着非常重要的作用”

###### 核心逻辑：

路由转发+执行过滤器链

##### 入门配置网关

###### 新建项目：

cloud-gateway-gateway9527

###### pom文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.liang.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-gateway-gateway9527</artifactId>
    <dependencies>
        <!--新增gateway-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

###### yml：

```yml
server:
  port: 9527
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      routes:
      - id: payment_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
        uri: http://localhost:8001   #匹配后提供服务的路由地址
        predicates:
          - Path=/payment/get/**   #断言,路径相匹配的进行路由

      - id: payment_routh2
        uri: http://localhost:8001
        predicates:
          - Path=/payment/lb/**   #断言,路径相匹配的进行路由

eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka


eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
 
```

###### 启动类：

```java
package com.liang.cloud;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class GateWayMain9527 {
    public static void main(String[] args) {
        SpringApplication.run( GateWayMain9527.class,args);
    }
}
```



###### 9527网关如何做路由映射？

cloud-provider-payment8001看看controller的访问地址
get 接口

lb接口

我们目前不想暴露8001端口，希望在8001外面套一层9527

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604132946916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)

填坑：

Gateway模块 不需要依赖web和actuator，在pom文件中移除。

测试：

启动7001

启动8001

启动9527网关



![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604133322417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)

添加网关前
http://localhost:8001/payment/get/31
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604133332783.png)
添加网关后
http://localhost:9527/payment/get/31
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604133340794.png)
访问说明：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604133148169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)



##### YML配置说明

###### Gateway网关路由有两种配置方式

- 在配置文件yml中配置

- 代码中注入RouteLocator的Bean:



通过9527网关访问到外网的百度新闻网址
新建配置类：
GateWayConfig ：

```java
package com.liang.cloud.config;

import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class GateWayConfig {

    @Bean
    public RouteLocator toBaiduRouteLocator(RouteLocatorBuilder routeLocatorBuilder) {
        RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
        routes.route("to_baidu_guonei", 
                     r -> r.path("/guonei").uri("http://news.baidu.com/guonei")).build();
        return routes.build();
    }

    @Bean
    public RouteLocator toQQRouteLocator(RouteLocatorBuilder routeLocatorBuilder) {
        RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
        routes.route("to_qq", r -> 
                     r.path("/d/bj").uri("https://new.qq.com/d/bj")).build();
        return routes.build();
    }
}
```

访问
http://localhost:9527/guonei
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020060413383353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)
http://localhost:9527/d/bj

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604133905285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)



##### 通过微服务名实现动态路由

默认情况下Gateway会根据注册中心的服务列表，以注册中心上微服务名为路径创建动态路由进行转发，从而实现动态路由的功能

**启动服务**

​	Eureka7001、微服务提供者8001、8002

**POM文件**

**YML文件**

```yaml
sping:
  cloud:
    gateway:
      discovery:
        locatior:
          enable:true # 开启从注册中心动态创建路由功能，利用微服务名进行路由

```

需要注意的是uri的协议为lb，表示启用Gateway的负载均衡功能。

lb://serviceName是spring cloud gateway在微服务中自动为我们创建的负载均衡uri

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604134050792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)

测试：

http://localhost:9527/payment/lb/

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604134513118.png)
报错，因为我这里![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604134530905.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjMxMzE4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020060413453722.png)
访问路径为payment/payment/lb

修改yml：
\- Path=/payment/payment/lb/** #断言,路径相匹配的进行路由

访问：
http://localhost:9527/payment/payment/lb
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604134618535.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604134624580.png)
8001/8002两个端口切换



##### Predicate的使用

###### Predicate是什么



###### Route Predicate factories是什么



###### 常用的Route Predicate

- After Route Predicate

  ```yml
   spring:
     cloud:
       gateway:
         routes:
           - id: after_route
             uri: http://localhost:80017           
             predicates:
               - After=2020-04-20T23:57:57.308+08:00[Asia/Shanghai]
  ```

  测试请求命令：curl http://localhost:9527/payment/get/1

  

- Before Route Predicate

  ```yaml
  spring:
    cloud:
      gateway:
        routes:
          - id: before_route
            uri: http://localhost:80017           
            predicates:
               - Before=2020-04-21T23:57:57.308+08:00[Asia/Shanghai]
  ```

- Between Rout

  ```
  
  ```

- Cookie Route Predicate

  ```
  
  ```

- Header Route Predicate

  ```
  H
  ```

- Host Route Predicate

  ```
  -Host=**.baidu.com
  ```

- Method Route Predicate

  ```
  Method=GET
  ```

  

- Path Route Predicate

- Query  Route Predicate

测试发送web请求

```
jmeter
postman
curl
```

curl 





##### Filter的使用

###### 什么是Gateway Filter 

Gateway Filter Factories 路由过滤器可用于修改进入HTTP请求和返回的HTTP响应，路由过滤器只能指定路由进行使用

Spring Cloud Gateway 内置了多种过滤器，他们都由GatewayFilter的工厂类来产生

###### Spring Cloud Gateway的Fliter

**生命周期：pre之前 post之后**

Filter，在“pre”类型的过滤器可以做参数校验、权限校验、流量监控、日志输出、协议转换等，在“post”类型的过滤器中可以做响应内容、响应头的修改，日志的输出、流量监控等，有非常重要的作用

**种类：GatewayFilter单一的 GlobalFilter全局的**

Spring Cloud Gateway中，Filter从作用范围可分为另外两种，一种是针对于单个路由的Gateway Filter，它在配置文件中的写法同predict类似；另外一种是针对于所有路由的Global Gateway Filer。现在从作用范围划分的维度来讲解这两种Filer。

**单个路由的Gateway Filter用法**

参考官网

https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/#the-addrequestparameter-gatewayfilter-factory

https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.2.RELEASE/reference/html/#gatewayfilter-factories

**Global Gateway Filer用法**

当请求与路由匹配时，过滤Web处理程序会将的所有实例GlobalFilter和所有特定GatewayFilter于路由的实例添加到过滤器链中。该组合的过滤器链按org.springframework.core.Ordered接口排序，您可以通过实现该getOrder()方法进行设置。

　　由于Spring Cloud Gateway区分了执行过滤器逻辑的“前”阶段和“后”阶段，因此优先级最高的过滤器是“前”阶段的第一个，而“后”阶段的最后一个。

###### 常用的Gateway Filter



###### 自定义过滤器

```java
@Component
@Slf4j
public class MyLogGateWayFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("=========Come in MyLogGateWayFilter: " + new Date() + "==========");
        String uname = exchange.getRequest().getQueryParams().getFirst("uname");
        if(uname == null) {
            log.info("=========用户名为null，非法用户========");
            exchange.getResponse().setComplete();
        }
        // 成功
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return 0;
    }
}
```



## 13. Spring Cloud Config 分布式配置中心

### 概述

#### 分布式系统面临的问题

微服务意味着要将单体应用中的业务拆分成一个个子服务，每个服务的粒度相对较小，因此系统中出现了大量的服务，由于每个服务都需要必要的配置信息才能运行，所以一套集中式的、动态的配置管理设置是必不可少的。

SpringCloud提供了ConfigServer来解决这个问题，我们每个微服务自己都带着一个application.yml，上百个配置文件管理... ...

#### Spring Cloud Config是什么

Spring Cloud Config为分布式架构中的微服务提供集中化的外部配置支持。配置服务器为各个不同微服务应用的所有环境提供了一个中心化的的外部配置

#### Spring Cloud Config 能做什么

- 集中管理配置文件
- 不同环境不同配置，动态化的配置更新，分环境部署比如dev/test/prod/beta/release
- 运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件，服务会想配置中心统一拉取配置自己的信息
- 当配置发送变化是，服务不需要重启即可感知到配置的变化并应用新的配置
- 将配置信息已REST接口的形式暴露

#### 与GitHub整合配置

SpringCloudConfig默认使用Git来存储配置文件，（也有其他方式，比如支持SVN和本地文件），但最推荐的还是Git，而且使用的身世http/https的访问方式。

#### 官网

http://cloud.spring.io/spring-cloud-static/spring-cloud-config/2.2.1.RELEASE/reference/html

1、配置读取规则：

```
/{application}/{profile}[/{label}]
/{application}-{profile}.yml
/{label}/{application}-{profile}.yml
/{application}-{profile}.properties
/{label}/{application}-{profile}.properties
```

　　　　比如可以访问地址：

　　　　a、查看test分支上的config-dev.yml文件信息：http://localhost:8888/config/dev/test

　　　　b、获取默认（master）分支上的config-dev.yml文件：http://localhost:8888/config-dev.yml

　　　　c、获取dev分支上的config-dev.yml文件：http://localhost:8888/dev/config-dev.yml

　　2、项目application.yml中配置的git属性（searchPaths）

　　　　查找路径，默认在根路径下，以下写法表示在更目录下以及folder目录下查找文件

　　　　可以在更目录下建一个folder文件夹，然后在folder文件夹建一个config-test.yml文件

　　　　访问地址：http://localhost:8888/master/config-test.yml，查看文件事，自动到根路径和folder目录下查找

　　3、本地仓库目录设置：

　　　　spring.cloud.config.server.git.basedir = /Users/h__d/Documents/git-repository/springcloud-config

　　4、本地模式-设置本地目录作为文件读取目录，编辑配置文件，配置（删除其中git仓库配置）如下：


```yaml
spring:
  application:
    name: cloud-config-conter
  profiles:
    # 告诉服务,我现在要启用本地配置(优先考虑采用工程目录resources下配置)
    active: native
  cloud:
    config:
      server:
        native:
          # 搜索配置文件的位置。默认与Spring Boot应用相同，
          # [classpath：/，classpath：/ config /，file：./，file：./ config /]。
          search-locations: /Users/h__d/Documents/git-repository/springcloud-config2
```



#### Config客户端配置

##### 分布式配置动态刷新

ConfigClient 



###### 3355监控

###### 暴露监控端口

###### @RefresScope配置类上



## 14 SpringCloud bus 消息总线

## 15 SpringCloud Stream 消息驱动

#### 消息驱动概述

##### Spring Cloud Stream是什么

Spring Cloud Stream，官方定义Spring Cloud Stream是一个构建消息驱动微服务的框架

应用程序通过 inputs 或者 outputs 来与 Spring Cloud Stream 中的binder对象交互，通过配置binding（绑定），而 Spring Cloud Stream 的binder对象负载与消息中间件交互，所以，我们只需要高清楚如何与 Spring Cloud Stream 交互就可以方便使用消息驱动的方式，通过使用Spring Integration 来连接消息代理中间件以实现消息事件驱动。

Spring Cloud Stream 为一些供应商的消息中间件产品提供了个性化的自动化配置实现，引用了发布-订阅、消费组、分区的三个核心概念

目前仅支持RabbitMQ、Kafka

###### **一句话总结：**

屏蔽底层消息中间件的差异，降低切换成本，统一消息的编程模型

###### **官网**

大纲：[https://spring.io/projects/spring-cloud-stream](https://spring.io/projects/spring-cloud-stream#overview)

API：https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.0.RELEASE/reference/html/

中文手册：https://www.springcloud.cc/spring-cloud-greenwich.html#spring-cloud-stream-overview-introducing

中文手册：https://m.wang1314.com/doc/webapp/topic/20971999.html

##### 设计思想

###### 标准MQ

生产者/消费者之间靠消息媒介传递信息内容（Message）

消息必须走特定的消息通道：消息通道MessageChannel

消息通道里的消息如何被消息呢谁负责收发处理

消息通道MessageChannel的子接口SubscribableChannel，由MessageHandle消息处理器所订阅

###### 为什用Cloud Steam

stream凭什么可以统一底层差异

在没有绑定器这个概念的情况下，我们的SpringBoot应用要直接与消息中间件进行信息交互的时候

由于各消息中间件构成的初衷不同，他们的实现细节上有较大差异性

通过定义帮顶起作为中间层，完美实现了应用程序与消息中间件细节之间的隔离

通过向应用程序暴露同意的Channel通道，使得应用程序不需要再考虑各种不同的消息中间件的落地实现

通过定义绑定器Binder作为中间层，实现了应用程序与消息中年级细节之间的解耦和隔离。



Binder 定义了INPUT对应于消费者，OUTPUT对应于生产者

Stream中的消息通讯方式遵循了发布-订阅模式

使用Topic主题进行广播，在RabbitMQ就是ExChange，在Kafka中就是Topic



###### Spring Cloud Steam标准流套路

![](https://img2020.cnblogs.com/blog/851491/202005/851491-20200507001726623-839914513.png)

Binder绑定器，很方便的连接中间件，屏蔽差异

Channel通道 是队列Queue的一种抽象，在消息通讯系统中就是实现存储和转发的媒介，通过Channel对队列进行配置

Source和Sink 可以简单理解为参照对象是SpringCloud Stream自身，从Stream发布消息就是输出，接受消息就是输入

###### 编码API和常用注解

@Input 注解标识输入通道，通过该输入通道接收道德消息进入应用程序

@OutPut 注解表示输出通道，发布的消息将通过该通过离开应用程序

@StreamListener监听队列，用于消费者的队列的消息接收

@EnableBinding 指信道channel和exchange绑定在一起

#### 案例说明

建立RabbitMQ环境，并且测试OK

新建三个子模块

cloud-stream-rabbitmq-provider8801

cloud-stream-rabbitmq-consumer8802

cloud-stream-rabbitmq-consumer8803



#### 消息驱动之生产者

#### 消息驱动之消费者

#### 分支消费与持久化



## 16 Spring Cloud Sleuth分布式请求链路跟踪



## 17 Spring Cloud Alibaba

## 18. Spring Clound Alibaba Necos 服务注册和配置中心

#### Necos简介

为什么叫Necos

##### Necos是什么

一个更易于构建

Nacos是注册中心+配置中心组和

Eureka+Config+BUS

##### Nacos能做什么

替代Eureka做服务注册中心

##### 下载地址

**github**



**官网**

http://

#### 安装并允许Nacos

http://localhost8848/necos



https://spring.io/projects/spring-cloud-alibaba#learn

#### Nacos作为服务注册中心



#### Nacos作为服务配置中心



#### Nacos集群和持久化配置



## 19. Spring Cloud Alibaba  Sentinel实现熔断与限流

## 20. Spring Cloud Alibaba  Seata处理分布式事务

#### 分布式事务的问题

分布式数据库之前 单机单库

多数据源多库

分布式式之后

单体应用被拆分为微服务应用，原来三个模型被拆分为三个独立的应用，

异常业务操作需要跨多个库  全局数据一致性问题

#### Seata简介

###### Seata是什么

Seta是一款开源的额分布式事务解决方案。致力于在微服务架构下提供高性能和简单易用的分布式事务服务。

官网 http://seata.io/zh-cn

###### Seata能做什么

一个典型的分布式事务过程

分布式试过处理过程：一个ID+三个组件模型

**Transaction ID** 

​		XID 全局唯一ID

**TM-事务协调器**

​	

**TC-事务管理器**

​		控制全局事务的边界

**RM资源管理**

​		控制分支事务

###### Seata下载

http://seata.io/zh-cn/blog/download.html

推荐使用1.0.0GA版本

###### Seata使用

```properties
#本地
@Transactional
#全局
@GlobalTransactional
```



#### Seata-Server安装



###### 6、配置registry.conf

​	修改seta-server-1.0.0\seta\conf目录下的registry.conf配置文件 指明注册中心为nacos 以及修改nacos连接信息

```json
registry{
    # file nacose rureka redis zk consul etcd3 sofa
    type="nacos"
    
    nacos{
    	serverAddr="localhost:8848"	
    	namespace=""
	    cluster="default"
	}
}
```

###### 7、先启动nacos

###### 8、再启动seata-server

#### 订单/库存/账户业务数据库准备

##### 需要先启动Nacos后启动Seata 确保连个服务都正常

Seata没启动会报错：no available server to connect

##### 分布式事务业务说明

创建三个微服务：订单微服务、库存微服务、账户微服务

##### 创建业务数据库

seata_order： 存储订单数据库

seata_storage：存储库存的数据库

seata_account：存储账户信息的数据库

```sql
create database seata_order;
create database seata_storage;
create database seata_account;
```



##### 创建业务表

```sql
create table t_order (
	id, bingint(11) not null auto_increment primary key,
	user_id bigint default null comment '用户id',
	product_id bigint default null comment '产品id',
	count int(11) default null comment '数量',
	money decimal(11,0) default null comment '金额'
	status int(1) default null comment '订单状态 0：创建中；1：已完结'
) engine=INNODB auto_INCREment=7 default charset=utf8;
```



##### 创建回滚日志表

订单、库存、账户三个数据库都需要建立各自的回滚日志表



#### 代码实现

#### 业务需求

下订单->减库存->扣余额->修改订单状态

#### 新建订单Order-Module

##### 0-模块seata-order-service2001

##### 1- 修改POM

```xml
 <dependencies>
        <!--nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--seata-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>seata-all</artifactId>
                    <groupId>io.seata</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>io.seata</groupId>
            <artifactId>seata-all</artifactId>
            <version>0.9.0</version>
        </dependency>
        <!--feign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
            <version>2.2.0.RELEASE</version>
        </dependency>
        <!--web-actuator-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--mysql-druid-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.37</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.0.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
```



##### 2-修改YML

```yaml
server:
  port: 2001
 
spring:
  application:
    name: seata-order-service
  cloud:
    alibaba:
      seata:
      	#自定义事务组名称需要与seata-server中的对应
        tx-service-group: fsp_tx_group
    nacos:
      discovery:
        server-addr: localhost:8848
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/seata_order
    username: root
    password: root
 
feign:
  hystrix:
    enabled: false
 
logging:
  level:
    io:
      seata: info
 
mybatis:
  mapperLocations: classpath:mapper/*.xml
```



##### 3-修改file.conf

​	**把seata的conf目录中的file.conf文件和registry.conf文件copy到项目的resources目录下**

```
transport {
  # tcp udt unix-domain-socket
  type = "TCP"
  #NIO NATIVE
  server = "NIO"
  #enable heartbeat
  heartbeat = true
  #thread factory for netty
  thread-factory {
    boss-thread-prefix = "NettyBoss"
    worker-thread-prefix = "NettyServerNIOWorker"
    server-executor-thread-prefix = "NettyServerBizHandler"
    share-boss-worker = false
    client-selector-thread-prefix = "NettyClientSelector"
    client-selector-thread-size = 1
    client-worker-thread-prefix = "NettyClientWorkerThread"
    # netty boss thread size,will not be used for UDT
    boss-thread-size = 1
    #auto default pin or 8
    worker-thread-size = 8
  }
  shutdown {
    # when destroy server, wait seconds
    wait = 3
  }
  serialization = "seata"
  compressor = "none"
}

service {
  #transaction service group mapping
  vgroup_mapping.fsp_tx_group = "default"	#修改自定义事务组名称
  #only support when registry.type=file, please don't set multiple addresses
  default.grouplist = "127.0.0.1:8091"
  #degrade, current not support
  enableDegrade = false
  #disable seata
  disableGlobalTransaction = false
}
client {
  async.commit.buffer.limit = 10000
  lock {
    retry.internal = 10
    retry.times = 30
  }
  report.retry.count = 5
  tm.commit.retry.count = 1
  tm.rollback.retry.count = 1
}
## transaction log store, only used in seata-server
store {
  ## store mode: file、db
  mode = "file"

  ## file store property
  file {
    ## store location dir
    dir = "sessionStore"
  }

  ## database store property
  db {
    ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
    datasource = "dbcp"
    ## mysql/oracle/h2/oceanbase etc.
    db-type = "mysql"
    driver-class-name = "com.mysql.jdbc.Driver"
    url = "jdbc:mysql://127.0.0.1:3306/seata"
    user = "mysql"
    password = "mysql"
  }
}
```



##### 4-修改registry.conf

##### 5-domain

##### 6-Dao接口及实现

##### 7-Service接口及实现

##### 8-Controller层

##### 9-Config配置

##### 10-主启动类



##### 新建库存Storage-Module

##### 新建转换Account-Module

#### 事务测试