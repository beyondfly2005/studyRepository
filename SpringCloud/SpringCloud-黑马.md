> #### 4天从浅入深精通SpringCloud微服务架构
>
> 视频地址：https://www.bilibili.com/video/BV1eE41187Ug
>
> 参考文档：https://www.cnblogs.com/xuweiweiwoaini/p/13814405.html

## P1、课程介绍

##### **前置知识**

- java
- maven

##### **课程内容**

###### 第一天

- 微服务基础知识
- SpringCloud介绍
- 注册中心
- Ribbon

###### 第二天

- Feign组件 及源码解析
- Hystrix熔断

###### 第三天

- 微服务API网关 
- 链路追踪

###### 第四天

- 消息中间件处理
- 分布式配置中心

## P2、系统架构演变-上

#### 1.1 系统架构的演变

##### 1.1.1 单体应用架构

所有功能打包到一个项目 war包

优点：开发简单，适用于小型项目

缺点：不容易拓展、维护，代码耦合度高

##### 1.1.2 垂直应用架构

将系统拆分为多个子系统

优点：解决了高并发问题，针对不同的模块优化，方便水平扩展、容错

缺点：系统之间相互独立，系统之间功能调用不方便；同样的模块如登录、用户管理存储重复开发工作

## P3、系统架构演变-中

##### 1.1.3 分布式架构

拆分为展示层，和服务层，展示层根据业务需要调用服务层的功能

缺点：无法实现服务的评估、治理、调度

##### 1.1.4 分布式SOA架构

SOA Service-Oriented Architecture 面向服务的架构，它可以根据需求通过网络对松鼠耦合的粗粒度应用组件(服务)进行分布式部署，组合和使用。一个服务通常以独立的形式存在于操作系统进程中。

基于上面的分布式架构，SOA在分布式系统的展示层和服务层中间，加入SOA调度(服务调度))实现服务的评估、治理、调度等

常用的SOA架构：ESB总线、dubbo

优点：

- 抽取公共的功能作为服务，提高开发效率
- 对不同的服务进行集群化部署解决系统压力
- 基于ESB/Dubbo减少系统耦合

缺点：

- 抽取服务的粒度较大
- 服务提供方与调用方接口耦合度较高

## P4、系统架构演变-下

##### 1.1.5 微服务架构

优点

- 通过服务的原子化查房，已经未付的独立打包，部署和升级，小团队的交付周期将缩短，运维成本也将大幅下降
- 微服务遵循单一职责原则，未付之间采用Restful等清理协议传输

缺点

- 微服务较多，服务治理成本高，不利于系统维护
- 分布式系统开发的技术成本高（容错、分布式事务等）。

##### 1.16 SOA与微服务之间的关系

**SOA**（Service-Oriented Architecture）"面向服务的架构" 是一种设计方法，其中包含多个服务，服务之间通过相互依赖最终提供一系列的功能，一个服务，通常以独立的形式存在于操作系统进程中。各个服务之间通过网络调用。

**微服务** 其实和SOA架构类似，微服务实在SOA的基础上做了升华，微服务架构强调的一个重点是“业务需要彻底的组件化和服务化”，原有的单个业务系统会拆分为多个可独立开发、设计、运行的小应用。这些小应用之间通过服务完成交互和集成。

| 功能           | SOA                  | 微服务                       |
| -------------- | -------------------- | ---------------------------- |
| 组件大小       | 大块业务逻辑         | 单独任务或小块业务逻辑       |
| 耦合           | 通常松耦合           | 总是松耦合                   |
| 公司架构       | 任何类型             | 小型、专注于功能交叉团队     |
| 管理           | 着重中央管理         | 着重分散管理                 |
| 目标           | 确保应用能够交互操作 | 执行新功能、快速拓展开发团队 |
| 功能间调用技术 | RPC远程调用          | 基于Http的远程调用           |

## P5、基础知识：rpc相关概念

#### 1.2 分布式核心知识

##### 1.2.1 分布式中的远程调用

服务提供者 暴露接口供消费者调用，服务消费者调用服务提供者提供的接口

- RPC远程调用

RPC（Remote Procedure Call）一种进程间通信，采用TCP协议或UDP协议通信协议

序列化 数据 和反序列化数据

- Http调用/Restful调用

## P6、基础知识：rpc与restFul比较

Http形式的调用，通常是通过Restful接口 形式进行调用

（1）Restful

（2）RPC

（3）RPC 与Restful调用区别于联系

| 比较项   | RESTful    | RPC         |
| -------- | ---------- | ----------- |
| 通信协议 | HTTP       | 一般使用TCP |
| 性能     | 略低       | 较高        |
| 灵活度   | 高         | 低          |
| 应用     | 微服务架构 | SOA架构     |

1、Http相对更规范、更标准、更通用，无论哪种语言都支持http协议，如果你是对外开放API，例如开放平台，外部的编程语言多种多样，你无法拒绝对每种语言的支持，现在开源中间件，基本最先支持的几个协议都包含RESTful。

2、RPC框架作为架构微服务的基础组件，它能大大降低架构微服务化的成本，提高调用方与微服务提供方的研发效率，屏蔽跨进程调用函数（服务）的各类复杂细节。让调用方感觉就像调用本地函数一样调用远端函数，让服务提供方感觉就像实现一个本地函数一样实现服务

## P7、CAP原理-上

##### 1.2.2 分布式中的CAP原理

CAP原则是衡量分布式系统架构是否符合要求的重要方式

- **C 一致性 **保持服务可用：通过多节点实现

- **A 可用性** 多节点数据一致

- **P 分区容忍性** 是否可以将数据保存到多个节点  

## P8、CAP原理-下

 一个系统中不可能同时满足C A P

- AC 放弃分区容忍性，例如：物理数据库

- AP 放弃一致性 可以短暂允许数据不一致，NoSQL，追求最终一致性

- CP 放弃可用性，追求数据强一致和分区容忍性 例如：Zookeeper集群


**目前互联网中用的最多的是AP** 可以允许短暂不一致，追求最终一致性



## P9、SpringCloud概述

#### 1.3 常见微服务架构

##### 1.3.1 SpringCloud

SpringCloud是一系列框架的有序集合，它利用SpringBoot的开发便利性巧妙的简化了分布式系统基础设施的开发，如服务注册发现、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用SpringBoot的开发风格做到一键启动和部署，SpringCloud并没有重复制造轮子，它只是将目前各家公司开发的比较成熟、经得起时间考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和维护的分布式系统开发工具包。

##### 1.3.2 ServiceComb 

​	Apache ServiceComb是业界第一个APache微服务顶级项目，是一个开源微服务解决方案，致力于帮助企业，用户和开发者将企业应用轻松微服务化上云，并实现微服务应用的高效运维管理。其提供一站式开源微服务解决方案，融合SDK框架级，零侵入ServiceMesh场景并支持多语言。

##### 1.3.3 ZeroC ICE

ZeroC IceGrid是ZeroC公司的杰作，继承了CORBA的血统，是新一代面向对象的分布式系统中间件，作为一种微服务架构，它基于RPC框架发展而来，具有良好的性能与分布式能力。



#### 2.2 SpringCloud的介绍

SpringCloud是一些列框架的有序集合。它利用Spring Boot开发的便利性巧妙的简化了分布式系统基础设施的开发，如**服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控**等，都可以用SpringBoot的开发风格做到一键启动和部署，SpringCloud并没有重复制造轮子，它只是将目前各家公司开发的比较成熟、经得起时间考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和维护的分布式系统开发工具包。

#### 2.3 SpringCloud的架构

##### 2.3.1 SpringCloud中的核心组件

SpringCloud Neflix组件

- Eureka 服务注册与发现

- Ribbon 客户端负载均衡
- Feign 声明式事务调用
- Hystrix 客户端容错保护
- Zuul  API服务网关

SpringCloud Alibaba组件

- Nacos  服务注册中心
- Sentinel  客户端容错保护

Spring Cloud原生及其他组件

- Consul 服务注册中心
- Config 配置中心
- Gateway API服务网关
- Sleuth/Zipkin 
- SpringCloud Bus 消息总线

##### 2.3.2 SpringCloud体系结构

![img](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2051585292,2287143787&fm=26&gp=0.jpg)

- **注册中心** 负责服务的注册与发现，很好的将各个服务连接起来
- **断路器** 负责监控服务之间的调用情况，连续多次失败进行熔断保护
- **API网关** 负责转发所有对外的请求和服务
- **配置中心**  提供统一的配置信息管理服务，可以实时的二同志各个服务获取最新的配置信息

## P10、模拟微服务环境- 上

##### 创建父工程

​	spring-cloud-demo

##### 引入坐标

- spring-boot-start-parent 2.1.6.RELEASE
- 编码格式UTF-8
- starter-web
- starter-logging
- starter-test
- lombok
- spring-cloud-dependencies Greenwich.RELEASE版本 （放到dependencyManagement节点）

##### 创建子工程 

​	子模块名称product-service

​	pom依赖

- mysql-connector-java
- spring-boot-starter-data-jpa

##### 创建数据库表

- 商品表 tb_product

- 字段：id  product_name名称  status状态  price价格  product_desc描述 caption标题  inventory库存

##### 编写业务代码
- 商品实体类

  ```java
  package com.beyondsoft.prooduct.entity;
  
  @Data
  @Entity
  @Table(name="tb_product")
  public class Product {
      @Id
  	private Long id;
      private String product_name; //名称  
      private Integer status;// 状态  
      private BigDecimal price;//价格  
      private String product_desc;//描述 
      private String  caption;//标题  
      private Integer inventory;//库存
  }
  ```

- JPA 接口

  继承JpaRepository<Product,Long> , JpaSpecificationExecutor<Product>

  ```java
  package com.beyondsoft.product.dao;
  public interface ProductDao extends JpaRepository<Product,Long> , JpaSpecificationExecutor<Product>{    
  }
  ```

- Service接口

  ProductService

  ```java
  public class PrudocuService{
  /** 查询 */
  Product findById(Long id) 
  /** 保存 */
  void save(Product product) 
  /** 更新 */
  void update(Product product)
  /** 删除接口*/
  void delete(Long id)
  }
  ```

- Service实现  

  ProductServiceImpl，需要注入ProductDao接口

  ```java
  @Service
  public class PrudocuServiceImpl implements Product{
  	@Autowired
  	private ProductDao productDao;
  	/** 查询 */
  	Product findById(Long id){
          productDao.findById(id).get();
      }
  	/** 保存 */
  	void save(Product product){
          productDao.save(product);
      }
  	/** 更新 */
  	void update(Product product){
          productDao.save(product);
      }
  	/** 删除接口*/
  	void delete(Long id){
       	productDao.delete(id);   
      }
  }
  ```

## P11、模拟微服务环境- 下

- Controller控制器

  ```java
  @RestController
  @RequestMapping("/product")
  public class ProductController{
      @Autowired
      private ProductService productService;
      
      @RequestMapping(value="/{id}",method = RequestMethod.GET)
      public Product findById(@PathVariable Long id){
          Product product productService.findById(id);
          return product;
      }
      
      @RequestMapping(value="",method = RequestMethod.POST)
      public String save(@RequestBody Product product){
          Product product productService.save(product);
          return "保存成功";
      }
  }
  ```

- 启动类ProductApplication 

  ```java
  @EntitySacan("com.beyondsoft.product.entity")
  @SpringBootApplication
  public class ProductApplication{
      public static void main(String[] args){
          SpringApplication.run(ProductApplication.class, args);
      }
  }
  ```

- 配置文件application.yml 

  ```yaml
  server:
    port: 9001  #端口
  spring:
    application:
      name: serveice-product  #服务名称
    datasource:
      driver-class-name: com.mysql.jdbc.Driver
      url: jdbc:mysql://localhost:3306/shop?useUnicode=true&characterEncoding=utf8
      username: root
      password: 123456
    jpa:
      database: MySQL
      show-sql: true
      open-in-view: true
  ```

  配置文件中配置了：1-server端口  2-mysql连接  3-JPA配置  4-application名称

- 测试浏览器访问

  ```bash
  http://127.0.0.1:9001/produc/1
  ```

## P12、RestTemplate调用远程服务

##### 实现目标

```
实现：模拟 订单模块 远程调用 商品模块
```

##### 创建子模块

子模块名称：order-service

##### 引入依赖

- mysql-connector-java  版本5.1.32
- spring-boot-starter-data-jpa 

##### 配置文件

application.yaml

```yaml
server:
  port: 9002  #端口
spring:
  application:
    name: serveice-order  #服务名称
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/shop?useUnicode=true&characterEncoding=utf8
    username: root
    password: 123456
  jpa:
    database: MySQL
    show-sql: true
    open-in-view: true
```

##### SpringBoot启动类

```java
@EntitySacan("com.beyondsoft.order.entity")
@SpringBootApplication
public class OrderApplication{
    public static void main(String[] args){
        SpringApplication.run(OrderApplication.class, args);
    }
}
```

##### 控制器Controller

OrderController 订单控制器

```java
@RestController
@RequestMapping("/order")
public class OrderController{
    
    //@Autowired
    //private ProductService productService;
    
    @Autowired
    private RestTemplate restTemplate;
    
    /**
       参数为商品id
       通过订单调动商品服务，根据商品id查询商品信息
       1、需要配置商品对象
       2、需要调用商品服务
       可以使用 java中的urlconnection,httpclient,okhttpclient
    */
    @RequestMapping(value="/buy/{id}",method = RequestMethod.GET)
    public Product findById(@PathVariable Long id){
        //单体架构的：本地调用
        //Product product productService.findById(id);
        //如何远程调用商品服务
        //  1 注入RestTemplate
        //  2 使用restTemplate发起远程服务调用
        Product product = restTemplate.getForObject("http://127.0.0.1:9001/product/"+id,Product.class);
		return product;
    }
}
```

##### 商品对象实体类

由于不需要查询数据库 所以将jpa的配置删除

```java
package com.beyondsoft.order.entity;
@Data
public class Product {
	private Long id;
    private String product_name; //名称  
    private Integer status;// 状态  
    private BigDecimal price;//价格  
    private String product_desc;//描述 
    private String  caption;//标题  
    private Integer inventory;//库存
}
```

##### 远程服务调用

远程服务调用http接口返回json的方法

远程调用http接口，可以使用java的urlConnection、Httpclient、OKhttp等

这里使用Spring的RestTemplate远程调用

###### RestTemplate介绍

SpringRestTemplate是Spring 提供的用于访问 Rest 服务的客端, RestTemplate提供了多种便捷访问远程Http服务的方法，能够大大提高客户端的编写效率,所以很多客户端比如Android或者第三方服务商都是使用RestTemplate 请求 restful服务

###### RestTemplate方法介绍

| API             | 请求方式  | 说明                                                         |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| getForEntity()   | GET | 发送一个HTTP GET请求，返回的ResponseEntity包含了响应体所映射成的对象 |
| getForObject()   | GET | 发送一个HTTP GET请求，返回的请求体将映射为一个对象           |
| postForEntity()  | POST | POST 数据到一个URL，返回包含一个对象的ResponseEntity，这个对象是从响应体中映射得到的 |
| postForObject()   | POST | POST 数据到一个URL，返回根据响应体匹配形成的对象             |
| headForHeaders() | HEAD | 发送HTTP HEAD请求，返回包含特定资源URL的HTTP头               |
| optionsForAllow() | OPTIONS | 发送HTTP OPTIONS请求，返回对特定URL的Allow头信息             |
| postForLocation() | POST | POST 数据到一个URL，返回新创建资源的URL                      |
| put()           | PUT | PUT 资源到特定的URL                                          |
| delete()        | DELETE | 在特定的URL上对资源执行HTTP DELETE操作                       |
| exchange()      | any | 在URL上执行特定的HTTP方法，返回包含对象的ResponseEntity，这个对象是从响应体中映射得到的 |
| execute()       | any | 在URL上执行特定的HTTP方法，返回一个从响应体映射得到的对象    |

###### 通过RestTemplate调用微服务

使用方法：

1、注入RestTemplate 创建对象 由spring管理

```java
// 在启动类中或配置类中加入如下代码
@Bean
public RestTemplate restTemplate(){
	return new RestTemplate;
}
```

2、使用RestTemplate.的方法 getXX PostXX等 远程发起调用

```java
Product product = restTemplate.getForObject(url,Product.class);
return product;
```

##### 测试远程调用

```
http://127.0.0.1:9002/order/buy/1
```



## P13、模拟微服务中存在的问题

使用RestTemplate发起远程调用存在的问题：

- 将微服务的请求调用路径 硬编码到java代码中

- 如果要有多个微服务调用，在调用者方需要管理多个服务地址，加大了开发难度和复杂度

- 如果同一个微服务部署多份，如何实现负载均衡 让每一个服务都轮询的被调用到

- 如何通过API网关实现前端对多个微服务器的调用管理

- 如何实现配置的统一管理

- 如何实现调用的链路追踪，通过分散在各个服务中的日志来定位问题
- 其他：比如系统容错性的考虑和实现

通过SpringCloud框架，可以一站式解决以上存在的各种问题，以上每个问题，SpringCloud都有专门的组件来统一解决这些问题

## P14、注册中心概述

微服务注册中心 用来解决请求路径硬编码问题 

#### 3、服务注册Eureka基础

##### 3.1 微服务注册中心

注册中心可以说是微服务架构中的“通讯录”，它记录了服务和服务地址的映射关系，在分布式系统中，服务会注册到这里，当微服务需要调用其他服务时，就在这里找到服务的地址，进行调用。

![img](https://upload-images.jianshu.io/upload_images/11368879-dc1ce083ea81f0a5?imageMogr2/auto-orient/strip|imageView2/2/w/550/format/webp)

###### 3.1.1 注册中心的主要作用

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.uml.org.cn%2Fwfw%2Fimages%2F2017080324.png&refer=http%3A%2F%2Fwww.uml.org.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1613112865&t=9573d73b41a8fa97e11ec6ed447ee9eb)

- 服务发现
  - 服务注册/反注册
  - 服务订阅/取消订阅
  - 服务路由(可选)
- 服务配置
  - 配置订阅
  - 配置下发
- 服务健康检测
  - 检测服务提供者的监控情况

###### 3.1.2 常见的注册中心

- **Zookeeper**

- **Eureka**

- **Consul**

- **Nacos**

  Nacos 是阿里开源的, 功能其实也很多, 服务注册, 配置管理, 动态 DNS 服务, 元数据管理

**常见的注册中心对比**

| 注册中心  | 开发语言 | CAP  | 一致性算法 | 服务健康检查 | 对外暴露接口 |
| --------- | -------- | ---- | ---------- | ------------ | ------------ |
| Eureka    | java     | AP   | 无         | 可配置支持   | HTTP         |
| Consul    | Go       | CP   | Raft       | 支持         | HTTP/DNS     |
| Zookeeper | Java     | CP   | Paxos      | 支持         | 客户端       |
| Nacos     | Java     | AP   | Raft       | 支持         | HTTP         |



## P15、注册中心Eureka概述

##### 3.2 Eureka概述

Eureka是Netflix开发的服务发现框架，SPringCloud将它集成在自己的子项目spring-cloud-netflix中，实现SpringCloud的服务发现功能

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.uml.org.cn%2Fwfw%2Fimages%2F2017080324.png&refer=http%3A%2F%2Fwww.uml.org.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1613112865&t=9573d73b41a8fa97e11ec6ed447ee9eb)

Eureka的基本架构，有三个角色组成

1、Eureka Server

- 提供服务注册和发现功能

2、Service Provider

- 服务提供方
- 将自身服务注册到Eureka，从而使服务消费方能够找到

3、Service Consumer

- 服务消费方
- 从Eureka获取注册服务列表，从而能够消费服务

## P16、搭建EurekaServer注册中心

##### 使用Eureka的步骤

- 搭建Eureka Server

- 将服务提供者注册到EurekaServer上
- 服务消费者通过注册中心获取服务列表并调用服务

#### 1、搭建Eureka Server

##### 1.1 创建工程

子工程/模块名称 eureka-server

##### 1.2 导入pom依赖 坐标

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

##### 1.3 配置Application.yml配置文件

```yml
server:
  port: 9000

spring:
  application:
    name: Eureka-Server

# 配置Eureka
eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false  #是否将自己注册到注册中心
    fetch-registry: false  #是否要从eureka中获取注册的信息
    #配置暴露给Eureka Client的请求地址
    service-url:
      defaultZone: http://${eureka.instance.hostname}:{server.port}/eureka/

```

##### 1.4 配置启动类

```java
@SpringBootApplication
//声明这个一个Eureka注册中心
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

##### 1.5 测试Eureka是否配置成功

```
浏览器访问
http://localhost:9000/
```



## P17、Eureka将服务注册到注册中心

#### 2、将服务提供者注册到EurekaServer

##### 2.1 引入EurekaClient的pom坐标

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

##### 2.2 修改配置文件application.yml 添加EurekaServer的信息

```yaml
#配置eureka
eureka:
  client:
    service-url:
      defaultZone: http://localhost:9000/eureka/
  instance:
    prefer-ip-address: true #使用ip地址进行注册 方便在eureka管控控制台看到这个服务的ip
```

##### 2.3 修改启动类添加服务发现的支持（可选）

```java
//激活eurekaClient 声明这是一个eureka客户端
@EnableEurekaClient
//@EnableDiscoveryClient //这两个注解写任意一个都可以 一个是netflix提供 一个是spring提供，新版中 不写也可以 所以这一步是（可选）
public class ProductApplication{
    //......
}
```

##### 2.4 验证注册是否成功

打开eureka管控控制台 http://localhost:9000 可以看到这个服务的应用实例  就表明注册成功。

## P18、获取微服务的调用路径

#### 3、服务消费者通过注册中心获取服务列表并调用服务

Eureka元数据：Eurek的服务信息或配置信息；如：服务的主机名、IP地址等信息，这些信息可以通过eurekaserver进行获取，用于服务之间的调用

如果能获取到eureka的这些元数据，就能够在消费者端调用服务提供者提供的服务

##### 3.1 引入EurekaClient的pom坐标

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

##### 3.2 修改配置文件application.yml 添加EurekaServer的信息

```yaml
#配置eureka
eureka:
  client:
    service-url:
      defaultZone: http://localhost:9000/eureka/
  instance:
    prefer-ip-address: true #使用ip地址进行注册 方便在eureka管控控制台看到这个服务的ip
```

##### 3.3 修改启动类添加服务发现的支持（可选）

```java
@EnableEurekaClient
//@EnableDiscoveryClient //这两个注解写任意一个都可以 不写也可以 这一步可选
public class OrderApplication{
    //......
}
```

##### 3.4 使用DiscoveryClient获取Eureka元数据

在OrderController

```java
@Autowired
private DiscoveryClient discoveryClient;  //springcloud提供的获取元数据的工具类

//获取eureak服务的元数据信息
@RequestMapping("/getEurekaInstance",method=RequestMethod.GET)
public void getEurekaInstance(){
    List<ServiceInstance> instances = discoveryClient.getInstance("service-product"); //参数为注册到eureka的 服务名 小写
    for(ServiceInstance instance : instances){
        System.out.println(instance);
        //instance.appName()
        //instance.ipAddr()
        //instance.getHost() //主机名
        //instance.port()  //端口号
        //instance.homePageUrl()
    }
}

public Product findById(@Pathvariable Long id){
    ......
    // 远程调用
    //Product product = restTemplate.getForObject("http://127.0.0.1:9001/product/"+id,Product.class);
    // 改为
    Product product = restTemplate.getForObject("http://"+instance.getHost()+":"+instance.getPort()+"/product/"+id,Product.class);
    return product;
    ......
}
```



## P19、Eureka高可用的引入

#### 4.1 Eureka Server高可用集群

在上一个章节，实现了单节点的Eureka Server的服务注册与服务发现功能。Eureka Client会定时连接Eureka Server，获取注册表中的信息并缓存到本地。微服务在消费远程API时总是使用本地缓存中的数据。因此一般来说，即使Eureka Server发生宕机，也不会影响到服务之间的调用。但如果EurekaServer宕机时，某些微服务也出现了不可用的情况，Eureka Server中的缓存若不被刷新，就可能会影响到微服务的调用，甚至影响到整个应用系统的高可用。因此，在生成环境中，通常会部署一个高可用的Eureka Server集群。

Eureka Server可以通过运行多个实例并相互注册的方式实现高可用部署，Eureka Server实例会彼此增量地同步信息，从而确保所有节点数据一致。事实上，节点之间相互注册是Eureka Server的默认行为。



![技术分享图片](http://image1.bubuko.com/info/202003/20200317211840972101.png)

**目前项目存在的问题**

​		Eureka承担较高的负载，一旦EurekaServer出现宕机，整个系统将不可用。

##### Eureka高可用

​	1、准备两个EurekaServer，需要互相注册

​	Server1:9000 	Server2:8000

​	2、需要将微服务注册到两个EurekaServer上

## P20、EurekaServer的相互注册

```yml
server:
  port:8000
spring:
  application:
    name: eureka-server
eureka:
  client:
    register-with-eureka: true #是否将自己注册到注册中心 需要互相注册
    fetch-registry: true #是否从eureka中获取信息
    service-url:
      defaultZone: http://127.0.0.1:9000/eureka
      
```

```yml
server:
  port:9000
spring:
  application:
    name: eureka-server
eureka:
  client:
    register-with-eureka: true #是否将自己注册到注册中心  需要互相注册
    fetch-registry: true #是否从eureka中获取信息 需要拉取服务
    service-url:
      defaultZone: http://127.0.0.1:8000/eureka
      
```

### P21、服务注册到多个Eureka Server

一旦两个EurekaServer互相注册，他们之间就会进行信息同步，如果只注册到一个Eureka Server，在另一台Eurak Server中也会正常看到这个服务的的注册信息；但是对于高可用Eureka Server来说，仍需要将微服务同时注册到 两个Eureka-Server，这是因为万一注册到某一台EureakServer时，这个Server已经宕机了，那么将会注册失败，无法提供服务。

所以，需要将服务提供者同时注册到 两个Eureka-Server；服务消费者service-order同样需要同时从两台Eureka Server获取服务提供者信息列表

```yml
eureka:
  client:
    service-url:
     default-zone: http://192.168.1.254:8000/eureka,http://192.168.1.254:9000/eureka
```

## P22、Eureka显示IP与服务续约时间

#### 4.2 Eureka中的常见问题/细节问题

**在Eurela管理控制台，显示服务的IP地址**

在服务提供者，通过eureka.instance,instance-id配置控制台显示服务的ip

```yaml
eureka:
  instance:
   instance-id: ${spring.cloud.client.ip-address}:${server.port} #向注册中心注册服务id
```

**Eureka的服务剔除问题**

服务注册到Eureka后，默认情况下，每隔30秒向Eureka Sever发送一次心跳

如果90秒内没有发送心跳，则代表服务已经宕机

正式系统 这个时间会显得很长

在服务提供者 ，设置心跳时间间隔，设置续约到期时间

```yaml
eureka:
  instance:
    instance-id: ${spring.cloud.client.ip-address}:${server.port} #向注册中心注册服务id
    lease-renewal-interval-in-seconds: 5 #设置心跳时间间隔 5秒
    lease-expiration-duration-in-seconds: 10 #设置续约到期时间 10秒
```

## P23、Eureka自我保护机制

**Eureka的自我保护机制**

判断服务是否宕机，除了规定时间内发送心跳之外，还需要统计在15分钟之内 正常的心跳比例，是否低于85%；如果低于85% 就会开启服务保护模式，开启服务保护模式之后，就不会再对服务进行剔除。在开发测试阶段，一般不需要开启服务保护机制

关闭EurkaServer自我保护机制，和设置剔除时间简介

```yaml
eureka: 
  server: 
    enable-self-preservation: false #关闭自我保护机制
    eviction-interval-timer-in-ms: 4000 #剔除服务的时间价格 单位毫秒； 4秒进行服务剔除 在开发测试阶段更加准确的得到测试服务数据
```

## P24、SpringBoot的自动装载

#### 4.3 Eureka源码解析

##### 4.3.1 SpringBoot中的自动装载

## P25、SpringBoot的自动装载-下

## P26、Server的启动流程

## P27、Client的启动流程

## P28、Ribbon概述及基于Ribbon的远程调用

### 6.2 Ribbon概述

Ribbon是Netflix发布的一个负载均衡器，有助于控制http和tcp客户端行为。在springCloud中Eureka一般配合Ribbon进行使用，Ribbon提供了客户端负载均衡的功能，Ribbon利用从Eureka中读取到的服务信息，在调用服务节点提供的服务时，会合理的进行负载。

在SpringCloud中可以将注册中心和Ribbon配合使用，Ribbon自动从注册中心获取服务提供者的列表信息，并基于内置的负载均衡算法，请求服务

#### 6.2.2 Ribbon的主要作用

##### (1) 服务调用

基于Ribbon实现服务调用，是通过拉取到的所有服务列表组成（服务名-请求路径）映射关系，借助RestTemplate最终进行调用

##### (2) 负载均衡

当有多个服务提供者时，Ribbon可以根据负载均衡的算法自动的选择需要调用的服务地址

**使用Ribbon进行服务调用**

Eurka内部已经集成了ribbon，使用Ribbon继续服务调用只需以下两步：

- 在创建RestTemplate的时候，声明@LoadBalanced
- 使用RestTemplate调用远程微服务，不需要拼接微服务的url，使用待请求的服务名替换IP地址及端口

```java
//使用微服务名称替换ip地址
@RequestMapping(value="/buy/{id}",method=RequestMethod.GET)    
public Produc findByid(){
    Prodct  product = null;
    product = restTemplate.getForObject("http://127.0.0.1:9001/rpoduct/1",Product.class);
    //改为
    product = restTemplate.getForObject("http://service-product/rpoduct/1",Product.class);
}
```

### 6.3 基于Ribbon实现订单调用商品服务

不论是基于Eureka的注册中心还是基于Consul的注册中心，SpringCloudRibbon统一进行了封装，所以对于服务调用，两者的方式是一样的。

1、引入坐标

Eureka内部集成了Ribbbon 不需要在引入其他pom依赖

2、修改调用RestTemplate 调用方式

## P29、Ribbon：客户端负载均衡的概述

**负载均衡分类**

服务端负载均衡：Nginx、F5硬件负载均衡

客户端负载均衡：客户端记录各个服务节点的地址，客户端根据一定的算法绝对访问那个服务节点。

**Ribbon负载均衡**

Ribbon是一个典型的客户端负载均衡器，Ribbon会获取服务的所有地址，根据内部的负载均衡算法，获取本次请求的有效地址

## P30、Ribbon：基于ribbon的负载均衡测试

通过案例学习Ribbon负载均衡的实现

- 准备两个商品微服务(9001,9011)

- 在订单系统中远程以负载均衡的形式调用商品服务



## P31、Ribbon：负载均衡策略

**常用的负载均衡策略**

默认的负载均衡策略：轮询算法

- com.netflix.loadbalancer.RoudRobinRule 轮询

- com.netflix.loadbalancer.RandomRule 随机策略

- com.netflix.loadbalancer.RetryRule 重试策略

- com.netflix.loadbalancer.WeightedResponseTimeRule 权重策略：计算每个服务的权重，越高的被调用到的可能越大

- com.netflix.loadbalancer.BestAvailableRule 最佳策略：遍历所有的服务实例，过滤掉故障实例，并返回请求数最小的实例
- com.netflix.loadbalancer.AvailabilityFilteringRule 可用过滤策略：过滤掉故障和请求书超过阈值的服务实例，从剩下的实例中轮询调用

**修改默认的负载均衡策略**

在服务消费者端，修改application.yaml文件

```yaml
# 修改ribbon的负载均衡策略 服务名 - ribbon - NFLoadBalancerRuleClassName: 策略
service-product:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule #随机策略
```



## P32、Ribbon：请求重试

Ribbon请求重试 是Ribbon的一种负载均衡策略，可以设置请求连接的时间，或返回数据的时间，当超过设定的时间后，会发送请求到另外的服务节点上

**设置重试机制**

- 在pom依赖中 引入Spring的重试组件

  服务消费者Order-Service模块中

  ```xml
  <dependency>
  	<groupId>org.springframework.retry</groupId>
      <artifactId>spring-retry</artifactId>
  </dependency>
  ```

- 对Ribbon进行配置

  ```yaml
  service-product:
    ribbon:
      connectTimeout: 250 #Ribbon的连接超时时间
      ReadTimeout: 1000   #Ribbon的数据读取超时时间
      OkToRetryOnAllOperations: true #是否对所有操作都进行重试
      MaxAutoRetriesNextServer: 1 #切换实例的重试次数
      MaxAutoRetries: 1   #对当前实例的重试次数 1次 代表对当前节点只连接一次， n代表对当前服务节点 重试n次后再尝试连接其他服务节点
  ```

  

## P33、Ribbon：源码分析

##### 自动装载

RibbonAutoConfiguration(自动装配)

​        |--------  LoadBalancerConfigureation

LoadBalancerInterceptor中的intercept方法

|-------- IloadBalancer

## P34、Consul概述 替换Eureka

Eureka 1.x停更，Eureka 2.x 闭源，考虑替换Eureka

**Eureka的替换方案**

- Zookeeper

- Consul

- Necos

**什么是Consul**

Consul是HashiCorp公司推出的开源工具，用于实现分布式系统的服务发现与配置，与其他分布式服务注册于发现

Consul的优势：

- 使用Raft算法保证一致性
- 支持多数据中心
- 支持健康检查
- 支持http和dns协议接口

- 官方提供web页面
- 综合比较，COnsul作为服务注册和配置管理的新星，比较值得关注

特性：

- 服务发现

- 健康检查

- Key/Value存储

- 多数据中心

  

#### 5.2.2 Consul与Eureka的区别

##### (1) 一致性

Consul是强一致性CP  牺牲的是可用性；Eureka保证的是AP 一致性差一点

Eurka 短时间内 可能额存在 数据不一致，但是能保证最终一致性

##### (2) 开发语言和使用

Consul是go语言编写的, Necos是java编写的



## P35、Consul安装与快速启动

#### 5.2.3 Consul的下载与安装

官网地址：https://www.consul.io/

window版启动

开发者模式启动，进入consul的解压目录 ，cmd命令行中输入

```bash
consul agent -dev -client=0.0.0.0
```

```bash
#说明：# 0.0.0.0 表示所有服务器都可以访问
```

web管理控制台

```
http://127.0.0.1:8500/
```



## P36、Consul基本功能介绍

### 5.3 Consul的基本使用

#### 5.3.1 服务注册与发现

##### （1）注册服务

通过postman发送put请求到http://127.0.0.1:8500/v1/catalog/register地址可以完成服务注册



## P37、将微服务注册到Consul

#### 入门案例

- 提供一个商品微服务系统
- 提供一个订单微服务系统

将微服务注册到注册中心consul

服务的消费者从consul中拉取所有的服务列表

#### 项目工程

父工程 spruing-cloud-cosul-demo

子工程 order-service

子工程 product-sevice

#### 将微服务注册到consul

##### 添加依赖

服务提供端：product-service项目

```xml
<!-- spring提供的基于consul的服务发现 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
<!-- spring actuator 健康检查 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

##### 配置application.yml

```yaml
spring:
  cloud:
    consul:
      host: 127.0.0.1 #consul服务器的ip地址
      port: 8500 #consul服务器的端口
      discovery:
        register: true #是否需要注册 
        instance-id: ${spring.application.name}-1 #注册实例ID（唯一标志）
        server-name: ${spring.application.name} #服务的名称
        port: ${server.port} #服务的请求端口
        perfer-ip-address: true #指定开启ip地址注册
        ip-address: ${spring.cloud.client.ip-address} #当前服务的请求ip
```



## P38、消费者从consul获取服务并调用

#### 消费者从consul获取服务并调用

服务消费端：order-service项目

##### 添加依赖

```xml
<!-- spring提供的基于consul的服务发现 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
<!-- spring actuator 健康检查 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

##### 配置application.yml

```yaml
spring:
  cloud:
    consul:
      host: 127.0.0.1 #consul服务器的ip地址
      port: 8500 #consul服务器的端口
      discovery:
        register: false #是否需要注册 
        instance-id: ${spring.application.name}-1 #注册实例ID（唯一标志）
        server-name: ${spring.application.name} #服务的名称
        port: ${server.port} #服务的请求端口
        perfer-ip-address: true #指定开启ip地址注册
        ip-address: ${spring.cloud.client.ip-address} #当前服务的请求ip
```

##### 负载均衡配置

SpringCloud对consul进行了进一步封装，集成了对ribbon的支持，已经可以通过ribbon实现服务的调用，需要在引入RestTemplate的时候加入注释@LoadBalanced

```java
public class OrderApplication {
    @LoadBalanced
    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate;
    }
}
```

##### 使用Ribbon调用服务

```java
public class OrderController{
    
    @RequestMapping(value="/buy/{id}",method=RequestMethod.GET)
    public Product findById(Long id) {
        //Product product = restTemplate.getForObject("http://127.0.0.1:9001/product/"+id,Product.class);
        //将上面的通过ip地址调用，改为是否Ribbon负载均衡的方式通过服务名来调用
        Product product = restTemplate.getForObject("http://service-product/product/"+id,Product.class);
        return product
    }    
}
```



## P39、Consul集群：consul集群的基础知识

#### 5.6 consul高可用集群

##### 5.6.1 consul的核心知识

agnet: 启动一个consul的守护进程

​       	dev： 开发者模式

​			client：是consl代理，和consul server交换

​						client有多个

​						一个微服务对应一个client

​						微服务和client部署到一台机器上

​			server：真正工作的consul

​						推荐3-5个server

Gossip协议：

​		流言协议/流感传播：client随机向其他节点发送消息，被感染的节点也会随机发送数据到其他节点，随着时间的地址，集群的所有节点都会收到这条消息

​		所有的consul节点 都会参与到goosip协议中（多节点中数据赋值）

Raft协议：

​		保证server集群数据一致，是强一致性协议

​		三个角色：Leader角色： 是server集群中唯一处理客户端请求的

​							Follower角色：类似选民，被动接受Leader发送的同步数据

​							Candidate候选人：可以被选举为Leader

​					选主：绝对候选人是否可以作为Leader

​					数据同步：Leader将客户端发送的数据同步给选民Follower

​	

##### 5.6.2 consul集群搭建

##### 5.6.3 consul的常见问题



## P40、Consul集群：搭建consul集群



## P41、Consul集群：集群测试以及问题说明



## P42、总结



## P43、day2课程介绍

**Feign组件**

**Hystrix组件**

**Sentine组件**

## P44、Feign概述

Ribbin远程调用存在的问题

如果调用多个微服务 需要配置多个

需要拼接url 及参数，如果参数过多 ，拼接会很麻烦

### 1 服务调用Feign入门

#### 1.1 Feign简介

Feign是Netflix开发的声明式模板化http客户端，其灵感来自于Retrofit、JAXRS-2.0以及WebSocket

- Feign可以帮助我们更加便捷，优雅的调用HTTP API
- 在SpringCloud中，使用Feign非常简单——创建一个接口，并在接口上添加一些注解，代码就完成了。
- Feign支持多种注解，例如Efign自带的注解或者JAX-RS注解等。
- SpringCloud对Feign进行了增强，使得Feign支持了SpringMVC注解，并整合了Ribbon和Eureka，从而让Feign的使用更加方便。

#### 1.2 Feigin入门实例环境搭建

##### 1、导入依赖

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>        
        <artifactId>spring-cloud-starter-openfeign</artifactId>
        <!--<artifactId>spring-cloud-starter-netflix-feign</artifactId>-->
    </dependency>
```

##### 2、配置调用接口

​	在feign包下创建

```java
//声明需要调用的微服务名称
@FeignClient(name="service-product")
public interface ProductFeighClient {

    /**
     * 配置需要调用的微服务接口
     */
    @RequestMapping(value = "/product/{id}",method = RequestMethod.GET)
    public Product findById(@PathVariable("id") Long id);
}
```

##### 3、在启动类上 激活Feign

```java
@EntityScan("com.beyondsoft.intelsecurity.model")
@EnableFeignClients
public class OrderServerApplication{
    public static void main(String[] args){
        SpringApplication.run(OrderServerApplication.class, args);
    }
}
```

##### 4、通过自定义的接口调用远程微服务

```java
@Autowired
private ProductFeignClient productFeignClient;

@RequestMapping(value = "/product/{id}",method = RequestMethod.GET)
public Product findById(@PathVariable("id")Long id){
    Product product = productFeignClient.findById(id);
    return product
}
```

## P45、入门案例搭建



## P46、Feign:入门案例的注意事项以及与ribbon的区别和联系

#### 1.3 Feign与ribbon的区别和联系

**Ribbon**是一个基于HTTP和TCP的客户端负载均衡工具，它可以在客户端配置RibbonServerList(服务列表)，使用HttpClient或RestTemplate模拟http请求，步骤相对繁琐。

**Feign**是在Ribbon的基础上进行了一次改进，是一个使用起来更加方便的HTTP客户端，采用接口的的方式，只需要一个接口，然后在上面添加注释即可，将需要调用的其他服务的方法定义成抽象方法即可，不需要自己构建http请求，就像调用自身工程的方法一样进行调用，而感觉不到是调用远程的方法，使得编写客户端变得更加容易

#### 1.4 Feign负载均衡

Feign中本身已经继承了Ribbon依赖和自动配置，因此我们不需要额外引入依赖，也不需要在注册RestTemplate对象。另外可以配置Ribbon，可以通过ribbon.xx 来进行劝解配置，也可以通过服务名。ribbon.xx对指定服务进行配置

启动两个shop_service_product，重启测试可以发现适应Ribbon的轮询策略进行负载均衡。

### 2 服务调用Feign高级

#### 2.1 Feign的配置

从SpringCloud Edgware开始，Feign支持使用数学自定义Feign，对于一个指定名称的FeignClient（例如该FeignClient的名称为feignName）,Feign支持如下配置项：

```yaml
feign:
  client:
    config:
      feignName: ## 定义FeginClient的名称
        connectTimeOut: 5000 # 相当于Request.Options
        readTimeout: 5000    # 相当于Request.Options 
        loggerLevel: full    # 配置日志级别 相当于代码配置方式中的Logger
        errorDecoder: com.example.SimpleErrorDecoder # 配置Feign的错误解码器 相当于代码配置方式中的ErrorDecoder
        retryer: com.example.SimpleRetyer # 配置重试相当于代码配置方式中的Retryer
        requestInterceptors:   #配置拦截器 相当于代码配置方式中的RequestInterceptor
          - com.example.FootRequestInterceptor
          - com.example.BarRequestInterceptor
        decode404: false  
```

- FeignName：FeiginClient的名称
- connectTimeout： 建立连接的超时时长
- readTimeout：读取超长的时长
- loggerLevel：配置Feign日志级别
- errorDecoder：配置Feign的错误解码器
- retryer：配置重试
- requestInterceptors：配置拦截器
- decode404：配置熔断不出来404异常

####  2.2 请求压缩

SpringCloud支持对请求和响应进行GZIP压缩，以减少通信过程中的性能损耗，通过下面的参数即可开启请求与响应的压缩功能

```yaml
feign:
  compression:
    request:
      enabled: true # 开启请求压缩
    response:
      enabled: true # 开启响应压缩
```

同时我们也可以对请求的数据类型，已经出发压缩的大小进行设置：

```yaml
feign:
  compression:
    request:
      enabled: true # 开启请求压缩
      mime-types: text/html,application/xml,application/json # 设置压缩数据类型
```



## P47、Feign:负载均衡测试



## P48、Feign:打印Feigin日志

#### 2.2 请求压缩

SpringCloud Feign支持对请求和响应进行GZIP压缩，以减少通信过程中的性能损耗，通过下面的参数，即可开启请求与响应的压缩功能：

```yaml
feign:
  compression:
    request:
      enabled: true ##开启请求压缩
    response:
      enabled: true ## 开启响应压缩
```

同时，我们可以对请求的数据类型，以及处罚压缩的大小限制进行设置：

```yaml
feign:
  compression:
    request:
      enabled: true ##开启请求压缩
      mine-types: text/html,application/xml,application/json ##设置压缩的数据类型
      min-request-size: 2048 # 设置触发压缩的大小下限
```

注意： 上面的数据类型，压缩大小下限均为默认值。

#### 2.3 日志级别

在开发或者运行阶段往往希望看到Feign请求过程的日志记录，默认情况下Feign的日志是没有开启的。

要想用属性配置方式来达到日志效果， 只需要在application.yml中添加如下内容即可：

```yaml
feign:
  client:
    config:
      shop-service-product:
        loggerLevel: FULL
logging:
  level:
    cn.itcast.order.fegin,ProductFeiginClient: debug
```

- logging.level.xx: debug  Feign日志会对日志级别为debug的做出相应

- feign.client.config.shop-service-product.loggerLevel：配置Feign的日志级别

- Feign有四种日志级别：

  - NONE 性能最佳 用于生产 不记录任何日志（默认值）
  - BASIC 适用于生产环境追踪问题：仅记录请求方法、URL、相应状态代码以及执行时间
  - HEADERS 记录BASIC级别的基础上，记录请求和响应的Header
  - FULL 比较适用于开发及测试环境定位问题：记录请求和响应的Header Body和元数据。

  

## P49、Feign源码分析

#### 2.4 源码分析

```java
@EnableFeignClients
    FeignClientsRegistrar
        registerBeanDefinitions()方法
        FeignClientsRegistrar.class
            1 注册配置
            2 创建并注册FeignClientFactoryBean对象
               |__
```



## P50、高并发环境：模拟环境

### 3 服务注册与发现总结

#### 3.1 注册中心

##### （1）Eureka

- 搭建注册中心
  - 引入依赖spring-cloud-starter-netflix-eurk-server
  - 配置EurekaServer yml
  - 通过@EnabledEurekaServer激活EurekaServer配置
- 服务注册
  - 服务提供者引入spring-cloud-starter-netflix-enreka-client依赖
  - 通过eureka.client.serviceUrl.defaultZone配置注册中心地址

##### （2）Consul

- 搭建注册中心
  - 下载安装consul
  - 启动consul `consul agent -dev`
- 服务注册
  - 服务提供者导入spring-cloud-starter-consul-discovery依赖
  - 通过spring.cloud.consul.host和spring.cloud.consul.port指定Consul Server的请求地址

#### 3.2 服务调用

##### （1）Ribbon

- 通过Ribbon结合RestTemplate方式进行服务调用只需在声明RestTemplate的方法上添加注解@LoadBalanced即可
- 可以通过{服务名称}.ribbon.NFLoadBalancerRuleClassName配置负载均衡策略。

##### （2）Feign

- 服务消费者引入spring-cloud-starter-openfeign依赖
- 通过@FeignClient声明一个调用远程微服务接口
- 启动类上通过@EnabledFeignClients激活Feign

## P51、高并发问题：使用Jmeter模拟高负载存在的问题

### 4、微服务架构的高并发问题

#### 4.1 性能工具Jmetter

##### 4.1.1 安装Jmetter

apache-jmeter.2.13.zip

##### 4.1.2 配置Jemetter

#### 4.2 系统负载过高存在的问题

##### 4.2.1 问题分析

##### 4.2.2 线程池的形式实现服务隔离

## P52、高并发问题：问题分析

Tomcat会以线程池的形式对所有的请求进行统一的管理，对于某个方法可能存在的耗时问题的时候，随着外边积压的请求越来越多，势必会造成系统的崩溃，

为了一个方法不影响其他方法API接口的访问：可以对多个服务接口之间进行隔离

1 线程池隔离

​	针对每一个方法配置一个小的线程池 如下单方法5个线程，查询订单方法5个线程

2 信号量隔离（计数器，最大阈值 如5）

​	记录下单方法的最大阈值，超过阈值不允许方法

## P53、高并发问题：线程池隔离的方式处理请求积压问题

**引入pom坐标**

```xml
<dependency>
	<groupId>com.netflix.hystrix</groupId>
    <artifictId>hystrix-metrics-event-stream</artifictId>
    <version>1.5.12</version>
</dependency>
<dependency>
	<groupId>com.netflix.hystrix</groupId>
    <artifictId>hystrix-javanica</artifictId>
    <version>1.5.12</version>
</dependency>
```

**编写Command对象**

```java

public class OrderCommand extends HystrixCommand<Product>{
    private RestTemplate restTemplate;
    private Long id;
    public OrderCommand(RestTemplate restTemplate , Long id){
        super(setter());
        this.restTemplate=restTemplate;
        this.id=id;
    }
    
    private static Setter setter(){
        //服务分组
        HystrixCommandGroupKey groupKey = HystrixCommandGroupKey.Factory.asKey("order_product");
        HystrixCommandkey commandKey = HystrixCommandKey.Factory.asKey("product");
        HystrixThreadPoolKey threadPoolKey = HystrixThreadPoolKey.Factory.asKey("order_product_pool");
        /** 
        * 线程池配置
        *  withCoreSize： 线城池大小为10
        * withKeepAliveTimeMinutes: 线程存活时间15秒
        * withQueueSizeReJectionThreshold： 队列等待的阈值为100 超过100执行拒绝策略
        */
        HystrixThredPoolProperties.Setter HtreadPoolProperties = HystrixThreadPoolProperties.Setter().withCoreSize(50).withKeepAliveTimeMinutes(15).withQueueSizeRejectionThreshold(100);
        //命令属性peizHystrix开启超时
        HystrixCommandProperties.Setter commandProperties = HystrixCommandProperties.Setter()
            //采用线程池方式实现服务隔离
            .withExecutionIsolationStrategy(HystrixCommandProperties.ExecutionIsolationStrategy.THREAD)
            .withExecutionTimeoutEnabled(false);
        return Setter.withGroupKey(groupKey).andCommandKey(commandKey).andthreadPoolKkey(threadPoolKey)
            .andThreadPoolPropertiesDefaults(threadPoolProperties).andCommandPropertiesDeafults(commandProperties);
  
    }
    
    @overrride
    protected Product run() throws Exception{
        return restTemplate.getForObject("http://127.0.0.1/product/"+id,Product.class);        	//service-product
    }
    
    //降级方法
    @overrride
    protected Product getFallback(){
        //return null;
        Product product = new Product();
        product.setName("不好意思出错了。。。");
        return product;
    }
}
```

**使用Command调用远程服务**

```Java
public Product findById(@PathVariable Long id0){
    return OrderCommand(restTemplate,id).execute();
}
```



## P54、高并发问题：服务容错的核心知识

### 5、服务熔断Hstrix入门

#### 5.1 服务容错的核心知识

##### 5.1.1 雪崩效应

在微服务架构中，一个请求需要调用多个微服务的非常常见的，如客户端访问A服务，而A服务需要调用B服务，B服务调用C服务，由于网络原因或自身的原因，如果B服务或C服务不能及时响应，A服务就处于阻塞状态，直到B服务或C服务响应，此时如果有大量的请求涌入，容器的线程资源就会被消耗完毕，导致系统瘫痪。服务于服务之间的依赖性，故障会传播，造成连锁反应，会对整个未付系统造成灾难性的严重后果，这就是微服务故障的“雪崩”效应。

##### 5.1.2 服务隔离

顾名思义，它是指将系统安装一定的原则划分为若干个服务模块，各个模块之间相对独立，无强依赖。当有故障发生时，能将问题和影响隔离在某个模块内部，而不扩散风险，不波及其他模块，不影响整体的系统服务。

##### 5.1.3 熔断降级

熔断这一概念来源于电子工程中的断路器（Circuit Breaker），在互联网系统中，当下游服务因访问压力过大而想要变慢，或失败，上游服务为了保护整个系统的可用性，可以暂时切断对下游服务的调用，这种牺牲局部，保护整体的措施叫熔断。

![img](http://image.bubuko.com/info/201805/20180518105823816943.png)

所谓降级，就是当某个服务熔断之后，服务将不再被调用，此时客户端可以自己准备一个本地的fallback回调，返回一个缺省值，也可以理解为兜底数据

##### 5.1.4 服务限流

限流可以认为是服务降级的一种，限流就是现在系统的输入，和输出流量已到达包含系统的目的，一般来说系统的吞吐量是可以被推算的，为了保证系统的稳固运行，一旦达到需要限制的阈值，就需要限制流量并采取少量措施以完成限制流量的目的。比如：推迟解决、拒绝解决、或者部分解决等等。

## P55、基于RestTemplate的熔断配置

#### 5.2 Hystrix介绍

Hystrix是Netflix开源的一个延迟和容错库，用于隔离访问远程系统，服务或者第三方库，防止级联失败，从而提升系统的可用性与容错性，Hystrix主要通过以下几点实现延迟和容错

- 包裹请求：使用HystrixCommand包裹对依赖的调用逻辑，每个命令在独立线程中执行，这使用了设计模式中的命令模式
- 跳闸机制：当某个服务的错误率超过一定的阈值时，Hystrix可以自动或手动跳闸，停止请求该服务一段时间。
- 资源隔离：Hystrix为每个依赖都维护了一个小型线程池（或信号量）。如果线程池已满，发往该依赖的请求就会被立即拒绝，而不是排队等待，从而加速失败判定。
- 监控：Hystrix可以近乎实时的监控运行指标和配置的变化，例如成功、失败、超时、以及被拒绝的请求等。
- 回退机制：当请求失败、超时、被拒绝、或者当断路器打开是，执行回退逻辑，回退逻辑由开发人员自行提供，例如返回一个缺省值。
- 自我修复：断路器打开一段时间后，会自动进入“半开”状态。

#### 5.3 RestTemplate实现服务熔断

##### 引入hystrix的相关依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
    <artifictId>spring-cloud-starter-netflix-hystrix</artifictId>
</dependency>
```

##### 启动类中激活Hystrix

```java
@EnableCircuitBreaker
public class OrderApplication{
    
}
```

##### 配置熔断触发的降级逻辑

在Controller中加入降级方法，降级方法与需要受到保护的方法的返回值一致

```java
public class OrderController{
    
    @RequestMapping(value="/buy/{id}",method=RequestMethod.GET)
    public Product findById(@PathVariable Long id) {      
        Product product = restTemplate.getForObject("http://service-product/product/"+id,Product.class);
        return product
    }
    
    //降级方法
    //需要受到保护的方法的返回值一致 参数一致
    public Product orderFallBack(@PathVariable Long id){
        Product product = new Product();
        product.setName("触发降级方法");
        return product;
    }
}
```



##### 在需要受到保护的接口上使用@HystrixCommand注解

```java
public class OrderController{
    //配置熔断保护
    @HystrixCommand(fallbackMethod="orderFallBack")
    @RequestMapping(value="/buy/{id}",method=RequestMethod.GET)
    public Product findById(@PathVariable Long id) {      
        Product product = restTemplate.getForObject("http://service-product/product/"+id,Product.class);
        return product
    }
    
    //降级方法
    //需要受到保护的方法的返回值一致 参数一致
    public Product orderFallBack(@PathVariable Long id){
        Product product = new Product();
        product.setName("触发降级方法");
        return product;
    }
}
```

##### 超时设置

在之前的案例中，请求超过1秒都会返回错误信息，这是因为Hystrix的默认超时时长为1，我们可以通过配置修改这个值

```yaml
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 2000 #默认的连接超时时间1秒，如果1秒内没有返回数据，自动触发降级逻辑
```



#### 5.4 Feign 实现服务熔断


## P56、基于RestTemplate统一降级配置

**配置统一的降级方法**

```java
//指定统一的降低方法
//没有参数
public Product defaultFallBack(){
     Product product = new Product();
     product.setName("触发统一降级方法");
     return product;
}
```

**方法上不需要再指定降级方法**

```java
public class OrderController{
    //配置熔断保护
    // // // @HystrixCommand(fallbackMethod="orderFallBack")
    @RequestMapping(value="/buy/{id}",method=RequestMethod.GET)
    public Product findById(@PathVariable Long id) {      
        Product product = restTemplate.getForObject("http://service-product/product/"+id,Product.class);
        return product
    }
}
```

**在Controller类上配置公共的熔断设置**

```java
@DeafaultProperties(defaultFallBack="defaultFallBack")
public class OrderController{

}
```

使用统一的降级方法 要求各个方法具有相同类型的返回值

可以使用类对返回进行封装



## P57、基于Feign调用的熔断配置

引入依赖（Feign中已经集成了Hystrix）

在Feign中配置开启Hystrix

```yaml
fengin: 
  hystrix:
    enabled: true
```

自定义一个接口的实现类，这个实现类就是熔断触发的降级逻辑

```java
package com.beyondsoft.order.feign;

public classs ProductFeignClientCallBack implements ProductFeignClient {
    
    @Override
    public Product findById(Long id){
        Product product = new Product();
        product.setProductName("feign调用触发熔断降级方法")
        return product;
    }
}
```

修改FeignClient接口增加降级方法的支持

```java
@FeignClient(name="service-order",fallback=ProductFeignClientCallBack.class)
public interface ProductFeignClient{
    
}
```

Hystrix的超时时间

在规定的时间内，没有获取到微服务的数据，这个时候会自动的触发熔断的降级方法

```yaml
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 2000 #默认的连接超时时间1秒，如果1秒内没有返回数据，自动触发降级逻辑
```



## P58、通过Actuator获取Hystrix的监控数据

##### 1、引入依赖

```xml
<dependency>
    <groupId>org.springframe.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>org.springframe.cloud</groupId>
    <artifactId>spring-boot-starter-netflix-hystrix</artifactId>
</dependency>
```

##### 2、激活Hystrix

在主启动类上添加@EnableCircuitBreaker，不管是Feign还是RestTemplate都需配置这个注解

```java
@EnableCircuitBreaker
public class OrderApplication{

}
```

##### 3、配置actuator暴露端点

```yaml
management: 
  endpoints:
    web: 
      exposure:
        include: '*'
```

##### 4、浏览器查看监控数据

```
http://localhost:9001/actuator/hystrix.stream
```



## P59、通过hystrix的dashboard监控hystrix数据流

### 6 服务熔断Hystrix高级

#### 6.1 Hystrix的监控平台

除了实现容错功能，Hystrix还提供了近乎实时的监控，HystrixCommand和HystrixObservaleCommand在执行时，会生成执行解和运行指标，比如美标的请求数量，成功数量等，这些状态会暴露在Actuator提供的/health端点中，只需要为项目添加spring-boot-starter-actuator依赖，重启项目，访问 http://localhost:9001/actuator/hystrix.stream即可看到实时的监控数据

##### 6.1.1 搭建Hystrix DashBoard监控

**(1) 导入依赖**

```xml
<dependency>
    <groupId>org.springframe.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>org.springframe.cloud</groupId>
    <artifactId>spring-boot-starter-netflix-hystrix</artifactId>
</dependency>
<dependency>
    <groupId>org.springframe.cloud</groupId>
    <artifactId>spring-boot-starter-netflix-hystrix-dashboard</artifactId>    
</dependency>
```

##### (2) 启动类上添加EnableHystrixDashBoard注解

```java
@EnableHystrixDashBoard
public class OrderApplication{

}
```

##### (3) 访问测试

```
http://localhost:9001/hystrix
```

输入actuator监控地址

 http://localhost:9001/actuator/hystrix.stream 点击Monitor Srream

![img](https://img2018.cnblogs.com/blog/1107037/201909/1107037-20190908231125067-850469310.png)

![img](https://img2018.cnblogs.com/blog/1107037/201909/1107037-20190908231653328-194391044.png)

## P60、使用Turbine聚合所有的hytrix的监控数据

在微服务架构体系中，每个服务都需要配置Hystrix DashBoard监控。如果每次只能查看单个实例的监控数据，就需要不断切换监控地址，这显然很不方便。要想看这个系统的Hystrix Dashboard数据就需要用到Hystrix Turbine。Turbine是一个聚合Hystrix 监控数据的工具，他可以将所有相关微服务的Hystrix 监控数据聚合到一起，方便使用。引入Turbine后，整个监控系统架构如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201025111550231.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTIyOTI3NTQ=,size_16,color_FFFFFF,t_70#pic_center)

------

 **(1) 搭建TurbineServer**

创建工程shop-hystrix-turbine引入相关坐标

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-turbine</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>
    </dependencies>
```

(2) 配置多个微服务的hytrix监控

```yaml
server:
  port: 8031

spring:
  application:
    name: hystrix-turbine

eureka:
  client:
    service-url:
      defaultZone: http://localhost:9000/eureka/
  instance:
    prefer-ip-address: true

turbine:
  # 要监控的微服务列表，多个用,分隔
  appConfig: service-order
  clusterNameExpression: "'default'"
```

(3)  启动类上添加相关注解

```java
@SpringBootApplication
// turbin配置
@EnableTurbine
@EnableHystrixDashboard
public class TurbinApplcation {

    public static void main(String[] args) {
        SpringApplication.run(TurbinApplcation.class, args);
    }
}
```

作为一个独立项目，需要配置启动类，开启HystrixDashboard监控平台，并激活，Turbine

(4) 测试

浏览器访问 http://localhost:8031/hystrix展示HystrixDashboard。并在URL位置

输入：http://localhost:8031/turbine.stream，动态根据turbine.stream数据展示多个微服务的监控数据

![img](https://img-blog.csdnimg.cn/20201025113203662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTIyOTI3NTQ=,size_16,color_FFFFFF,t_70#pic_center)

## P61、hystrix断路器的工作状态

Hystrix可以对请求失败的请求，以及被拒绝，或者连接超时的请求进行统一的降级处理。

### 6.2 熔断器的状态

熔断器有三个状态 CLISED、OPEN\HALF_OPEN 熔断器默认关闭状态，当触发熔断后状态变更为`OPEN` ,在等待到指定的时间，Hystrix会放请求检测服务是否开启，这期间熔断器会变为`HALF_OPEN` 半开启状态，熔断探测服务可用则继续变更为 `CLOSED`关闭熔断器。

![img](https://img-blog.csdnimg.cn/20201028230024568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTIyOTI3NTQ=,size_16,color_FFFFFF,t_70#pic_center)

![img](https://img-blog.csdnimg.cn/20201028230318781.png#pic_center)

- `Closed`：关闭状态（断路器关闭），所有请求都正常访问。代理类维护了最近调用失败的次数，如果某次调用失败，则使失败次数加1。如果最近失败次数超过了在给定时间内允许失败的阈值，则代理类切换到断开(Open)状态。此时代理开启了一个超时时钟，当该时钟超过了该时间，则切换到半断开（Half-Open）状态。该超时时间的设定是给了系统一次机会来修正导致调用失败的错误。
- `Open`：打开状态（断路器打开），所有请求都会被降级。Hystix会对请求情况计数，当一定时间内失败请求百分比达到阈值，则触发熔断，断路器会完全关闭。默认失败比例的阈值是50%，请求次数最少不低于20次。
- `Half Open`：半开状态，open状态不是永久的，打开后会进入休眠时间（默认是5S）。随后断路器会自动进入半开状态。此时会释放1次请求通过，若这个请求是健康的，则会关闭断路器，否则继续保持打开，再次进行5秒休眠计时。

## P62、hystrix测试断路器的工作状态



## P63、hystrix隔离策略的说明

### 6.3 断路器的隔离策略

微服务使用Hystrix熔断器实现了服务的自动降级，让微服务具备自我保护的能力，提升了系统的稳定性，也较好的解决雪崩效应。其使用方式目前支持两种策略：

- **线程池隔离策略：**使用一个线程池来存储当前的请求，线程池对请求作处理，设置任务返回处理超时时间，堆积的请求堆积入线程池队列。这种方式需要为每个依赖的服务申请线程池，有一定的资源消耗，好处是可以应对突发流量（流量洪峰来临时，处理不完可将数据存储到线程池队里慢慢处理）
- **信号量隔离策略：**使用一个原子计数器（或信号量）来记录当前有多少个线程在运行，请求来先判断计数器的数值，若超过设置的最大线程个数则丢弃改类型的新请求，若不超过则执行计数操作请求来计数器+1，请求返回计数器-1。这种方式是严格的控制线程且立即返回模式，无法应对突发流量（流量洪峰来临时，处理的线程超过数量，其他的请求会直接返回，不继续去请求依赖的服务）

**线程池和信号量两种策略功能支持对比如下：**

![img](https://img-blog.csdnimg.cn/20201102215838522.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTIyOTI3NTQ=,size_16,color_FFFFFF,t_70#pic_center)

```
hystrix.command.default.execution.isolation.strategy : 配置隔离策略
	ExecutionIsolationStrategy.SEMAPHORE 信号量隔离
	ExecutionIsolationStrategy.THREAD 线程池隔离
hystrix.command.default.execution.isolation.maxConcurrentRequests : 最大信号量上限
```



## P64、总结

### 6.4 Hystrix核心源码

Hystrix底层是基于RxJava,RxJava是响应式编程开发库，英雌Hystrix的整个实现策略简单说就是：把一个HystrixCommand封装成一个Observabke(待观察者)，针对自身要实现的核心功能，对Observable进行各种装饰，并在订阅各步装饰的Observable,以便在指定时间到达时，添加自己的业务。

Hystrix 执行命令整体流程如下图：

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.iocoder.cn%2Fimages%2FHystrix%2F2018_10_15%2F01.jpeg&refer=http%3A%2F%2Fwww.iocoder.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1613282514&t=2b49a0b57ca3e870102204a452a41fe1)

Hystrix主要有4种调用方式：

*toObservable()* ：未做订阅，只是返回一个Observable
*observe()*：调用 #toObservable() 方法，并向 Observable 注册，rx.subjects.ReplaySubject发起订阅，因此它具有回放的能力
			observe() 方法使用了ReplaySubject缓存了toObservable的消息，使得执行后再监听也可以收到所有消息。新订阅者连历史数据也能够监听到（1分钟内）
*queue()*：调用toObservable().toBlocking().toFuture()返回 Future 对象
*execute()*：调用#queue() 方法的基础上，马上调用 Future#get() 方法，同步返回 #run() 的执行结果。

## P65、Sentinel概述

### 7 服务熔断Hystrix的替换方案

2018年底Netflix公司宣布Hystrix已经足够稳定，不再积极开发Hystrix，该项目处于维护模式。就目前来看Hystrix是比较稳定的，并且Hystrix只是停止开发新的版本，并不是完全停止维护，Bug什么的依然会维护。因此，短期内，Hystrix依然是能继续使用的。但是从长远看，Hystrix总会达到它的生命周期，那么Spring Cloud生态中是否有替代产品呢？

#### 7.1 替换方案介绍

##### Alibaba Sentinel

Sentinel是阿里开源的一款熔断器的实现，目前在Spring Cloud的孵化器项目Spring Cloud Alibaba中的一员Sentinel本身在阿里内部已经被大规模采用，非常稳定。因此，可以作为一个很好的替代品。

##### Resilience4

Resilience4J是一款轻量、简单，并且文档非常清晰、丰富的熔断工具，这也是Hystri官方推荐的替代产品。不仅如此，Resilience4J还原生支持SpringBoot 1.x/2.x，而且监控也不像Hystrix一样弄Dashboard/Hystrix等一堆轮子，而是支持和micrometer、prometheus以及Dropwizard metrics进行整合。

#### 7.2 Sentinel概述

##### 7.2.1 Sentinel简介

随着微服务的流行，服务和服务之间的稳定性变得越来越重要。Sentinel 以流量为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。

Sentinel 具有以下特征:

- **丰富的应用场景**：Sentinel 承接了阿里巴巴近 10 年的双十一大促流量的核心场景，例如秒杀（即突发流量控制在系统容量可以承受的范围）、消息削峰填谷、集群流量控制、实时熔断下游不可用应用等。
- **完备的实时监控**：Sentinel 同时提供实时的监控功能。您可以在控制台中看到接入应用的单台机器秒级数据，甚至 500 台以下规模的集群的汇总运行情况。
- **广泛的开源生态**：Sentinel 提供开箱即用的与其它开源框架/库的整合模块，例如与 Spring Cloud、Dubbo、gRPC 的整合。您只需要引入相应的依赖并进行简单的配置即可快速地接入 Sentinel。
- **完善的 SPI 扩展点**：Sentinel 提供简单易用、完善的 SPI 扩展接口。您可以通过实现扩展接口来快速地定制逻辑。例如定制规则管理、适配动态数据源等。

**Sentinel特性**

![Sentinel的主要特性](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022162012669-1932997933.png)

##### 7.2.2 Sentinel与Hystrix的区别

| 项             | Sentinel                                                   | Hystrix                 | resilience4j                     |
| -------------- | ---------------------------------------------------------- | ----------------------- | -------------------------------- |
| 隔离策略       | 信号量隔离（并发线程数限流）                               | 线程池隔离/信号量隔离   | 信号量隔离                       |
| 熔断降级策略   | 基于响应时间、异常比率、异常数                             | 基于异常比率            | 基于异常比率、响应时间           |
| 实时统计实现   | 滑动窗口（LeapArray）                                      | 滑动窗口（基于 RxJava） | Ring Bit Buffer                  |
| 动态规则配置   | 支持多种数据源                                             | 支持多种数据源          | 有限支持                         |
| 扩展性         | 多个扩展点                                                 | 插件的形式              | 接口的形式                       |
| 基于注解的支持 | 支持                                                       | 支持                    | 支持                             |
| 限流           | 基于 QPS，支持基于调用关系的限流                           | 有限的支持              | Rate Limiter                     |
| 流量整形       | 支持预热模式、匀速器模式、预热排队模式                     | 不支持                  | 简单的 Rate Limiter 模式         |
| 系统自适应保护 | 支持                                                       | 不支持                  | 不支持                           |
| 控制台         | 提供开箱即用的控制台，可配置规则、查看秒级监控、机器发现等 | 简单的监控查看          | 不提供控制台，可对接其它监控系统 |

##### 7.2.3 Sentinel迁移方案

Sentinel官方提供了详细的由Hystrix迁移到Sentinel的方法

 [迁移指南](https://github.com/alibaba/Sentinel/wiki/Guideline:-从-Hystrix-迁移到-Sentinel)

| Hystrix功能           | 迁移方案                                                     |
| --------------------- | ------------------------------------------------------------ |
| 线程池隔离/信号量隔离 | Sentinel不支持线程池隔离；信号量隔离对Sentinel中的线程数限流，详见[此处](https://github.com/alibaba/Sentinel/wiki/Guideline:-从-Hystrix-迁移到-Sentinel#信号量隔离) |
| 熔断器                | Sentinel 支持按平均响应时间、异常比率、异常数来进行熔断降级。从 Hystrix 的异常比率熔断迁移的步骤详见[此处](https://github.com/alibaba/Sentinel/wiki/Guideline:-从-Hystrix-迁移到-Sentinel#熔断降级) |
| Command 创建          | 直接使用 Sentinel `SphU` API 定义资源即可，资源定义与规则配置分离，详见[此处](https://github.com/alibaba/Sentinel/wiki/Guideline:-从-Hystrix-迁移到-Sentinel#command-迁移) |
| 规则配置              | 在 Sentinel 中可通过 API 硬编码配置规则，也支持多种动态规则源 |
| 注解支持              | Sentinel 也提供注解支持，可以很方便地迁移，详见[此处](https://github.com/alibaba/Sentinel/wiki/Guideline:-从-Hystrix-迁移到-Sentinel#注解支持) |
| 开源框架支持          | Sentinel 提供 Servlet、Dubbo、Spring Cloud、gRPC 的适配模块，开箱即用；若之前使用 Spring Cloud Netflix，可迁移至 [Spring Cloud Alibaba](https://github.com/spring-cloud-incubator/spring-cloud-alibaba) |

##### 7.2.4 名词介绍/基本概念

Sentinel ：可以简单的分为 Sentinel 核心库和 Dashboard。核心库不依赖 Dashboard，但是结合 Dashboard 可以取得最好的效果。

主要用于流量控制，熔断降级     

使用 Sentinel 来进行资源保护，主要分为几个步骤:

1、 定义资源

2、定义规则

3、检验规则是否生效

**资源：**可以是任何东西，服务，服务里的方法，甚至是一段代码。    

**规则：**Sentinel 支持以下几种规则：流量控制规则、熔断降级规则、系统保护规则、来源访问控制规则 和 热点参数规则。Sentinel的所有规则都可以在内存中动态的查询及修改，修改之后立即生效

先把可能需要保护的资源定义好，再配置规则。也可以理解为，只要有了资源，我们就可以在任何时候灵活地定义各种流量控制规则，在编码的时候，只需要考虑这个代码是否需要保护，入股偶需要保护，就将其定义为一个资源。

## P66、Sentinel管理控制台的安装与执行

#### 7.3 Sentinel中的管控控制台

##### 7.3.1下载启动控制台

**(1) 获取Sentinel控制台**

可以从官方网站中下载最新版本的控制台jar包，下载地址如下

https://github.com/alibaba/Sentinel/releases

https://github.com/alibaba/Sentinel/releases/download/v1.8.0/sentinel-dashboard-1.8.0.jar

**(2) 启动**

使用如下命令启动控制台

```bash
# -Dserver.port=8080用于指定Sentinel控制台端口为8080
java -Dserver.port=8080 -Dcsp.sentinel.dashboard.server=localhost:8080 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard-1.8.0.jar
```

Sentinel的登录界面（访问地址默认是http://localhost:8080/，用户名和密码为sentinel/sentinel）：

![Sentinel的登录页面](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022162012980-1029674296.png)

## P67、Sentinel客户端接入管理控制台

##### 7.3.2 客户端接入控制台

控制台启动会，客户端需要按照以下步骤接入控制台

**（1）引入JAR包**

客户端需要引入Transport模块与Sentinel控制台进行通信，可以通过pom.xml引入JAR包：

```xml
<dependency>
	<groupId>com.alibaba.csp</groupId>
    <artifactid>sentinel-transport-simple-http</artifactid>
</dependency>
```

父工程引入alibaba实现的SpringCloud

```xml
<dependency>
	<groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

子功能模块引入依赖

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```

需要注意SpringCloud-Alibaba与SpringCloud、SpringBoot三者之间的版本关系

| Spring Cloud Version   | Spring Cloud Alibaba Version | Sproing Boot Version |
| ---------------------- | ---------------------------- | -------------------- |
| Spring Cloud Greenwich | 2.1.0.RELEASE                | 2.1.X.RELEASE        |
| Spring Cloud Finchley  | 2.0.0.RELEASE                | 2.0.X.RELEASE        |
| Spring Cloud Edgware   | 1.5.0.RELEASE                | 1.5.X.RELEASE        |

**（2）配置启动参数**

在工程的application.yml中添加Sentinel控制台配置信息

```yaml
spring:
  cloud:
    sentinel:
      transport:
        dashboard: 127.0.0.1:8080            # 配置Sentinel Dashboard地址
```

##### 7.3.3 查看机器列表以及监控情况

默认情况下Sentinel会在客户端首次调用的时候进行初始化，开始向控制台发送心跳包，也可通过配置sentinel,eager=true，取消sentinel控制台懒加载。

## P68、Sentinel通用资源保护

### 7.4 基于Sentinel的服务保护

#### 7.4.1 通用资源保护

通用资源保护比较适用于RestTemplate调用方式风格

**（1）案例准备** 

​	创建子模块shop_service_order_rest_sentinel

**（2）引入POM依赖**

**（3）配置熔断降级方法**

在OrderController中添加熔断降级方法

定义降级逻辑：Hystrix和Sentinel不同

Hystrix不管是熔断或者抛出异常，执行的降级方法是一样的

Sentinel可以分别指定熔断和抛出异常执行的降级方法

```java
public class Order Controller{
    
    @SentinelResource(blockHandler="orderBlockHandler",fallback="orderFallback")
    public Product findById(Long id){
        return restTemplate.getForObject("",Product.class);
    }    
    
    public Product orderBlockHandler(Long id){
        Product product = new Product();
        product.setName("触发熔断执行的方法");
        return product;
    }
    
    public Product orderFallback(Long id){
        Product product = new Product();
        product.setName("抛出异常执行的方法");
        return product;
    }
}
```

熔断保护和熔断降级的设置 是通过Sentinel控制台来设置的，实时拉取控制台设置

## P69、Sentinel加载本地配置

**通过文件读取限流规则**

```yaml
spring:
  cloud:
    centinel:
      datasource:
        ds1:
         file:
          file: classpath:flowrule.json
          data-type: json
          rule-type: flow
```

resources 目录下flowrule.json

```json
[
    {
        "resource": "orderFindById",  #资源名称
        "controlBehavior": 0,         #流量控制效果
        "count": 1,  #流量阈值
        "grade": 1,   #流量阈值类型
        "limitApp":"default", #请求来源
        "strategy"
    }
]
```



## P70、Sentinel对RestTemplate的支持

对RestTemplate做注解，所有使用RestTemplate 的方法都做保护

@SentinelResTemplate

Sentinel RestTemplate 限流的资源规则提供两种粒度：

httpmethod:schema://host:port/path: 协议、主机名、端口和路径

httpmethod:schema://host:port : 协议、主机名、端口

```java
public class RestORderApplication{
    
    @Bean
    @LoadBalanced
    @SentinalResTemplate
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

异常降级 

​		fallback  降级方法

​        fallbackClass 降级配置类

限流熔断

​		blockHandler

​		blockHandlerClass

```java
public class RestORderApplication{
    
    @Bean
    @LoadBalanced
    @SentinalResTemplate()
    public RestTemplate restTemplate(fallbackClass=ExceptionUtils.class,fallback  ="handlerFallback" ){
        return new RestTemplate();
    }
}
```

```java
public class ExceptionUtils{
    //要求是静态方法 
    //返回值类型SentinelClientHttpReponse
    
    public static SentinelClientHttpReponse handlerBlock(HttpRequest request, byte[] body ,ClientHttpRequestExecution execution,BlockException){
        System.out.print("Oops:"+ex.getClass().getCanonicalName());
        Product product = new Product();
        product.setName("Block 限流熔断降级")
        return new SentinelClientHttpReponse(JSON.toJSONString(product));
    }
    
    public static SentinelClientHttpReponse handlerFallback(HttpRequest request, byte[] body ,ClientHttpRequestExecution execution,BlockException){
                System.out.print("Oops:"+ex.getClass().getCanonicalName());
        Product product = new Product();
        product.setName("Fallback 异常熔断降级")
        return new SentinelClientHttpReponse(JSON.toJSONString(product));
    }
}
```



## P71、Sentinel对feign的支持

**(1) 引入依赖**

```xml
spring-cloud-starter-alibaba-sentinel
spring-cloud-starter-openfeign
```

**(2) 开启Sentinel支持**

在工程的application.yml中添加sentinel对feign的支持

```yaml
feign:
  sentinel:
    enabled: true
```

**(3) 配置FeignClient**

和使用Hystrix的方式基本一致，需要配置FeignClient接口以及通过fallback指定熔断降级方法

```java
@FeignClient(name="shop-service-product",fallback=ProductFeignClientCallBack.class)
public interface ProductFeiginClient{
	@RequestMapping(value="/product/{id}",method=RequestMethod.GET)
	public Product findById(Long id);
}
```

**(4) 配置熔断方法**

```java
@Componet
public class ProductFeiginClientCallBack implements ProductFeignClient{
	public Product findById(Long id){
		Product product = new Product();
		product.setId(-1L);
		product.setName("熔断：触发降级方法");
		return product;
	}
}
```

第三步和第四步 和Hystrix的Feign的方式是一样的

## P72、第二天总结

##### Feign组件

##### Hystrix组件

##### Sentinel组件

## P73、回顾和今日内容介绍

内容介绍：

##### 微服务/网关API网关

##### 链路追踪

## P74、微服务网关的引入

在学完上面的之后后，微服务架构已经出具雏形，但是还存在一些问题，不同的微服务一般会部署在不同的服务器上，有不同的网络地址，客户端访问这些微服务时，必须记住几十甚至几百个地址，这对客户端来说太复杂也难以维护

如果让客户端daunt与各个微服务通讯，会存在很多问题：

- 客户端会请求多个不同的服务，需要维护不同的请求地址，增加开发难度
- 在流行场景下存在跨域请求的问题
- 加大身份验证的难度，每个微服务需要独立认证

因此我们需要一个微服务网关，介于客户端与微服务之间的中间层，所有的外部请求都会先经过微服务网关，客户端只需要与网关交互，只要知道一个网关的地址即可， 这样简化了开发，具有以下优点

- 易于监控
- 易于认证
- 减少了客户端与各个微服务之间的交互次数

## P75、 微服务网关

#### 1.1 微服务网关的概念

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201014134223220-1541395353.jpg)

##### 1.1.1 什么是微服务网关

API网关是一个服务器，是系统对外的唯一入口，API网关封装了系统内部架构，为每个客户端提供一个定制API，API网关的核心要点是，所有的客户端和消费端都通过统一的网关接入微服务，在网关层处理所有的非业务功能。通常，网关实时通过REST/HTTP的API。服务端通过API-GW注册和管理服务。

##### 1.1.2 作用和应用场景

网关具有的职责，如身份验证、监控、负载均衡、缓存、请求分片与管理、静态响应处理。当然最主要的支持还是与“外界联系”。

#### 1.2 常用的API网关实现方式

- **Nginx+lua实现**

  使用Nginx的反向大力和负载均衡可以实现对api服务器的负载均衡及高可用

  缺点：自注册的问题和网关本身的扩展性。

- **Kong** 

  基于Nginx+Lua开发，性能高，稳定，有多个可用的插件(限流、鉴权等待)可用开箱即用。

  缺点：只支持http协议；二次开发、自由扩展困难； 缺乏更易用的管控/配置方式。

  非微服务环境下可以使用

- **Zuul **

  Netflix开源，功能丰富，使用JAVA开发，易于二次开发；需要运行在web容器中，如Tomcat。

  问题：缺乏管控，无法动态配置；依赖的组件较多；处理http请求依赖的是web容器，性能不如Nginx好。

- **Traefik**

  Go语言开发的；轻量易用；提供大多数的功能：服务路由；负载均衡等等；提供WebUI

  问题：二进制文件部署，二次开发难度大；UI更多的是监控，缺乏配置、管理能力

- **SpringCloud Gateway**

  SpringCloud提供的网关



## P76、Nginx模拟微服务网关

#### 1.3 基于Nginx的网关实现

##### 1.3.1 Nginx介绍

Nginx是一款自由的、开源的、高性能的http服务器和反向代理服务器；同时也是一个IMAP POP3 SMATP代理服务器；Nginx可以作为一个Http服务器进行网站的发布出来，两位Nginx可以作为反向代理实现负载均衡

##### 1.3.2 正向/反向代理

##### （1） 正向代理

正向代理，“它代理的是客户端，代客户端发出请求”，是一个位于客户端和原始服务器(origin Server)之间的服务器，为了从原始服务器取得内容，客户端向代理付出一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理。

![img](https://img-blog.csdnimg.cn/20190926135119475.jpg)

正向代理的用途：

- 访问原来无法访问的资源，如Google
- 可以做缓存，加速访问资源
- 对客户端访问授权，上网进行认证
- 代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息

##### （2）反向代理

反向代理，“它代理的是服务端，代服务端接收请求”，主要用于服务器集群分布式部署的情况下，反向代理隐藏了服务器的信息。

![img](https://img-blog.csdnimg.cn/20190926135917861.png)

反向代理的作用：

- 保证内网的安全，通常将反向代理作为公网访问地址，Web服务器是内网
- 负载均衡，通过反向代理服务器来优化网站的负载

##### （3）正向+反向 场景

实际项目操作时，正向代理和反向代理很有可能会存在在一个应用场景中，正向代理代理客户端的请求去访问目标服务器，目标服务器是一个反向代理利服务器，反向代理了多台真实的业务处理服务器。具体的拓扑图如下

![img](https://img-blog.csdnimg.cn/20190926140037807.png)

（4）正向代理 反向代理的区别

![img](https://img-blog.csdnimg.cn/20190926140139194.png)

图解：

在正向代理中，Proxy和Client同属于一个LAN（图中方框内），隐藏了客户端信息；

在反向代理中，Proxy和Server同属于一个LAN（图中方框内），隐藏了服务端信息；

实际上，Proxy在两种代理中做的事情都是替服务器代为收发请求和响应，不过从结构上看正好左右互换了一下，所以把后出现的那种代理方式称为反向代理了

##### 1.3.2 配置

准备两个个微服务order-service 和 product-service

启动 Nginx

配置 Nginx

```
# 商品服务
location /api-propduct{
    proxy_paxx http://127.0.0.1:9001/;
}
# 订单服务
location /api-order{
    proxy_paxx http://127.0.0.1:9002/;
}
```

浏览器测试

​	访问 http://localhost:api-product/product/1

​	访问 http://localhost:api-order/order/1

## P77、Zuul网关介绍

##### Zuul网关（了解）

- Zuul的简单介绍

- 搭建Zuul网关服务器

- 路由

- 过滤器 

- 内部源码

##### SpringCloud gateway网关（重点）



## P78、Zuul搭建环境

#### 1.1 Zuul简介

Zuul是Netflix开源的微服务网关，它可以和Eureka、Ribbon、Hystrix等组件配合使用，Zuul组件的核心是一系列的过滤器 ，这些过滤器可以完成以下功能：

- **动态路由：**动态将请求路由到不同后端集群

- **压力测试：**主键增加指向集群的流量，以了解性能

- **负载分配：**为每一种负载类型分配对应容量，并启用超出限定值的请求

- **静态响应处理：**编译位置进行响应，避免转发到内部集群

- **身份认证和安全：** 识别每一个资源的验证要求，并拒绝那些不符的请求。

SpingCloud对Zuul进行了整合和增强



#### 1.2 搭建zuul网关服务器

##### 1、创建工程，导入pom坐标

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
        </dependency>
```

##### 2、配置启动类 开启网关服务器功能

```java
@SpringBootApplication
@EnableZuulProxy  //开启Zuul网关功能  非@EnableZuulServer
public class ZuulServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ZuulServerApplication.class, args);
    }
}
```

##### 3、配置文件

```yml
server:
  port: 8080
spring:
  application:
    name: api-zuul-server

```


## P79、Zuul路由：基础路由配置

#### 1.3 路由

路由：根据请求的URL，将请求转发/分配到对应的微服务中进行处理。

##### 2.3.1 基础路由配置

```yaml
#路由配置
zuul:
  routes:
    #以商品微服务举例
    product-service: #路由id 随便写
      path: /product-service/** #映射路径localhost:8080/product-service/xxx
      url: http://127.0.0.1:9001  #映射路径对应的实际微服务的地址
```

## P79、Zuul路由：基础路由配置

#### 1.3  路由

路由：根据请求的URL 将请求分配到对应的微服务中进行处理

##### 1.3.1 基础路由配置

```yaml
server:
  port: 8080
spring:
  application:
    name: api-zuul-server
zuul:
  routes: #路由配置
    product-service: #路由的ID 随意命名 不重复即可
      path: /product-service/**  ##映射路径
      url: http://127.0.0.1:9001  ##映射路径对应的实际微服务url地址      
```

## P80、Zuul路由：面向服务的路由配置

##### 1.3.2 面向服务的路由配置

1、Zuul与Eureka整合 添加 Eureka的依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2、开启Eureka的客户端服务发现

```java
@SpringBootApplication
@EnableZuulProxy
@EnableDiscoveryClient //或者@EnableEurekaClient  或者省略
public class ZuulServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ZuulServerApplication.class, args);
    }
}
```

3、Zuul网关服务中配置Eureka的注册中心的相关信息

```yml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:9000/eureka/

  instance:
    prefer-ip-address: true  #使用ip地址注册
```

4、 修改路由中的映射配置

```yml
    product-service:
      path: /product-service/**
      #url: http://127.0.0.1:8086
      serviceId: service-product  # 配置转发的微服务的服务名称
```

## P81、Zuul路由：简化路由的配置

##### 1.3.3 简化路由配置

如果路由id 与service服务名称一致的话，可以使用如下简化配置

```yaml
zuul:
  routes: #路由配置
    service-product: /product-service/**
```

zuul中的默认路由配置

如果当前的微服务名称 是service-product ,则默认的请求路径是 /service-product/**

```yaml
zuul:
  routes: #路由配置
    #不配置微服务名称 按zuul默认的
```

## P80、Zuul路由：面向服务的路由配置

##### 2.3.2 面向服务的路由配置

微服务异步是由几十、上百个微服务组成，对于一个URL请求，最终会确认一个服务实例进行处理，如果对每个服务实例手动指定一个唯一访问地址，然后根据URL去手动实现请求匹配，这样做显然不合理。

Zuul支持与Eureka整合开发，根据ServiceID自动的从注册中心中获取服务地址并转发请求，这样做的好处不仅仅可以通过单个端点来访问应用的所有服务，而且在添加或移除服务实例的时候不用修改Zuul的路由配置。

**（1）添加Eureka客户端依赖**

```xml
<dependency>
	<groupId>org.springframe.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

**（2）开启Eureka的客户端发现功能**

```java
@SrpingBootApplication
@EnableZuulProxy
@EnableDiscoveryClient//添加Eureka的服务发现  可以省略
public class ZuulServerApplication{
    public static void maain(String[] args){
        SpringApplication.run(ZuulServerApplication.class,args);
    }
}
```

**（3）在Zuul中配置Eureka的注册中心**

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:9000/eureka/,http://localhost:9000/eureka
  instance:
    prefer-ip-address: true #使用ip地址注册
```

**（4）修改路由中的映射配置**

```yaml
#路由配置
zuul:
  routes:
    #以商品微服务举例
    product-service: #路由id 随便写
      path: /product-service/** #映射路径localhost:8080/product-service/xxx
      #url: http://127.0.0.1:9001  #映射路径对应的实际微服务的地址
      serviceId: service-product  #配置转发的微服务的的服务名称
```



## P81、Zuul路由：简化路由的配置

##### 2.3.3 简化路由配置

如果路由id和 对应的微服务的serviceId一致的话，可以将

```yml
#路由配置
zuul:
  routes:
    #以商品微服务举例
    product-service: #路由id 随便写
      path: /product-service/** #映射路径localhost:8080/product-service/xxx
      #url: http://127.0.0.1:9001  #映射路径对应的实际微服务的地址
      serviceId: service-product  #配置转发的微服务的的服务名称
```

简化为

```yaml
#路由配置
zuul:
  routes:    
    service-product: /product-service/**   #配置转发的微服务的的服务名称
```

**Zuul默认路由配置规则**

如果当前的微服务名称为 service-product，则默认的请求映射路径为/service-product/**

```yaml
# 如不配置 service-order 那么默认的 映射路径为
service-order /service-order/**
```

###### 



##### 2.3.4 Zuul加入之后的架构

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201014134435110-1964844521.jpg)



## P82、Zuul过滤器：入门

#### 2.4  Zuul中的过滤器

##### 2.4.1 ZuulFilter简介

Zuul中的过滤器跟我们之前使用的javax.servlet.Filter不一样，javax.servlet.Filter 只有一种类型，可以通过配置 urlPatterns 来拦截对应的请求。而 Zuul 中的过滤器总共有 4 种类型，且每种类型都有对应的使用场景。

请求的校验、服务的聚合

ZuulFilter 的四种类型

- ###### PRE Filter

  转发到无服务之前执行请求之前，过滤，对身份信息进行识别校验

- ###### Routing Filter

  负载均衡

- ###### POST Filter

  添加请求头Header、收集日志等

- ###### Error Filter

  其他阶段错误 抛出异常时 执行的过滤器

Zuul提供了自定义过滤器的功能实现起来也十分简单，只需要编写一个类去实现zuul提供的接口

```java
public abstract ZuulFilter implements IZuulFilter{
    abstact public String filterType();
    abstact public int filterType();
    boolean shouldFilter();				//来自IZuulFilter
    Object run() throws ZuulException;  //IZuulFilter
}
```

ZuulFilter是过滤器的顶级福来，在这里定义了4个重要的方法：

1）shouldFilter

是否执行该过滤器，true 为执行，false 为不执行，这个也可以利用配置中心来实现，达到动态的开启和关闭过滤器。

2）filterType

过滤器类型，可选值有 pre、route、post、error。

3）filterOrder

过滤器的执行顺序，数值越小，优先级越高。

4）run 

过滤器的具体执行业务逻辑

##### 1.4.2 生命周期

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA4MjMvNS0xWlIzMTAzNDQzMk4ucG5n?x-oss-process=image/format,png)

正常流程执行的顺序，请求发过来首先到 pre 过滤器，再到 routing 过滤器，最后到 post 过滤器，任何一个过滤器有异常都会进入 error 过滤器。

##### 1.4.3 自定义过滤器

自定义过滤器，实现ZuulFilter抽象类

```java
/**
	自定义ZuulFilter
*/
public Class LoginFilter extend ZuulFilter{
    /**
    * 定义过滤器类型
    * pre routing post error
    */
    public String filterType(){
        return 'pre';
    }
    /**
    * 指定过滤器的执行属性
    * 返回值越小，执行顺序越高
    */
    public int filterOrder(){
        return 1;
    }
    
    /** 
    * 当前过滤器是否生效
    * 是否使用过滤器：true 使用; false 不使用
    */
    public boolean shouldFilter(){
        return true;
    }
    /**
    * 执行过滤器中的业务逻辑
    * 指定过滤器中的业务逻辑
    */
    public Object run() throws ZuulException{
        System.out.pringln("执行了过滤器");
        return null;
    }
}
```

## P83、Zuul过滤器：身份认证过滤器


使用最多的是pre过滤器，用于做身份验证 身份识别

示例：认证客户端token

1、所有的请求都协大一个参数 acccess-toker

2、获取request请求

3、通过request获取参数access-token

4、判断token是否合法

在zuul网关中 通过RequestContext的上下文对象，可以获取request，（Zuul是 通过threadLocal实现）

```java
    /**
     * 执行过滤器中的业务逻辑
     * 身份认证：
     * 1.所有的请求需要携带一个参数:access-token
     * 2.获取request请求
     * 2.通过request请求对象获取access-token
     * 4判断token是否为空
     * 4.1如果token == null，身份验证失败
     * 4.2如果token != null，执行后续操作
     * 在Zuul网关中，通过RequestContext的上下文对象，可以获取对象request对象。
     *
     * @return
     * @throws ZuulException
     */
    @Override
    public Object run() throws ZuulException {
        //1.获取zuul提供的上下文对象RequestContext
        RequestContext currentContext = RequestContext.getCurrentContext();
        //2.从RequestContext中获取Request
        HttpServletRequest request = currentContext.getRequest();
		//3.获取请求参数access-token
        String token = request.getHeader("access-token");
		//4.判断
        //4.1如果token为空，身份认证失败
        if (StringUtils.isEmpty(token)) {
            currentContext.setSendZuulResponse(false); //拦截请求
            currentContext.setResponseStatusCode(HttpStatus.UNAUTHORIZED.value()); //设置响应的状态码
        }
        //4.2 如果token不为空，执行后续操作
        return null;
    }
```



## P84、Zuul源码分析

#### 2.5  Zuul内部源码

Spring.factirues:

​	ZoolProxyAutoConfiguration 

​	父类

​	ZuulServerAutoConfigureation

​		ZuulHandlerMapping: 会自动加入到SpringMVC的handlerMapping链中，用于处理Zuul的映射配置

​		ZuulController: 配置ZuulServlet，将所有服务规则的请求，交给 处理

#### 2.6 Zuul网关存在的问题

在实际使用中我们会发现直接使用Zuul存在诸多问题，包括：

- 1、性能问题：Zuul 1.x版本本质上就是一个同步阻塞的Servlet，采用多线程阻塞的模型进行请求转发。简单的讲，每一个请求，Servlet容器要为该请求分配一个线程专门负责处理这个请求，直到响应返回客户端这个线程才会被释放返回容器线程池。如果后台服务调用比较耗时，那么这个线程就会被阻塞，阻塞期间线程资源被占用，不能干其他事情。我们知道Servlet容器线程池的大小是由限制的，当前端请求量大，而后台慢服务比较多的时候，很容易耗尽容器线程池内的线程，造成容器无法接收新的请求。
- 2、不支持任何长连接，如WebSockt。

#### 2.7 Zuul网关的替代方案

- **Zuul 2.X版本**  SpringCloud目前还没有整合Zuul 2.x版本。

- **SpringCloud Gateway**



## P85、SpringCloud GateWay概述

Zuul 1.x 是一个基于阻塞IO的API GetWay以及Servlert；直到2018年5月，Zuul 2.x(基于Netty 非阻塞的，支持长连接) 才发布，但是SpringCloud暂时没有整合计划。SpringCloud Gateway比Zuul 1.x系列的性能和功能整体要好。

- 不支持长连接 如websocket

- 基于阻塞IO的API GetWay

### 3 微服务网关Gateway

#### 3.1 gateway简介

##### 3.1.1 简介

Spring Cloud GateWay是Spring官方基于Spring5.0，SpringBoot2.0 和ProjectReactor等技术开发的网关，旨在为微服务架构提供一种简单而有效的统一API路由管理方式，统一访问接口。Spring Cloud Gateway 作为Spring Cloud生态系统中的网关，目标是替代Netflix ZUUL，它不仅提供统一的路由方式，而且基于Filter链的方式提供了网关的基本功能，例如：安全，监控/埋点，和限流等，它是基于Netty的响应式开发模式。

SpringCloud gataway 性能大概是Zuul的1.6倍

**SpringCloud gataway 网关课程内容介绍**

- 3.1 Gateway简介
- 3.2 路由配置
- 3.3 过滤器
- 3.4 统一鉴权
- 3.5 网关限流
- 3.6 网关的高可用

##### 3.1.2 核心概念

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161040773-1513563606.png)

**1. 路由（Route）**：路由是网关最基础的部分，路由信息由一个ID，一个目的URL、一组断言工厂和一组Filter组成。如果断言为真，则说明请求URL和配置的路由匹配。

**2.断言（Predicate）**：Java8中的断言函数，Spring Cloud Gateway中的断言函数输入类型是Spring5.0框架中的ServerWebExchange。Spring Cloud Gateway中的断言函数允许开发者去定义匹配来自http Request中的任何信息，比如请求头和参数等。

**3.过滤器（Filter）**：一个标准的Spring WebFilter，Spring Cloud Gateway中的Filter分为两种类型：**Gateway Filter**和**Global Filter**。过滤器Filter可以对请求和响应进行处理。

 **GatewayFilter ** 某个路由的过滤器

**GlobolFilter**  全局过滤器



## P86、SpringCloudGateway路由：基本配置

#### 3.2 入门案例

##### 3.2.1 入门案例

##### （1）创建工程 导入依赖

工程名：API-gateway-server

```xml
<!-- 
	SpringCloud Gateway的内部是通过netty + webflux实现
	Wsebflux和SpringMVC存在冲突
-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>    
```

##### （2）配置启动类

```java
@SpringBootApplication
public class GatewayServerApplication{
    public static void main(String[] args){
        SpringApplication.run(GatewayServerApplication.class,args);
    }
}
```

##### （3）编写配置文件

```yml
server: 
  port: 8080
spring:
  application:
    name: api-gateway-server #服务名称
    cloud:
      gateway:
        routes:
        #配置路由： 路由id 路由到微服务的uri 断言(判断条件)
        - id: product-service  #保持唯一
          url: http://127.0.0.1:9001  #目标微服务请求地址
          predicates: 
          - Path=/product/**  #路由条件 Path：路由配套条件
```



## P87、SpringCloudGateway路由：gateway依赖问题和内置断言条件介绍

**解决项目报错**

SpringCloud Gateway的内部是通过netty + webflux实现

Wsebflux和SpringMVC存在冲突，所以如果工程中同时引入了SpringCloud Gateway和SpringMvc就会抛出异常

解决方法：移除父工程中的spring-boot-starter-web，将其移动到需要使用的 

springcloudgateway 的内部是通过netty+webfex 实现的

webflex会和springmvc web冲突，解决方法是不要在父工程中引入web的依赖

##### 3.2.2 路由规则

Spring Cloud Gateway的功能很强大，上面我们只是使用了Path的predicates进行了简单的条件匹配，其实Spring Cloud Gateway帮准我们内置了很多Predicates功能。在Spring Cloud Gateway中Spring利用Predicate的特性实现了各种路由匹配规则，有通过header、请求参数等不同的条件来作为条件匹配到对应的路由。

[![Spring Cloud Gateway路由规则](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161041326-1414793846.jpg)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161041326-1414793846.jpg)

断言：也就是路由条件

**断言条件**

- After 在某个时间节点之后再进行路由匹配
- Before 某个时间节点之前再进行路由匹配 
- Between 某个时间节点之间再进行路由匹配 
- Cookie 满足Cookie中携带某些Cookie值
- Header 
- Host 主机地址
- Method 请求方式如Post Get
- Query 根据请求参数
- RemoteAddr 根据请求地址
- Path 路径

**断言的示例**

- 示例：在某个时间之前允许转发

```yaml
spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - Before=2020-11-11T00:00:00+08:00[Asia/Shanghai]   # 在2020-11-11T00:00:00之前允许访问
```

- 示例：在某个时间之后允许转发

```yaml
spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - After=2020-11-11T00:00:00+08:00[Asia/Shanghai]   # 在2020-11-11T00:00:00之后允许访问
```

- 示例：在某个时间段内允许转发

```yaml
spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - Between=2018-11-11T00:00:00+08:00[Asia/Shanghai],2020-11-11T00:00:00+08:00[Asia/Shanghai]   
```

- 示例：通过Cookie匹配

```yaml
spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）  
            # 如果cookie的名称是abc，cookie的值是根据下面的正则表达式匹配
            - Cookie=abc,admin       # Cookie的name,正则表达式  
```

- 示例：通过Header属性匹配

```yaml
spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - Header=X-Request-Id, \d+  # Header头名称，正则表达式
```

- 示例：通过Host匹配

```yaml
spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - Host=**.jd.com         # http://surveys.jd.com/和http://passport.jd.com/等都可以匹配
```

- 示例：通过请求方式匹配

```yaml
spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - Method=GET   #GET请求匹配
```

- 示例：根据请求路径匹配

```yaml
server:
  port: 7007

spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - Path=/foo/{segment}    # http://localhost:7007/foo/a等都可匹配
```

- 示例：根据请求参数匹配

```yaml
server:
  port: 7007

spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:            # 断言（判断条件）
            - Query=smile        # http://localhost:7007?simle=abc，只要请求中包含smile参数即可
server:
  port: 7007

spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - Query=smile,abc        # http://localhost:7007?simle=abc,请求中包含smile参数且smile的参数值是abc
```

- 示例：根据请求的IP地址进行匹配

```yaml
server:
  port: 7007

spring:
  application:
    name: api-gateway-server
  # 配置 Spring Cloud Gateway
  cloud:
    gateway:
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          uri: http://localhost:9004 # 路由到微服务的uri
          predicates:                # 断言（判断条件）
            - RemoteAddr=192.168.1.1/24
```



## P88、SpringCloudGateway路由：动态路由配置

##### 3.2.3 动态路由/面向服务的路由

和Zuul网关类似，在Spring Cloud Gateway中也支持动态路由：即自动从注册中心获取服务列表并访问。

**1、添加注册中心依赖**  引入依赖Eureka-client依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

**2、配置动态路由**  启动类中开启路由发现功能

```java
@EnableEurekaClient
@EnableDiscoveryClient
//两个注解二选一 也可以不写者两个注解
public class GatewayServerApplication{
    public static void manin(String[] args){
        SpringApplication.run(GatewayServerApplication.class,args);
    }
}
```

**3、添加注册中心信息**

```yaml
#eureka注册信息
eureka:
  client:
    service-url:
      defaultZone: http://localhost:9000/eureka/
  instance:
    prefer-ip-address: true  #使用ip地址注册
```

**4、在yml配置文件中 进行配置**

```yaml
spring:  
  cloud:
    gateway:
      routes:
        - id: product-service #路由id 保持唯一即可
          #uri: http://127.0.0.1:9001 #目标服务器IP地址
          uri: lb:service-product #lb://根据微服务名称从注册中心拉取服务请求路径
          predicates:
            - Path=/product/**  #路由条件 path：路由匹配条件
        
```

## P89、SpringCloudGateway路由：重写转发路径

##### 2.1.4 路径重写

在Spring Cloud Gateway中，路由转发是直接将匹配的路由Path直接拼接到映射路径URI之后，那么在微服务开发中往往并不方便。这里可以使用RewritePath机制来进行路径重写。

配置类似Zuul带前缀的访问规则

```yml
spring:  
  cloud:
    gateway:
      routes:
      - id: product-service #路由id
        uri: lb:service-product
        predicates:
        #- Path=/product/*
        - Path=/product-service/** #将当前请求转发到http://127.0.0.1:9001/product-service/product/1
        						#我们要实现 转发到  http://127.0.0.1:9001/product/1
        filters: # 配置一个路由过滤器
        - RewritePath=/product-service/(?<segment>.*),/$\{segment} #路径重写过滤器
         #将http://127.0.0.1:8080/product-service/product/1 转发到 http://127.0.0.1:9001/product/1
         # 将中间部分/product-service重写为空 
         # yml中$ 要携程$\ 进行转义
```

## P90、SpringCloudGateway路由：微服务名称转发

##### 2.1.5 开启微服务名称的转发规则

配置自动的根据微服务名称进行路由转发

Spring Cloud Gateway支持根据微服务名称进行自动转发。只需要修改application.yml配置文件即可（需要和Eureka整合）。

```yaml
spring:  
  cloud:
    gateway:
      routes:
        ...
      #配置自动根据微服务名称进行路由转发
      discovery:
        locator:
          enable: true #开启根据微服务名称自动转发
          lower-case-service-id: true #微服务名称以小写形式呈现
```

## P91、SpringCloudGateway过滤器：概述

#### 3.3 过滤器

Spring Cloud Gateway除了具备请求路由功能之外，也支持对请求的过滤。和Zuul网关类似，也是通过过滤器的形式来实现的。

##### 3.3.1 过滤器基础

**（1）过滤器的声明周期**

SpringCloudGateway过滤器没有Zuul那么丰富，它只有两个pre和post

- PRE：这种过滤器在请求被路由之前调用。我们可以利用这种过滤器实现身份认证、在集群中选择请求的微服务、记录调试信息等。
- POST：这种过滤器在路由到微服务以后执行。这种过滤器可以用来为响应添加标准的HTTP Header、收集统计信息和指标、将响应从微服务发送给客户端等等。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161041641-2109258876.png)

**（2）过滤器类型**

Spring Cloud Gateway的Filter从作用范围可以分为两种：GatewayFilter和GlobalFilter。

- GatewayFilter  针对某一个路由或者某一组路由有效的路由配置 

- GlobalFilter 全局路由配置 应用到所有路由上   如：身份认证，权限校验；GlobalFilter是重点

##### 3.3.2 局部过滤器

局部过滤器（GatewayFilter），是针对单个路由的过滤器。可以对访问的URL过滤，进行切面处理。在Spring Cloud Gateway中通过GatewayFilter的形式内置了很多不同类型的局部过滤器。

| 过滤器工厂                  | 作用                                                         | 参数                                                         |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| AddRequestHeader            | 为原始请求添加Header                                         | Header的名称及值                                             |
| AddRequestParameter         | 为原始请求添加请求参数                                       | 参数名称及值                                                 |
| AddResponseHeader           | 为原始响应添加Header                                         | Header的名称及值                                             |
| DedupeResponseHeader        | 剔除响应头中重复的值                                         | 需要去重的Header名 称及去重策略                              |
| Hystrix                     | 为路由引入Hystrix的断路器保护                                | HystrixCommand的名 称                                        |
| FallbackHeaders             | 为fallbackUri的请求头中添加具 体的异常信息                   | Header的名称                                                 |
| PrefixPath                  | 为原始请求路径添加前缀                                       | 前缀路径                                                     |
| PreserveHostHeader          | 为请求添加一个 preserveHostHeader=true的属 性，路由过滤器会检查该属性以 决定是否要发送原始的Host | 无                                                           |
| RequestRateLimiter          | 用于对请求限流，限流算法为令 牌桶                            | keyResolver、 rateLimiter、 statusCode、 denyEmptyKey、 emptyKeyStatus |
| RedirectTo                  | 将原始请求重定向到指定的URL                                  | http状态码及重定向的 url                                     |
| RemoveHopByHopHeadersFilter | 为原始请求删除IETF组织规定的 一系列Header                    | 默认就会启用，可以通 过配置指定仅删除哪些 Header             |
| RemoveRequestHeader         | 为原始请求删除某个Header                                     | Header名称                                                   |
| RemoveResponseHeader        | 为原始响应删除某个Header                                     | Header名称                                                   |
| RewritePath                 | 重写原始的请求路径                                           | 原始路径正则表达式以 及重写后路径的正则表 达式               |
| RewriteResponseHeader       | 重写原始响应中的某个Header                                   | Header名称，值的正 则表达式，重写后的值                      |
| SaveSession                 | 在转发请求之前，强制执行 WebSession::save操作                | 无                                                           |
| secureHeaders               | 为原始响应添加一系列起安全作 用的响应头                      | 无，支持修改这些安全 响应头的值                              |
| SetPath                     | 修改原始的请求路径                                           | 修改后的路径                                                 |
| SetResponseHeader           | 修改原始响应中某个Header的值                                 | Header名称，修改后 的值                                      |
| SetStatus                   | 修改原始响应的状态码                                         | HTTP 状态码，可以是 数字，也可以是字符串                     |
| StripPrefix                 | 用于截断原始请求的路径                                       | 使用数字表示要截断的 路径的数量                              |
| Retry                       | 针对不同的响应进行重试                                       | retries、statuses、 methods、series                          |
| RequestSize                 | 设置允许接收最大请求包的大 小。如果请求包大小超过设置的 值，则返回 413 Payload Too Large | 请求包大小，单位为字 节，默认值为5M                          |
| ModifyRequestBody           | 在转发请求之前修改原始请求体内容                             | 修改后的请求体内容                                           |
| ModifyResponseBody          | 修改原始响应体的内容                                         | 修改后的响应体内容                                           |

> 每个过滤器工厂都对应一个实体类，并且这些类的名称必须以GatewayFilterFactory结尾，这是Spring Cloud Gateway的一个约定，例如AddRequestHeader对一个的实体类为AddRequestHeaderGatewayFilterFactory。

## P92、SpringCloudGateway过滤器：自定义全局过滤器

##### 3.3.3 全局过滤器

全局过滤器（GlobalFilter）作用于所有路由，Spring Cloud Gateway定义了Global Filter接口，用户可以自定义实现自己的Global Filter。通过全局过滤器可以实现对权限的统一校验，安全性校验等功能，并且全局过滤器也是程序员使用比较多的过滤器。

Spring Cloud Gateway内部也是通过一些列的内置全局过滤器对整个路由转发进行处理，如下图所示

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161042478-1955282141.png)

##### 3.3.4 自定义全局过滤器

定义了GlobalFilter接口，用户实现GlobalFilter接口，就可以实现自定义的过滤器

```
filter.LoginFilter
#自定义全局过滤器
```

```java
/**
* 自定义一个全局过滤器
*	实现GlobalFilter, Ordered两个接口
*/
@Component
public class LoginFilter implements GlobalFilter, Ordered {

    /**
     * 执行过滤器中的业务逻辑
     * @param exchange
     * @param chain
     * @return
     */
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        System.out.println("执行了自定义的全局过滤器");
        return chain.filter(exchange);  //继续向下执行
    }

    /**
     * 指定过滤器的执行顺序 返回值越小，优先级越高
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }
}
```

## P93、SpringCloudGateway过滤器：认证过滤器

#### 3.4 统一鉴权

内置的过滤器已经可以完成大部分的功能，但是对于企业开发的一些业务功能处理，还是需要我们自己去编写过滤器来实现的，那么我们通过代码的形式自定义一个过滤器，去完成统一的权限校验。

##### 3.4.1 鉴权逻辑

- 开发中的鉴权逻辑：
- 1️⃣当客户端第一次请求服务的时候，服务端对用户进行信息认证（登录）。
- 2️⃣认证通过，将用户信息进行加密形成token，返回给客户端，作为登录凭证。
- 3️⃣以后每次请求，客户端都携带认证的token。
- 4️⃣服务daunt对token进行解密，判断是否有效。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161042716-1691638325.jpg)

如上图所示，对于验证用户是否已经登录授权的过程可以在网关层统一校验。校验的标准就是请求中是否携带token凭证以及token的正确性。

##### 3.4.2 代码实现

下面，我们自定义一个GlobalFilter，去校验所有请求的请求参数中是否包含“token”，如果不包含请求参数“token”则不转发路由，否则执行正常的业务逻辑。

```java
@Component
public class LoginFilter implements GlobalFilter, Ordered {

    /**
     * 执行过滤器中的业务逻辑
     *    对请求参数中的access-token进行判断
     *    如果存在此参数代表认证成功
     *    不存在这个参数 认证失败
     *
     * ServerWebExchange 相当于请求和响应的上下文 类似Zuul中的RequestContext 
     * @param exchange
     * @param chain
     * @return
     */
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        System.out.println("执行了自定义的全局过滤器");
        //1获取access-toker
        String token = exchange.getRequest().getQueryParams().getFirst("access-toen");      //请请参数
        
        //2判断是否存在
        if(token==null){
            //3 如果不存在认证失败
            System.out.println("没有登录");
            exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
            return exchange.getResponse().setComplete(); //请求结束
        }
        //4 如果存在则继续执行
        return chain.filter(exchange);  //继续向下执行
    }

    /**
     * 指定过滤器的执行顺序 返回值越小，优先级越高
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }
}

```

## P94、网关限流算法：计数器算法

#### 3.5 网关限流

##### 3.5.1 常见的限流算法

###### **1 计数器算法**

计数器限流算法是最简单的一种限流实现方式。

其本质是通过维护一个单位时间内的计数器，每次请求计数器+1，当单位时间内计数器累加到大于设定的阈值，则之后的请求都被拒绝，直到单位时间已经过去，再将计数器重置为零。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161042946-2072001940.png)

单位时间：每分钟  阈值：单位时间内最大的请求数量

计数器算法缺点：

- 流量限制不够平滑  

## P95、网关限流算法：漏桶算法

###### **2 漏桶算法**

漏桶算法可以很好的限制容量池的大小，从而防止流量暴增。

漏桶可以看作是一个带有常量服务时间的单服务器队列，如果漏桶（包缓存）溢出，那么数据包会被丢弃。

在网络中，漏桶算法可以控制端口的流量输出速率，平滑网络上的突发流量，实现流量整形，从而为网络提供一个稳定的流量。

[![漏桶算法](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161043231-1253106548.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161043231-1253106548.png)

为了更好的控制流量，漏桶算法需要通过两个变量进行控制：一个是桶的大小，支持流量突发增加时可以存多少水(burst)，另一个是桶漏洞的大小(rate)。

相当于在网关侧维护一块内存，可以理解为一个队列，请求都发送到网关，网关到微服务 可以设置队列的输出频率

漏桶溢出，那么请求数据包就会丢失

队列大小=漏桶的容量

输出频率

如果设置不当，可能导致网关宕机，**漏桶算法主要是为了保护别人**的服务（后端的微服务）

## P96、网关限流算法：令牌桶算法

###### **3 令牌桶算法**

令牌桶算法是对漏桶算法的一种改进，漏桶算法能够限制请求调用的速率，而令牌桶算法能够在限制调用的平均速率的同时还允许一定程序的突发调用。

- 在令牌桶算法中，存在一个桶，用来存放固定数量的令牌。算法中存在一种机制，以一定速率往桶中放令牌。每次请求调用需要先获取令牌，只有拿到令牌，才有机会继续执行，否则选择等待可用的令牌或者直接拒绝。放令牌这个动作是持续不断的进行，如果桶中令牌数量达到上限，就丢弃令牌，所以就存在这种情况，桶中一直有大量的可用令牌，这时进来的请求就可以直接拿到令牌执行，比如设置QPS为100，那么限流器初始化完成1秒后，桶中就已经有100个令牌了，这时服务可能还没完全启动好，等启动完成对外提供服务时，该限流器可以抵挡瞬时的100个请求。所以，只有桶中没有令牌时，请求才会进行等待，最后相当于以一定速率执行。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161043454-1750790529.png)

请求必须拿到令牌才可以访问，如果拿不到令牌或者令牌桶中没有可用的令牌，要么等待，要么访问被拒绝

请求过来之后，必须先拿到令牌，获取到了令牌

**令牌桶算法主要是为了保护自己**，网关挂了，所有请求都进不了，所有微服务中通常使用令牌桶算法



## P97、SCG网关：filter限流-上

##### 3.5.2 基于filter的限流

cloud官方提供了令牌桶算法进行限流支持。基于其内置的过滤器工厂org.springframework.cloud.gateway.filter.factory.RequestRateLimiterGatewayFilterFacory实现。在过滤器工厂中是通过Redis和Lua脚本结合的方式进行流量控制。

**1、环境搭建**

准备工作：准备Redis

```
monitor 命令监控redis 数据变化
```

工程中引入redis相应的依赖，注意：是基于Reactive的Redis依赖，而非spring-data-redis依赖

```xml
    <!-- 监控依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <!-- Redis依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
```

**2、修改网关中的application.yml配置**

```yaml
server:
  port: 8080 #端口号
spring:
  application:
    name: api-gateway-server #服务名称
  redis:
    host: localhost
    poll: 6379
    database: 0

  # 配置springCloudgateway的路由
  cloud:
    gateway:
      routes:
      - id: product-service #路由id 保持唯一即可
        uri: http://127.0.0.1:9001 #目标服务器IP地址
        predicates:
        - Path=/product/*  #路由条件 path：路由匹配条件

        filters:
        - name: RequestRateLimiter #使用限流过滤器，是springcloud gateway 提供的
          args:
            # 使用SpEL从容器中获取对象
            key-resolver: '#{@pathKeyResolver}'
            # 令牌桶每秒填充的评价速率
            redis-rate-limiter.replenishRate: 1
            # 令牌桶的上限
            redis-rate-limiter.burstCapacity: 3
        - RewritePath=/product-service/(?<segment>.*),/$\{segment}
eureka:
  client:
    service-url:
      defaultZone: http://localhost:9000/eureka
    instance:
      prefer-ip-address: true #使用ip地址注册
```

## P98、SCG网关：filter限流-中

**3、配置一个redis 中key的解析器KeyResolver**

 KeyResolverConfiguration类中 必须有 pathKeyResolver方法，因为配置类中指明的是这个方法 需要对应起来

```java
@Configuration
public class KeyResolverConfiguration{
    //编写基于请求路径的限流规则 /abc
	//基于ip127.0.0.1
    //基于参数
    @Bean
    public KeyResolver pathKeyResolver() {
        //自定义的KeyResolver
        return new KeyResolver(){
            @Override
            public Mono<String> resolve(ServerWebExchange exchange){
                return Mono.just(
                     exchange.getRueqest().getPath().toString();
                )
            }
        }
    }
}

//基于路径 /abc这个路径 最大只能放3个  redis-rate-limiter.burstCapacity: 3
```



```java
package com.beyondsoft.gateway.config;

import org.springframework.cloud.gateway.filter.ratelimit.KeyResolver;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import reactor.core.publisher.Mono;

/**
 * 配置Redis中Key的解析器
 *
 * @author 许大仙
 * @version 1.0
 * @since 2020-10-22 09:53
 */
@Configuration
public class KeyResolverConfig {

    /**
     * 基于请求路径的限流
     * 请求/abc/1
     * @return
     */
    //@Bean
    public KeyResolver pathKeyResolver() {
        return exchange -> Mono.just(exchange.getRequest().getPath().toString());
    }

    /**
     * 基于请求参数的限流
     * 请求/abc?userId=1
     *
     * @return
     */
    //@Bean
    public KeyResolver useKeyResolver() {
        return exchange -> Mono.just(exchange.getRequest().getQueryParams().getFirst("userId"));
    }

    /**
     * 基于请求IP地址的限流
     *
     * @return
     */
    @Bean
    public KeyResolver ipKeyResolver() {
        return exchange -> Mono.just(exchange.getRequest().getHeaders().getFirst("x-Forwarded-For"));
    }
}
```

## P98、SCG网关：filter限流-下

基于参数的限流

```java
    //基于参数的
    //请求/abc?userId=1
    //@Bean
    public KeyResolver useKeyResolver() {
        return exchange -> Mono.just(exchange.getRequest().getQueryParams().getFirst("userId"));
    }
```

基于请求ip的限流

```java
    /**
     * 基于请求IP地址的限流
     *
     * @return
     */
    @Bean
    public KeyResolver ipKeyResolver() {
        return exchange -> Mono.just(exchange.getRequest().getHeaders().getFirst("x-Forwarded-For"));
    }
```

## P100、SCG网关中使用sentinel限流：入门案例

##### 3.5.3 基于Sentinel的限流

Sentinel 支持对 Spring Cloud Gateway、Zuul 等主流的 API Gateway 进行限流。

[![Sentinel支持网关限流](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161043694-241326761.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161043694-241326761.png)

从1.6.0版本卡死，Sentinel提供了Spring Cloud Gateway的适配模块，可以提供两种资源维度的限流

- Route维度：即在Spring配置文件中配置的路由条码，资源名对应的routeId
- 自定义API维度：用户可以利用Sentinel提供的API来自定义一些API分组

Sentinel1.6.0 引入了Sentinel API Gateway Adapter Common模块，此模块中包含网关限流的规则和自定义API的实体类和管理逻辑：

- GatewayFlowRule : 网关限流规则，针对API Gateway的场景定制的限流规则，可以针对不同route或自定义的API分组进行限流，支持针对请求中的参数，Header，来源IP等进行定制化的限流。
- ApiDefinition：用户自定义的API分组，可以看做是一些URL匹配的组合，比如我们可以定义一个API叫my_api，请求path模式为/foo/** 和/baz/**  的都归到my_apu这个API分组下面，限流的时候可以针对这个自定义的API分组维度进行限流。

##### (1) 环境搭建

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-spring-cloud-gateway-adapter</artifactId>
    <version>1.6.3</version>
</dependency>
```

##### (2) 编写配置类

```java
package com.beyondsoft.gateway.config;

import com.alibaba.csp.sentinel.adapter.gateway.common.SentinelGatewayConstants;
import com.alibaba.csp.sentinel.adapter.gateway.common.api.ApiDefinition;
import com.alibaba.csp.sentinel.adapter.gateway.common.api.ApiPathPredicateItem;
import com.alibaba.csp.sentinel.adapter.gateway.common.api.ApiPredicateItem;
import com.alibaba.csp.sentinel.adapter.gateway.common.api.GatewayApiDefinitionManager;
import com.alibaba.csp.sentinel.adapter.gateway.common.rule.GatewayFlowRule;
import com.alibaba.csp.sentinel.adapter.gateway.common.rule.GatewayRuleManager;
import com.alibaba.csp.sentinel.adapter.gateway.sc.SentinelGatewayFilter;
import com.alibaba.csp.sentinel.adapter.gateway.sc.callback.BlockRequestHandler;
import com.alibaba.csp.sentinel.adapter.gateway.sc.callback.GatewayCallbackManager;
import com.alibaba.csp.sentinel.adapter.gateway.sc.exception.SentinelGatewayBlockExceptionHandler;
import org.springframework.beans.factory.ObjectProvider;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.Ordered;
import org.springframework.core.annotation.Order;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.codec.ServerCodecConfigurer;
import org.springframework.web.reactive.function.BodyInserters;
import org.springframework.web.reactive.function.server.ServerResponse;
import org.springframework.web.reactive.result.view.ViewResolver;

import javax.annotation.PostConstruct;
import java.util.*;

/**
 * Sentinel限流的配置
 */
@Configuration
public class GatewayConfig {

    private final List<ViewResolver> viewResolvers;

    private final ServerCodecConfigurer serverCodecConfigurer;

    //构造方法
    public GatewayConfig(ObjectProvider<List<ViewResolver>> viewResolversProvider,
                         ServerCodecConfigurer serverCodecConfigurer) {
        this.viewResolvers = viewResolversProvider.getIfAvailable(Collections::emptyList);
        this.serverCodecConfigurer = serverCodecConfigurer;
    }

    /**
     * 配置限流的异常处理器:SentinelGatewayBlockExceptionHandler
     */
    @Bean
    @Order(Ordered.HIGHEST_PRECEDENCE)
    public SentinelGatewayBlockExceptionHandler sentinelGatewayBlockExceptionHandler() {
        return new SentinelGatewayBlockExceptionHandler(this.viewResolvers, this.serverCodecConfigurer);
    }

    /**
     * 配置限流过滤器
     */
    @Bean
    @Order(Ordered.HIGHEST_PRECEDENCE)
    public GlobalFilter sentinelGatewayFilter() {
        return new SentinelGatewayFilter();
    }

    /**
     * 配置初始化的限流参数
     * 用于指定资源的限流规则：
     * 1.资源名称（路由id）
     * 2.配置统计时间窗口
     * 3.配置限流阈值
     */
    @PostConstruct  //对象完成创建之后，立即执行的初始化方法 用于指定资源的限流规则
    public void initGatewayRules() {
        Set<GatewayFlowRule> rules = new HashSet<>();
        rules.add(new GatewayFlowRule("product-service") //资源名称
                .setCount(1) // 限流阈值
                .setIntervalSec(1) // 统计时间窗口，单位是秒，默认是 1 秒
        );
        //配置自定义API分组
        rules.add(new GatewayFlowRule("product_api").setCount(1).setIntervalSec(1));
        rules.add(new GatewayFlowRule("order_api").setCount(1).setIntervalSec(1));
        GatewayRuleManager.loadRules(rules);
    }

    /**
     * 自定义异常提示
     */
    @PostConstruct
    public void initBlockHandlers() {
        BlockRequestHandler blockRequestHandler = (serverWebExchange, throwable) -> {
            Map<String, Object> map = new HashMap<>();
            map.put("code", "001");
            map.put("message", "对不起,接口限流了");
            return ServerResponse
                    .status(HttpStatus.OK)
                    .contentType(MediaType.APPLICATION_JSON)
                    .body(BodyInserters.fromValue(map));
        };
        GatewayCallbackManager.setBlockHandler(blockRequestHandler);
    }

    /**
     * 自定义API限流分组
     * 1.定义分组
     * 2.对小组配置限流规则
     */
    @PostConstruct
    private void initCustomizedApis() {
        Set<ApiDefinition> definitions = new HashSet<>();
        ApiDefinition api1 = new ApiDefinition("product_api")
                .setPredicateItems(new HashSet<ApiPredicateItem>() {{
                    this.add(new ApiPathPredicateItem().setPattern("/product-service/product/**").
                            setMatchStrategy(SentinelGatewayConstants.URL_MATCH_STRATEGY_PREFIX));
                }});
        ApiDefinition api2 = new ApiDefinition("order_api")
                .setPredicateItems(new HashSet<ApiPredicateItem>() {{
                    this.add(new ApiPathPredicateItem().setPattern("/order-service/order"));
                }});
        definitions.add(api1);
        definitions.add(api2);
        GatewayApiDefinitionManager.loadApiDefinitions(definitions);
    }
}
```

- 基于Sentinel的Gateway限流是通过其提供的Filter来完成的，使用时只需要注入对应的SentinelGatewyFilter实例以及SentinelGatewayBlockExceptionHandler实例即可。
- @PostConstruct定义初始化的加载方法，用于指定资源的限流规则。这里资源的名称为order-service，统计时间是1秒内，限流阈值时1,。表示美标只能访问一个请求。

##### (3) 网关配置

```yml
spring:
  application:
    name: api-gateway-server
  redis:
    host: 127.0.0.1
    port: 6379
    database: 0    
  #配置 Spring Cloud Gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true # 开启从注册中心动态创建路由的功能，利用微服务名进行路由
          lower-case-service-id: true # 微服务名称以小写形式呈现
      routes:
        # 配置路由： 路由id，路由到微服务的uri,断言（判断条件）
        - id: product-service    # 路由id
          # uri: http://localhost:9004
          uri: lb://service-product # 路由到微服务的uri。 lb://xxx，lb代表从注册中心获取服务列表，xxx代表需要转发的微服务的名称
          predicates: # 断言（判断条件）
            #  - Path=/product/**
            - Path=/product-service/**
          filters: # 配置路由过滤器
            - RewritePath=/product-service/(?<segment>.*), /$\{segment} # 路径重写的过滤器
```

1秒内访问多次http://localhost:8080/order-service/order/buy/1就可以看到限流起作用了



## P101、SCG网关中使用sentinel限流：限流异常提示

##### (4) 自定义错误提示

当触发限流后 页面显示的是：Blocked by Sentinel:FlowException。 为了展示更加优化的限流提示，Sentinel支持自定义异常处理。

可以在GatewayCallbackManger注册回调进行定制：

- setBlockHandler：注册函数用于实现自定义的逻辑处理被限流的请求，对应接口为BlockRequestHandler。默认实现为DefaultBlockRequestHandler，当被限流是会返回类似下面的信息：Blocked by Sentinel:FlowException

```java

@PostConstruct
public void initBlockHandlers(){
    BlockRequestHandler blockHandler = new BlockRequestHandler(){
        @Override
        public Mono<ServerResponse> handleRequest(ServerWebExchange serverWebExchange, Throwable throwable){
            Map<> map = new HashMap();
            map.put("code","001");
            map.put("msg","不好意思 限流了");
            return ServerResponse.status(HttpStatus.OK).contentType(MediaType.APPLICATION_JSON_UTF8).body(BodyInserts.fromObject(map))
        }
    }
    GatewayCallbackManger.setBlockHandler();
}
```

## P102、SCG网关中使用sentinel限流：自定义分组限流

以上Sentinel的限流 是针对路由的限流规则，当前路由下，或当前请求的微服务下，所有的请求都添加上了相应的限流规则，这样不是特别灵活

除此之外 Sentinel还支持用户自定义限流分组

```java
    //自定义API分组
    /**
        1 定义分组
        2 对校长配置限流规则
    */
    @PostConstruct
    private void initCustomizedApis(){
        Set<ApiDefinition> definitions = new HashSet<>();
        ApiDefinition api1 = new ApiDefinition("product_api")
                .setPredicateItems(new HashSet<ApiPredicateItem>(){{
                    add(new ApiPathPredicateItem().setPattern("/product-service/product/**") //以/product-service/product/开头的所有url
                            .setMatchStrategy(SentinelGatewayConstants.URL_MATCH_STRATEGY_PREFIX));
                }});
        ApiDefinition api2 = new ApiDefinition("order_api")
                .setPredicateItems(new HashSet<ApiPredicateItem>(){{
                    add(new ApiPathPredicateItem().setPattern("/order-service/order"));  //完全匹配order-service/order 的url
                }});
        definitions.add(api1);
        definitions.add(api2);
        GatewayApiDefinitionManager.loadApiDefinitions(definitions);
    }
```
配置每个小组的限流规则
```java
  //配置初始化的限流参数
    @PostConstruct
    public void initGatewayRules(){
        Set<GatewayFlowRule> rules = new HashSet<>();
        //配置每个小组的限流规则
        rules.add(new GatewayFlowRule("product_api").setCount(1).setIntervalSec(1)); //没秒只能接收一个请求
        rules.add(new GatewayFlowRule("order_api").setCount(1).setIntervalSec(1));
        GatewayRuleManager.loadRules(rules);
    }
```

## P103、SCG网关高可用：概述

#### 3.6 网关的高可用

**高可用HA（High Availability）**是分布式系统架构设计中必须考虑的因素之一，它通常是指，通过设计减少系统不能提供服务的时间。我们都知道，单点是系统高可用的大敌，单点往往是系统高可用的最大的风险和敌人，应该尽量在系统设计的过程中避免单点。

方法论上，高可用保证的原则是“集群化”，或者叫做“冗余”。只有一个单点，挂了服务会受影响；如果有冗余备份，挂了还有其他备份能够顶上。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161043935-916979919.jpg)

实际使用Spring Cloud Gateway的方式如上图所示，不同的客户端使用不同的负载将请求分发到后端的Gateway，Gateway再通过HTTP调用后端服务，最后对外输出。因此为了保证Gateway的高可用性，前端可以同时启动多个Gateway实例进行负载，在Gateway的前端使用Nginx或者F5进行负载转发以达到高可用性。

## P104、SCG网关高可用：ngnix结合网关集群构造高可用网关

(1) 准备多个Gateway工程

(2) 配置Nginx

```
#集群配置
upstream gateway {
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}

server {
    listen 80;
    server_name localhost;
    location / {
        proxy_pass http://gateway;
    }
}
```



## P105、链路追踪：概述

### 4 微服务的链路追踪

#### 4.1 微服务架构下的问题

在大型系统的微服务化构建中，一个系统会被拆分成许多模块。这些模块负责不同的功能，组合成系统，最终可以提供丰富的功能。在这种架构中，一次请求往往需要涉及到多个服务。互联网应用构建在不同的软件模块集上，这些软件模块，可能是由不同团队开发、可能使用不同的编程语言来实现、可能部署在几千台服务器上，横跨多个不同的数据中心，也就意味着这种架构形式也会存在一些问题：

- 1️⃣如何快速的发现问题？
- 2️⃣如何判断故障影响范围？
- 3️⃣如何梳理服务依赖以及依赖的合理性？
- 4️⃣如何分析链路性能问题以及实时容量规划？

分布式链路追踪（Distributed Tracing），就是将一次分布式请求还原成调用链路，进行日志记录，性能监控并将一次分布式请求的调用情况集中展示。比如各个服务节点上的耗时、请求具体到达那台机器上、每个服务节点的请求状态等等。

目前业界比较流行的链路追踪系统如：Twitter的**Zipkin**，阿里的**鹰眼**，美团的**Mtrace**，大众点评的**cat**等，大部分都是基于Google发表的Dapper。Dapper阐述了分布式系统，特别是微服务架构中链路追踪的概念、数据展示、埋点、传递、收集、存储和展示等技术细节。

通常使用 **Sleuth + Zipkin**相结合使用

#### 4.2 Sleuth 概述

##### 4.2.1 简介

Spring Cloud Sleuth 主要功能是在分布式系统中提供追踪解决方案，并且兼容支持了zipkin，只需要在pom文件中引入相应的依赖即可。

##### 4.2.2 相关概念

Spring Cloud Sleuth为Spring Cloud提供了分布式追踪解决方案。它大量借用了Google的Dapper的设计。需要先了解一下Sleuth中的术语和相关概念。

Spring Cloud Sleuth采用的是Google的开源项目Dapper的专业术语。

- **Span**：基本工作单元，例如：在一个新建的span中发送一个RPC等同于发送一次回应请求给RPC，span通过一个64位ID唯一标识，trace以另一个64位ID标识，span还有其他数据信息，比如摘要、时间戳事件、关键值注释（tags）、span的ID以及进度ID（通常是IP地址），span在不断的启动和停止的同时记录了时间信息，当你创建了一个span，你必须在未来的某个时刻停止它。
- **Trace**：一系列span组成的一个树状结构。例如，当你正在跑一个分布式大数据工程，你可能需要创建Trace。
- Annotation：用来及时记录一个事件的存在，一些核心annotations用来定义一个请求的开始和结束。
  - cs-Client Server：客户端发送一个请求，这个annotation描述了这个span的开始。
  - sr-Server Received：服务端获得请求并准备开始处理它，如果将其srj减去cs时间戳便可得到网络延迟。
  - ss-Server Sent：注解表明请求处理的完成（当请求返回客户端），如果ss减去sr时间戳便可得到服务端需要的处理请求时间。
  - cr-Client Received：表明span的结束，客户端成功接收到服务端的回复，如果cr减去cs时间戳便可得到客户端从服务端获取回复的所有所需时间。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161149773-197855166.jpg)

**Trace: 整个调用链路；Span：每个最小的工作单元(一次远程微服务调用)；多个远程调用组成了完整的调用链路**



## P106、链路追踪：sleuth入门

#### 4.3 链路追踪Sleuth入门

接下来通过之前的项目案例 网关调用 order-service；order-service 调用 product-service 完成入门案例

##### 4.3.1 配置依赖

修改微服务工程，引入Sleuth依赖；每一个微服务都需要添加( 网关微服务、订单微服务、商品微服务)

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
```

##### 4.3.2 修改配置文件

修改application.yml的日志级别

```yml
logging:
  level:
    root: INFO
    org.springframework.web.servlet.DispatcherServlet: DEBUG
    org.springframework.cloud.sleuth: DEBUG
```

重启网关层、订单微服务、商品微服务重启之后，我们可以在控制台观察到Sleuth的日志输出。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161150008-1345243283.png)

其中，`81a807d076c2a9f6`是TraceId，后面跟着的是SpanId，依次调用有一个全局的TranceId，将调用链路串起来。仔细分析每个微服务的日志，不难看出请求的具体过程。

查看日志文件并不是一个很好的方法，当微服务越来越多日志文件也会越来越多，通过ZipKin可以将日志聚合，并进行可视化展示和全文检索。



## P107、链路追踪：zipkin概述

#### 4.4 Zipin的概述

Zipkin是Twitter的一个开源项目，它基于Google Dapper实现，它致力于收集服务的定时数据，以解决微服务架构中的延迟问题，包括数据的收集、存储、查找和展现。我们可以使用它来收集各个服务器上请求链路的跟踪数据，并通过它提供的REST API接口来辅助我们查询跟踪以实现分布式系统的监控程序，从而及时的发现系统中出现的延迟升高的问题并找出系统性能瓶颈的根源。除了面向开发的API接口之外，它也提供了方便的UI组件来帮助我们直观的搜索跟踪信息和分析请求链路明细，比如：可以查询某段时间内各用户请求的处理时间等。Zipkin提供了可插拔数据存储方式：In-Memory、MySQL、Cassandra以及ElasticSearch。

![zipkin](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161150288-1342978748.jpg)

上图展示了Zipkin的基础架构，它主要由4个核心组件构成：

- `Collector`：收集器组件，它主要用于处理从外部系统发送过来的跟踪信息，将这些信息转换为Zipkin内部处理的span格式，以支持后续的存储、分析、展示等功能。
- `Storage`：存储组件，它主要对处理收集器接收到的跟踪信息，默认会将这些信息存储在内存中，我们也可以修改此存储策略，将跟踪信息存储到数据库中。
- `Restful API`：API组件，它主要用来提供外部访问接口。比如给客户端展示跟踪信息，或者外接系统访问以实现监控等。
- `Web UI`：UI组件，基于API组件实现的上层应用。通过UI组件用户可以方便而直观的查询和分析跟踪信息。

Zipkin分为两端，一个是Zipkin服务端，一个是Zipkin客户端，客户端也就是微服务的应用。客户端会配置服务端的URL地址，一旦发生服务间的调用时候，会被配置在微服务里面的Sleuth的监听器监听，并生成相应的Trace和Span信息发送给服务端。

发送的方式有两种：一种是HTTP报文的方式，另外一种是消息总线的方式如RabbitMQ。

不论哪种方式，我们都需要：

- 一个Eureka服务注册中心。
- 一个Zipkin服务端。
- 多个微服务，这些微服务中配置了Zipkin客户端。



## P108、day3-36-链路追踪：zipkinServer的安装和启动

#### 4.5 Zipin Server的部署和配置

##### 4.5.1 Zipin Server下载

从SpringBoot2.0开始，官方就不再支持使用自建Zipkin Server的方式进行服务链路追踪，而是直接提供了编译好的jar包来给我们使用。可以从官方网站上下载[Zipkin的Web UI](https://repo1.maven.org/maven2/io/zipkin/java/zipkin-server/2.12.9/zipkin-server-2.12.9-exec.jar)。我们这里下载的是zipkin-server-2.12.9-exec.jar

##### 4.5.2 启动

在命令行输入`java -jar zipkin-server-2.12.9-exec.jar`启动Zipkin Server。

```bash
java -jar zipkin-server-2.12.9-exec.jar
```
![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161150449-39609125.png)

- 默认Zipkin Server的请求端口是9411.
- Zippin Server的启动参数，可以通过参考官方提供的[yml配置文件查找](https://github.com/openzipkin/zipkin/blob/master/zipkin-server/src/main/resources/zipkin-server-shared.yml) 
- 在浏览器输入http://localhost:9411即可进入到Zipkin Server的管理后台。

#### 4.5 使用Docker启动Zipkin Server

```shell
docker run -d -p 9411:9411 --name zipkin  openzipkin/zipkin:2.12.9
```

## P109、链路追踪：zipkin整合sleuth展示调用链路

#### 4.6 客户端Zipin+Sleuth整合

以下配置在网关微服务、订单微服务、商品微服务都需要配置

##### 4.6.1 添加zipkin客户端依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

##### 4.6.2 修改配置文件

```yml
spring:  
  # zipkin配置
  zipkin:
    base-url: http://192.168.217.100:9411/ # Zipkin Server端的请求地址
    sender:
      type: web # 数据的传输方式，以HTTP的形式向Zipkin Server端发送数据
  sleuth:
    sampler:
      probability: 1 # 采样比  默认为0.1，即10%，这里配置1，是记录全部的sleuth信息，是为了收集到更多的数据（仅供测试使用）
```

##### 4.6.3 测试

分别重启网关层、订单微服务和商品微服务，通过浏览器发送一次微服务请求。打开Zipkin的Web UI控制台，我们可以根据条件追踪每次请求调用过程。

[![Zipkin的WebUI中看到的监控信息](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161150636-1863082934.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161150636-1863082934.png)

点击该trace可以看到请求的细节：

[![击该trace可以看到请求的细节](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161150800-645806415.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161150800-645806415.png)

## P110、链路追踪：zipkin整合sleuth的执行过程和存在的问题分析

[![分析Zipkin整合Sleuth的问题](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161151032-1691822424.jpg)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161151032-1691822424.jpg)

由上图可知：

- 1️⃣链路数据如何持久化保存（当前数据保存在Zipkin服务端的内存中，断电易丢失）。
- 2️⃣如何优化数据采集过程（HTTP方式是同步阻塞方式，一旦出现网络波动等情况，可能会波及业务系统）。



## P111、链路追踪：zipkin服务端数据保存mysql数据库

#### 4.8 存储跟踪数据

Zipkin Server默认是将追踪数据信息保存到内存中，这种方式不适合生产环境。因为一旦Zipkin Server端关闭重启或者服务崩溃，就会导致历史数据小时。Zipkin支持将追踪数据持久化到MySQL数据库中或者存储到ElasticSearch中。

##### 4.8.1 准备数据库

先创建一个zipkin数据库，数据库表结构可以从官网中找到Zipkin Server持久化的MySQL数据库脚本：

```sql
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for zipkin_annotations
-- ----------------------------
DROP TABLE IF EXISTS `zipkin_annotations`;
CREATE TABLE `zipkin_annotations`  (
  `trace_id_high` bigint(20) NOT NULL DEFAULT 0 COMMENT 'If non zero, this means the trace uses 128 bit traceIds instead of 64 bit',
  `trace_id` bigint(20) NOT NULL COMMENT 'coincides with zipkin_spans.trace_id',
  `span_id` bigint(20) NOT NULL COMMENT 'coincides with zipkin_spans.id',
  `a_key` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'BinaryAnnotation.key or Annotation.value if type == -1',
  `a_value` blob NULL COMMENT 'BinaryAnnotation.value(), which must be smaller than 64KB',
  `a_type` int(11) NOT NULL COMMENT 'BinaryAnnotation.type() or -1 if Annotation',
  `a_timestamp` bigint(20) NULL DEFAULT NULL COMMENT 'Used to implement TTL; Annotation.timestamp or zipkin_spans.timestamp',
  `endpoint_ipv4` int(11) NULL DEFAULT NULL COMMENT 'Null when Binary/Annotation.endpoint is null',
  `endpoint_ipv6` binary(16) NULL DEFAULT NULL COMMENT 'Null when Binary/Annotation.endpoint is null, or no IPv6 address',
  `endpoint_port` smallint(6) NULL DEFAULT NULL COMMENT 'Null when Binary/Annotation.endpoint is null',
  `endpoint_service_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT 'Null when Binary/Annotation.endpoint is null',
  UNIQUE INDEX `trace_id_high`(`trace_id_high`, `trace_id`, `span_id`, `a_key`, `a_timestamp`) USING BTREE COMMENT 'Ignore insert on duplicate',
  INDEX `trace_id_high_2`(`trace_id_high`, `trace_id`, `span_id`) USING BTREE COMMENT 'for joining with zipkin_spans',
  INDEX `trace_id_high_3`(`trace_id_high`, `trace_id`) USING BTREE COMMENT 'for getTraces/ByIds',
  INDEX `endpoint_service_name`(`endpoint_service_name`) USING BTREE COMMENT 'for getTraces and getServiceNames',
  INDEX `a_type`(`a_type`) USING BTREE COMMENT 'for getTraces and autocomplete values',
  INDEX `a_key`(`a_key`) USING BTREE COMMENT 'for getTraces and autocomplete values',
  INDEX `trace_id`(`trace_id`, `span_id`, `a_key`) USING BTREE COMMENT 'for dependencies job'
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = Compressed;

-- ----------------------------
-- Table structure for zipkin_dependencies
-- ----------------------------
DROP TABLE IF EXISTS `zipkin_dependencies`;
CREATE TABLE `zipkin_dependencies`  (
  `day` date NOT NULL,
  `parent` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL,
  `child` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL,
  `call_count` bigint(20) NULL DEFAULT NULL,
  `error_count` bigint(20) NULL DEFAULT NULL,
  PRIMARY KEY (`day`, `parent`, `child`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = Compressed;

-- ----------------------------
-- Table structure for zipkin_spans
-- ----------------------------
DROP TABLE IF EXISTS `zipkin_spans`;
CREATE TABLE `zipkin_spans`  (
  `trace_id_high` bigint(20) NOT NULL DEFAULT 0 COMMENT 'If non zero, this means the trace uses 128 bit traceIds instead of 64 bit',
  `trace_id` bigint(20) NOT NULL,
  `id` bigint(20) NOT NULL,
  `name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL,
  `remote_service_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `parent_id` bigint(20) NULL DEFAULT NULL,
  `debug` bit(1) NULL DEFAULT NULL,
  `start_ts` bigint(20) NULL DEFAULT NULL COMMENT 'Span.timestamp(): epoch micros used for endTs query and to implement TTL',
  `duration` bigint(20) NULL DEFAULT NULL COMMENT 'Span.duration(): micros used for minDuration and maxDuration query',
  PRIMARY KEY (`trace_id_high`, `trace_id`, `id`) USING BTREE,
  INDEX `trace_id_high`(`trace_id_high`, `trace_id`) USING BTREE COMMENT 'for getTracesByIds',
  INDEX `name`(`name`) USING BTREE COMMENT 'for getTraces and getSpanNames',
  INDEX `remote_service_name`(`remote_service_name`) USING BTREE COMMENT 'for getTraces and getRemoteServiceNames',
  INDEX `start_ts`(`start_ts`) USING BTREE COMMENT 'for getTraces ordering and range'
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = Compressed;

SET FOREIGN_KEY_CHECKS = 1;
```

##### 4.8.2 配置启动服务端

```bash
java -jar zipkin-server-2.12.9-exec.jar --STORAGE_TYPE=mysql --MYSQL_HOST=127.0.0.1 --MYSQL_TCP_PORT=3306 --MYSQL_DB=zipkin --MYSQL_USER=root --MYSQL_PASS=123456
## 参数详解：
# STORAGE_TYPE 存储类型    
# MYSQL_HOST MySQL主机地址
# MYSQL_TCP_PORT MySQL端口
# MYSQL_DB MySQL数据库名称
# MYSQL_USER MySQL用户名
# MYSQL_PASS MySQL地址
```

##### 4.8.3 Docker方式配置启动服务端

```bash
docker run -d \
-p 9411:9411 \
--restart always \
-v /etc/localtime:/etc/localtime:ro \
-e MYSQL_USER=root \
-e MYSQL_PASS=123456 \
-e MYSQL_HOST=192.168.217.100 \
-e STORAGE_TYPE=mysql \
-e MYSQL_DB=zipkin \
-e MYSQL_TCP_PORT=3306 \
--name zipkin \
openzipkin/zipkin:2.12.9
```

##### 4.8.4 测试

配置好服务daunt之后，可以在浏览器中请求几次。回到数据库查看，会发现数据已经持久化到MySQL中了。

[![Zipkin数据持久化到MySQL中](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161151207-1065750424.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161151207-1065750424.png)

## P112、day3-40-链路追踪：zipkin通过消息中间件进行数据收集的思路分析

#### 4.7 基于消息中间件收集数据

##### 4.7.1 概述

- 在默认情况下，Zipkin客户端和Zipkin服务端之间是使用HTTP请求的方式进行通信（即同步的请求方式）。
- 在网络波动，Zipkin服务端异常等情况下，可能会存在信息收集不机制的问题。
- Zipkin支持和RabbitMQ整合完整异步消息传输。

[![基于消息中间件收集数据](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161151480-934626227.jpg)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161151480-934626227.jpg)

##### 4.7.2 步骤

- 1️⃣准备RabbitMQ中间件。
- 2️⃣修改Zipkin客户端，将消息发送到MQ服务器。
- 3️⃣修改Zipkin服务端，从MQ中拉取消息。

## P113、day3-41-链路追踪：zipkin服务端连接rabbitmq

##### 4.7.3 RabbitMQ的安装和启动

以Docker形式安装和启动RabbitMQ：

```shell
docker run -d  --name rabbit -p 15672:15672 -p 5672:5672 rabbitmq:management
```

##### 4.7.4 命令行方式配置启动服务端

修改Zipkin服务端的从MQ中拉取消息

停掉服务，修改启动参数，并重新启动。

```shell
java -jar zipkin-server-2.12.9-exec.jar --STORAGE_TYPE=mysql --MYSQL_HOST=192.168.217.100 --MYSQL_TCP_PORT=3306 --MYSQL_DB=zipkin --MYSQL_USER=root --MYSQL_PASS=123456 --RABBIT_ADDRESSES=192.168.217.100:5672 --RABBIT_USER=guest --RABBIT_PASSWORD=guest
## 启动参数详解：
# STORAGE_TYPE 存储类型    
# MYSQL_HOST MySQL主机地址
# MYSQL_TCP_PORT MySQL端口
# MYSQL_DB MySQL数据库名称
# MYSQL_USER MySQL用户名
# MYSQL_PASS MySQL地址
# RABBIT_ADDRESSES 指定Rabbitmq的地址
# RABBIT_USER 用户名（默认为guest）
# RABBIT_PASSWORD 密码（默认为guest）
```

##### 4.7.5 Docker方式启动配置启动服务端[#](https://www.cnblogs.com/xuweiweiwoaini/p/13858906.html#95-docker方式启动配置启动服务端)

```shell
docker run -d \
-p 9411:9411 \
--restart always \
-v /etc/localtime:/etc/localtime:ro \
-e MYSQL_USER=root \
-e MYSQL_PASS=123456 \
-e MYSQL_HOST=192.168.217.100 \
-e STORAGE_TYPE=mysql \
-e MYSQL_DB=zipkin \
-e MYSQL_TCP_PORT=3306 \
-e RABBIT_ADDRESSES=192.168.217.100:5672 \
-e RABBIT_USER=guest \
-e RABBIT_PASSWORD=guest \
--name zipkin \
openzipkin/zipkin:2.12.9
```

[![Zipkin服务端配置消息中间件](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161151723-1220611619.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161151723-1220611619.png)

## P114、day3-42-链路追踪：zipkin客户端向rabbitmq中发送数据并测试

##### 4.7.6 客户端配置[#](https://www.cnblogs.com/xuweiweiwoaini/p/13858906.html#96-客户端配置)

###### 4.7.6.1 导入相关jar包的Maven坐标[#](https://www.cnblogs.com/xuweiweiwoaini/p/13858906.html#961-导入相关jar包的maven坐标)

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

###### 4.7.6.2 修改配置文件

```yaml
spring:
  #zipkin配置
  zipkin:
    #base-url: http://192.168.217.100:9411/ # Zipkin Server端的请求地址
    sender:
      #type: web # 数据的传输方式，以HTTP的形式向Zipkin Server端发送数据
      type: rabbit
  sleuth:
    sampler:
      probability: 1 # 采样比  默认为0.1，即10%，这里配置1，是记录全部的sleuth信息，是为了收集到更多的数据（仅供测试使用）
  rabbitmq:
    host: 192.168.217.100
    port: 5672
    username: guest
    password: guest   
    listener:   # 这里配置了重试机制
      direct:
        retry:
          enabled: true
      simple:
        retry:
          enabled: true
```

##### 4.7.7 测试

- 关闭Zipkin Server，并随意发送请求。打开RabbitMQ管理后台可以看到，消费已经推送到了RabbitMQ中。
- 当Zipkin Server启动时，会自动从RabbitMQ获取消息并消费，展示追踪数据。

## P115、第3天课程总结

**微服务网关Zuul**

**微服务网关Gateway**

**微服务链路跟踪**

## P116、第4天-内容介绍

**SpringCloud Stream** 多个中间件封装

**配置中心**

- SpringCloud config配置中心
- Appllo配置中心

## P117、SpringCloudStream的概述

### 1 Spring Cloud Stream

在实际的企业开发中，消息中间件是至关重要的组件之一。消息中间件主要解决应用解耦、异步消息、流量削峰等问题，实现高性能、高可用、可伸缩和最终一致性的架构。不同的中间件其实现方式，内部结构是不一样的。如常见的RabbitMQ和Kafka，由于这两个消息中间件的架构上的不同，如RabbitMQ有Exchange，Kafka有Topic、Partitions，这些中间件的差异性导致我们实际项目开发会给我们造成了一定的困扰，我们如果用了两个消息中间件的其中一种，后面的业务需求，需要往另一种消息中间件迁移，这时候无疑就是一个灾难，一大堆东西需要重新推倒重做，因为它和我们系统耦合了，这时候`Spring Cloud Stream`给我们提供了一种解耦合的方式。

#### 1.1 概述

Spring Cloud Stream由一个中间件中立的Core组成。应用通过Spring Cloud Stream的input（相当于消费者consumer，它是从队列中接收消息的）和output（相当于生产者producer，它是从队列中发送消息的）通道和外界交流。通道通过指定中间件的Binder实现和外部代理连接。业务开发者不再关注具体消息中间件，只需要关注Binder对应用程序提供的抽象概念来使用消息中间件实现业务即可。

[![Spring Cloud Stream处理架构](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161352257-707461761.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161352257-707461761.png)



## P118、SpringCloudStream的实现思想

## P119、SpringCloudStream的核心概念

#### 1.2 核心概念

##### 1.2.1 绑定器[#](https://www.cnblogs.com/xuweiweiwoaini/p/13858925.html#31-绑定器)

- 绑定器（Binder）是Spring Cloud Stream中的一个非常重要的概念。在没有绑定器这个概念的情况下，我们的SpringBoot应用要直接和消息中间件进行信息交互的时候，由于各个消息中间件构建的初衷不同，它们的实现细节上会有较大的差异性，这似的我们实现的消息交互逻辑就会非常笨重，因此对具体的中间件实现细节有太重的依赖，当中间件有较大的变动升级、或者更换中间件的时候，我们就需要付出非常大的代价来实施。
- 通过定义绑定器作为中间层，实现了应用程序和消息中间件细节之间的隔离。通过向应用程序暴露统一的Channel，使得应用程序不需要再考虑各种不同的消息中间件实现。当需要升级消息中间件或者更换其他的消息中间件产品时，我们需要做的就是更换对应的Binder绑定器而不需要修改任何应用逻辑，甚至可以任意的改变中间件的类型而不需要修改一行代码。
- Spring Cloud Stream支持各种Binder实现，如下图所示：

[![Spring Cloud Stream支持各种Binder实现](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161352573-813314464.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161352573-813314464.png)

- 通过配置把应用和Spring Cloud Stream的binder绑定在一起，之后我们只需要修改binder的配置来达到动态修改Topic、Exchange、type等一系列信息而不需要修改一行代码。

##### 1.2.2 发布/订阅模型[#](https://www.cnblogs.com/xuweiweiwoaini/p/13858925.html#32-发布订阅模型)

- 在Spring Cloud Stream中的消息通信方式遵循了发布--订阅模式，当一条消息被投递到消息中间件之后，它会通过共享的Topic主题进行广播，消息消费者在订阅的主题中收到它并触发自身的业务逻辑处理。这里所提到的Topic主题是Spring Cloud Stream中的一个抽象概念，用来代表发布共享消息给消费者的地方。在不通过的消息中间件中，Topic可能对应着不同的概念，如：在RabbitMQ中它对应了Exchange，在Kafka中对应了Topic。

##### 4 Spring Cloud Stream标准流程套路[#](https://www.cnblogs.com/xuweiweiwoaini/p/13858925.html#4-spring-cloud-stream标准流程套路)

[![pring Cloud Stream标准流程套路1](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161352905-1316777573.bmp)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161352905-1316777573.bmp)

- Middleware：中间件，目前只支持RabbitMQ和Kafka。
- Binder：Binder是应用和消息中间件之间的封装，目前实现了Kafka和RabbitMQ的Binder，通过Binder可以很方便的连接中间件，可以动态的改变消息类型（对应于Kafka的Topic和RabbitMQ的Exchange），这些都是可以通过配置文件来实现。
- @Input：注解标识输入通道，通过该输入通道接收到的消息进入应用程序。
- @Output：注解标识输出通道，发布的消息将通过该通道离开应用程序。
- @StreamListener：监听队列，用于消费者的队列的消息接收。
- @EnableBinding：将通道Channel和Exchange绑定在一起。

[![Spring Cloud Stream标准流程套路2](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161353313-1094741875.bmp)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161353313-1094741875.bmp)

- Binder：很方便的连接中间件，屏蔽差异。
- Channel：通道，是队列Queue的一种抽象，在消息通讯系统中就是实现存储和转发的媒介，通过对Channel实现对队列进行配置。
- Source和Sink：简单可以理解为参照对象就是Spring Cloud Stream本身，从Spring Cloud Stream发布消息就是输出，从Spring Cloud Stream接受消息就是输入。

## P120、消息生产者的入门案例：上

#### 1.3 入门案例

##### 1.3.1 准备工作

案例中采用RabbitMQ作为消息中间件，完成Spring Cloud Stream的案例，需要自行安装

Docker安装方式如下：

```shell
docker run -d  --name rabbit -p 15672:15672 -p 5672:5672 rabbitmq:management
```

##### 1.3.2 消息生产者

###### 1.3.2.1 创建工程引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-stream</artifactId>
</dependency>    
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
</dependency>
```

修改配置文件

```yaml
server:
  port: 7001

spring:
  application:
    name: stream-producer
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest
  cloud:
    stream:
      bindings: # 绑定通道
        output:
          destination: beyondsoft-default#指定消息发送的目的地，在RabbitMQ中，发送一个beyondsoft-defuault的exchange中
      binders: # 配置绑定器
        defaultRabbit: # 表示定义的名称，用于binding整合
          type: rabbit # 消息组件类型
          
```

启动类

```java
package com.beyondsoft.stream.producer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * 入门案例
 * 	1.引入依赖
 * 	2.配置application.yml
 *  3.发送消息的话，需要定义一个通道接口，通过接口中内置的Message Channel。
 * 		Spring Cloud Stream中内置接口 Source
 * 	4.@EnableBinding`注解：绑定对应通道
 * 	5.发送消息的话，通过MessageChannel发送消息
 *		如果需要MessageChannel --> 是通过绑定的内置接口获取的
 */
@SpringBootApplication
@EnableBinding(Source.class)
public class ProducerApplication implement CommonLineRunner{
    
    @Autowired
    private MessageChannel output;
    
    @Override
    public void run(String... args) throws Exception {
        //发送消息
        //通过MessageBuilder工具类 创建发送消息
        output.send(MessageBuilder.withPlayload("hello 超越软件").build());
    }
    
    public static void mian(String[] args){
        SpringApplication.run(ProducerApplication);
    }
}
```



```java
package com.beyondsoft.stream.producer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * 入门案例
 * 	1.引入依赖
 * 	2.配置application.yml
 *  3.发送消息的话，需要定义一个通道接口，通过接口中内置的Message Channel。
 * 		Spring Cloud Stream中内置接口 Source
 * 	4.@EnableBinding`注解：绑定对应通道
 * 	5.发送消息的话，通过MessageChannel发送消息
 *		如果需要MessageChannel --> 是通过绑定的内置接口获取的
 */
@SpringBootApplication
public class StreamProducer9005Application {
    public static void main(String[] args) {
        SpringApplication.run(StreamProducer9005Application.class, args);
    }
}
```



###### 1.3.2.2 定义binding

```

```



## P121、消息生产者的入门案例：下

## P122、消息消费者的入门案例：下

##### 1.3.3 消息消费者

###### 1.3.3.1 创建工程引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
```

###### 1.3.3.2 定义binding

同发送消息一致，在Spring CLoud Stream中介绍消息， 需要定义一个接口，如下是内置的一个接口。

```java
public interface Sink {
    String INPUT ="input";

    @Input("input")
    SubscribableChannel input();
}
```

注释@Input对应的方法，需要返回`SubscribableChannel`，并且传入一个参数值。

这里接口声明了一个binding命名为“**input**”。

###### 1.3.2.3 配置application.yml

```yaml
server:
  port: 7002
spring:
  application:
    name: stream-consumer #指定服务名
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest
  cloud:
    stream:
      bindings: # 绑定通道
        input:	# 内置额获取消息的通道，从beyondsoft-defuaul 通道中获取
          destination: beyondsoft-default#指定消息发送的目的地，在RabbitMQ中，发送一个beyondsoft-defuault的exchange中
      binders: # 配置绑定器
        defaultRabbit: # 表示定义的名称，用于binding整合
          type: rabbit # 消息组件类型
          
```

###### 1.3.2.4 测试

```java
    /**
    * 	1. 引入依赖
    	2. 配置application.yml
    	3. 需要配置一个通道的接口
    	   不需要自己写 内置了一个获取消息的通道接口 sink
    	4. 绑定通道
    	5. 配置一个监听方法： 当程序从中间件获取数据之后，执行的业务逻辑方法
	    	需要在监听方法上配置@StreamListener
    */
@SpringBootApplication
@EnableBinding(Skin.class)
public class ConsumerApplication{

    //监听binding中的消息
    @StreamListener(Skin.INPUT)
    public void input(String message){
        System.out.println("获取到的消息"+message);
    }
    
    public static void main(String[] args){
        SpringApplication.run(ConsumerApplication.class);
    }
}
```

## P123、基于入门案例的代码优化

以上课程中，消息生产的发送消息和消息的消费者接收消息 都是写到了启动类中，这只是为了快速学习了解测试stream，实际生产工作中需要将发送和接收抽取出来作为工具类使用。

**生产者端**

消息发送工具类

```java
/**
 负责向中间件发送数
*/
@Component
@EnableBinding(Source.class)
public class MessageSender{
    
    @Autowired
    private MessageChannel output;
    
    //发送消息
    public void send(Object object) throws Exception {
        //发送消息
        //通过MessageBuilder工具类 创建发送消息
        output.send(MessageBuilder.withPlayload(object).build());
    }
}
```

启动类

```java
@SpringBootApplication
public class ProducerApplication    
    public static void mian(String[] args){
        SpringApplication.run(ProducerApplication);
    }
}
```

测试类

```java
@Runwith(SpringJUnit4ClassRunner.class)
@SpringBootTest
public class classProducerTest{
    @Autowired
    private MessageSender messageSender;
    
    @Test
    public void testSend(){
        messageSender.send("hello 工具类");
    }
}
```

**消费者端**

消费者监听类

```java
@Component
@EnableBinding(Skin.class)
public class MessageListener{

    //监听binding中的消息
    @StreamListener(Skin.INPUT)
    public void input(Object object){
        System.out.println("获取到的消息"+object);
    }
}
```

启动类

```java
@SpringBootApplication
public class ConsumerApplication{
    public static void main(String[] args){
        SpringApplication.run(ConsumerApplication.class);
    }
}
```



## P124、SpringCloud Stream-自定义消息通道

Spring Cloud Stream内置类两种接口，分别定义了binding为“input”输入流和“output”输出流，而在我们实际使用中，往往需要定义各种输入输出流。使用方法也很简单。

**生产者端**

自定义通道

```java
/**
  自定义消息通道
*/
public interface MyProcessor{
    /** 消息生产者的配置*/
    String MYOUTPUT ="myoutput";
    
    @Outut("output")
    MessageChannel myoutput();
    
    /** 消息消费者的配置*/
    String MYINPUT="myinput";
    @Input("myinput")
    SubscribableChannel myinput();
    
    //消息通道 在一个接口中可以定义若干组； 以上定义了一组
}
```

消息发送工具类

```java
//@EnableBinding(Source.class)
@EnableBinding(MyProcessor.class)  //将消息发送到自己定义的通道MyProcessor上
public class MessageSender{
    
    @Autowired
    @Qualifier(value="myoutput")
    private MessageChannel myoutput;
    
    //发送消息
    public void send(Object object) throws Exception {
        //发送消息
        //通过MessageBuilder工具类 创建发送消息
        myoutput.send(MessageBuilder.withPlayload(object).build());
    }
}
```

配置文件

```yaml
  cloud:
    stream:
      bindings: # 绑定通道
        output:
          destination: beyondsoft-default #指定消息发送的目的地，在RabbitMQ中，发送一个beyondsoft-defuault的exchange中
        myoutput: #自己定义的通道
          destination: my-channel # 发送到的通道名称 
      binders: # 配置绑定器
        defaultRabbit: # 表示定义的名称，用于binding整合
          type: rabbit # 消息组件类型
```

**消费者端**

自定义通道

```java
/**
  自定义消息通道
*/
public interface MyProcessor{
    /** 消息生产者的配置*/
    String MYOUTPUT ="myoutput";
    
    @Outut("output")
    MessageChannel myoutput();
    
    /** 消息消费者的配置*/
    String MYINPUT="myinput";
    @Input("myinput")
    SubscribableChannel myinput();
    
    //消息通道 在一个接口中可以定义若干组； 以上定义了一组
}
```

接收消息监听类

```java
@Component
@EnableBinding(MyProcessor.class)
public class MessageListener{

    //监听binding中的消息
    @StreamListener(MyProcessor.INPUT)
    public void input(Object object){
        System.out.println("获取到的消息"+object);
    }
}
```

配置文件

```yaml
  cloud:
    stream:
      bindings: # 绑定通道
        input:	# 内置额获取消息的通道，从beyondsoft-defuaul 通道中获取
          destination: beyondsoft-default#指定消息发送的目的地，在RabbitMQ中，发送一个beyondsoft-defuault的exchange中
        myinput: #自定义的通道
          destination: my-channel # 接收的通道名称 
      binders: # 配置绑定器
        defaultRabbit: # 表示定义的名称，用于binding整合
          type: rabbit # 消息组件类型
```



## P125、SpringCloud Stream-消息分组

通常在生产环境中，我们的每个服务都不会以单节点的方式，当同一个服务启动多个实例的时候，这些实例都会绑定到同一个消息通道的目标主题（Topic）上。默认情况下，当生产者发出一条消息到绑定通道上，这条消息会产生多个副本被每个消费者实例接收和处理，但是有些业务场景下，我们希望生产者的消息只被一个实例消费，这个时候我们需要为这些消费者设置消费组来实现这样的功能。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161354600-2089310066.png)

实现的方式很简单，我们只需要在服务消费端设置spring.cloud.stream.bindings.input.group属性即可，组名一致 同组内就只有一人消费消息

如下配置：

```yaml
server:
  port: 9006

spring:
  application:
    name: stream-consumer
  #  rabbitmq:
  #    host: 192.168.32.100
  #    port: 5672
  #    username: guest
  #    password: guest
  cloud:
    stream:
      binders: # 配置绑定器
        defaultRabbit: # 表示定义的名称，用于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置RabbitMQ的相关的环境配置
            spring:
              rabbitmq:
                host: 192.168.32.100
                port: 5672
                username: guest
                password: guest
      bindings: # 绑定通道
        input:
          destination: rabbit-default # 指定消息获取的目的地。
          contentType: application/json # 消息的类型
          binder: defaultRabbit
          group: group1 # 设置消息的组名（同名组中的多个消息者，只有一个去消费消息）
        # 自定义通道
        customInput:
          destination: rabbit-custom
          group: group2 # 设置消息的组名（同名组中的多个消息者，只有一个去消费消息）
```



## P126、SpringCloud Stream-消息分区

有一些场景需要满足，同一特征的数据被同一个实例消费，比如同一个id的传感器监测数据必须被同一个实例统计计算分组，否则可能无法获取全部的数据。又比如部分异步任务，首次请求启动task，二次请求取消task，此场景就必须保证两次请求到同一个实例。

[![消息分区](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161354786-593291102.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161354786-593291102.png)

- 消息生产者配置：

```yaml
server:
  port: 9005

spring:
  application:
    name: stream-producer
  #    rabbitmq:
  #      host: 192.168.32.100
  #      port: 5672
  #      username: guest
  #      password: guest
  cloud:
    stream:
      binders: # 配置绑定器
        defaultRabbit: # 表示定义的名称，用于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置RabbitMQ的相关的环境配置
            spring:
              rabbitmq:
                host: 192.168.32.100
                port: 5672
                username: guest
                password: guest
      bindings: # 绑定通道
        output:
          destination: rabbit-default # 指定消息发送的目的地。在RabbitMQ中，发送到一个名为rabbit-default的Exchange上。
          contentType: application/json # 消息的类型
          binder: defaultRabbit
        # 自定义通道
        customOutput:
          destination: rabbit-custom
          producer:
            # ===========修改部分开始===========
            partition-key-expression: payload #通过该参数指定了分组键的表达式规则，我们可以根据实际的输出消息规则来配置SPEL来生成合适的分区键
            partition-count: 2 # 该参数指定了消息分区的数量 有几个消费者
            # ===========修改部分结束===========          
```

- 消息消费者配置：

```yaml
server:
  port: 9006

spring:
  application:
    name: stream-consumer
  #  rabbitmq:
  #    host: 192.168.32.100
  #    port: 5672
  #    username: guest
  #    password: guest
  cloud:
    stream:
      # ===========修改部分开始===========
      instance-count: 2 # 该参数指定了当前消费者实例的数量  消费者总数
      instance-index: 0 # 该参数设置当前实例的索引号，从0开始，最大值为spring.stream.instance-count-1
      # ===========修改部分结束===========
      binders: # 配置绑定器
        defaultRabbit: # 表示定义的名称，用于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置RabbitMQ的相关的环境配置
            spring:
              rabbitmq:
                host: 192.168.32.100
                port: 5672
                username: guest
                password: guest
      bindings: # 绑定通道
        input:
          destination: rabbit-default # 指定消息获取的目的地。
          contentType: application/json # 消息的类型
          binder: defaultRabbit
          group: group1 # 设置消息的组名（同名组中的多个消息者，只有一个去消费消息）
        # 自定义通道
        customInput:
          destination: rabbit-custom
          group: group2 # 设置消息的组名（同名组中的多个消息者，只有一个去消费消息）
          # ===========修改部分开始===========
          consumer:
            partitioned: true # 开启消费者分区功能
          # ===========修改部分结束===========
```

## P127、SpringCloud Config-配置中心的概述

### 2.1 什么是配置中心

#### 2.1.1 配置中心概述

对于传统的单体应用而言，常使用配置文件来管理所有配置，比如SpringBoot的application.yml文件，但是在微服务架构中全部手动修改的话很麻烦而且不易维护。

微服务的配置管理一般有以下需求：

- 1️⃣集中配置管理，一个微服务架构中可能有成百上千个微服务，所以集中配置管理很重要。
- 2️⃣不同环境不同配置：比如数据源配置在不同环境（开发、测试、生产）中是不同的。
- 3️⃣运行期间可动态调整。比如可以根据各个微服务的负载情况，动态调整数据源连接池的大小等。
- 4️⃣配置修改后可以自动更新。比如配置内容发生改变，微服务可以自动更新配置。

综上所述，对于微服务架构来说，一套统一的、通用的管理配置机制是不可缺少的重要组成部分。常见的做法就是通过配置服务器进行管理。

#### 2.1.2 常见的配置中心

##### 2.1.2.1 Spring Cloud Config

Spring Cloud Config是分布式系统中的外部配置提供服务器和客户端支持。

##### 2.1.2.2 Apollo（阿波罗）

Apollo（阿波罗）是携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景。

##### 2.1.2.3 Nacos

Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。

Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。

##### 2.1.2.4 Disconf

是百度开发的，颛臾与各种 分布式系统配置管理的通用组件和通用平台，提供统一的配置管理服务，包括：百度、滴滴、银联、网易、拉勾网、苏宁易购、顺丰科技、等指明户联防公司正在使用Disconf，在2015年度新增开源圆角排名TOP100（OSC开源中国提供）中的排名第16强。

## P128、SpringCloud Config-概述

#### 2.2 Spring Cloud Config简介

Spring Cloud Config项目是一个解决分布式系统的配置解决方案。它包含了Client和Server两个部分，Server提供配置文件的存储，以接口的形式将配置文件的内容提供出去；Client通过接口获取数据，并依据此数据初始化自己的应用。

[![Spring Cloud Config简介](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161526248-590746099.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161526248-590746099.png)

Spring Cloud Config为分布式系统的外部配置提供服务器和客户端支持。使用Config Server，您可以为所有环境中的应用程序管理其外部属性。它非常适合Spring应用，也可以使用在其他语言的应用上。随着应用程序通过从开发到测试和生产的部署流程，您可以管理这些环境之间的配置，并确定应用程序具有迁移时需要运行的一切。服务器存储后端的默认使用使用Git，因此他轻松的支持标签版本的配置环境以及可以访问用于管理内容的各种工具。

Spring Cloud Config服务端的特性：

- 1️⃣Http，为外部配置提供基于资源的API（键值对或者等价的YAML内容）。
- 2️⃣属性值的加密和解密（对称加密和非对称加密）。
- 3️⃣通过使用@EnableConfigServer在Spring Boot应用中非常简单的嵌入。

Spring Cloud Config客户端的 特性：

- 1️⃣绑定Config服务端，并使用远程的属性源初始化Spring环境。
- 2️⃣属性值的加密和解密（对称加密和非对称加密）。

## P129、SpringCloud Config-入门案例



## P130、springcloudConfig入门案例：搭建config服务端

##### 2.3.1 准备工作

Config Server是一个可横向扩展、集中式的配置服务器，它用于集中管理应用程序各个环境下的配置，默认使用Git存储配置文件内容，也可以使用SVN存储或者是本地文件存储。这里使用git作为学习的环境。

1. 登录码云gitee.com

2. 在码云上创建仓库：例如：config-repo

[![在码云上创建仓库](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161526475-1733005617.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161526475-1733005617.png)

3. 上传配置文件

创建`product-dev.yml`和`product-prod.yml`文件上传到创建的仓库上。

**文件命名规则：**

- {application}-{profile}.yml
- {application}-{profile}.properties
- application为应用名称 profile为指定的开发环境（用于区分开发环境、测试环境、生成环境等）

##### 2.3.2 搭建服务端程序

###### 2.3.2.1 引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>    
    <artifactId>spring-cloud-config-server</artifactId>    
</dependency>
```

###### 2.3.2.2 配置启动类

```java
@EnableConfigServer
@SpringBootApplication
public class ConfigServerApplication {
    public static manin(String[] args){
        SpringApplication.run(ConfigServerApplication.class,args);
    }
}
```

###### 2.3.2.3  配置文件

```yml
server:
  port: 10000
spring:
  application:
    name: config-server
  cloud:
    config:
      server:
        git:
          # git服务地址
          uri: https://gitee.com/AncientFairy/config-repo.git
          # 配置git的用户名
          username:
          # 配置git的密码
          password:
          search-paths:
            - config-repo
      # 分支
      label: master


# 配置 eureka
eureka:
  instance:
    # 主机名称:服务名称修改，其实就是向eureka server中注册的实例id
    instance-id: config-server:${server.port}
    # 显示IP信息
    prefer-ip-address: true
  client:
    service-url: # 此处修改为 Eureka Server的集群地址
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

###### 2.3.2.4 测试

启动微服务，可以在浏览器，通过访问http://localhost:10000/master/product-dev.yml请求访问到Git服务器上的文件。

如果没有配置git分支 http://localhost:10000/product-dev.yml

[![服务器端程序测试](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161526686-1040249464.png)](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161526686-1040249464.png)

## P131、day4-16-springcloudConfig入门案例：客户端改造，动态获取配置信息



##### 2.3.3修改客户端程序

###### 2.3.3.1 引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

###### 2.3.3.2 bootstrap.yml

```yaml
spring:
  cloud:
    config:
      name: product # 应用名称，需要对应git中配置文件名称的前半部分
      profile: prod # 开发环境，需要对应git中配置文件名称的后半部分
      label: master # 分支名称
      uri: http://localhost:10000 # config-server的请求地址
```

###### 2.3.3.3 测试



## P132、day4-17-springcloudConfig入门案例：手动刷新数据

##### 2.3.4 手动刷新

我们已经在客户端获取到了配置中心的值，但是当我们修改git中的值的时候，服务端（Config Server）能实时的获取到最新的值，但是客户端（Config Client）读取的是缓存，无法实时获取最新值。SpringCloud已经为我们解决了这个问题，那就是客户端使用POST去触发refresh，获取最新数据。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161527085-767941016.jpg)

需要依赖spring-boot-starter-actuator。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

在对应的Controller类中添加@RefreshScope注解：

```java
@RestController
@RequestMapping(value = "/product")
@RefreshScope //开启动态刷新
public class ProductController {

    @Value("${server.port}")
    private String port;

    @Value("${spring.cloud.client.ip-address}")
    private String ip;

    @Autowired
    private ProductService productService;

    @PostMapping(value = "/save")
    public String save(@RequestBody Product product) {
        this.productService.save(product);
        return "新增成功";
    }

    @GetMapping(value = "/findById/{id}")
    public Product findById(@PathVariable(value = "id") Long id) {
        Product product = this.productService.findById(id);
        product.setProductName("访问的地址是：" + this.ip + ":" + this.port);
        return product;
    }
}
```

在application.yml中开放端点配置：

```yml
spring:
  cloud:
    config:
      name: product # 应用名称，需要对应git中配置文件名称的前半部分
      profile: dev # 开发环境，需要对应git中配置文件名称的后半部分
      label: master # 分支名称
      uri: http://localhost:9010 # config-server的请求地址

# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: '*'
```

手动刷新

http://localhost:9000/actuator/refresh

使用potman 发送post请求到这个接口 实现刷新

需要使用`curl -X POST "http://localhost:9002/actuator/refresh"`发送请求。


## P133、SpringcloudConfig高可用-上

#### 2.4 配置中心高可用

在之前的代码中，客户端都是直接调用配置中心的Server端来获取配置信息。这样就存在了一个问题，客户端和服务端的耦合性太高，如果Server要做集群，客户端只能通过原始的方式来路由，Server端改变IP地址的时候，客户端也需要修改配置，不符合Spring Cloud 服务治理的概念。

Spring Cloud提供了这样的解决方案，我们只需要将Server端当做一个服务注册到Eureka中，Client端去Eureka中去获取配置中心Server端的服务即可。

##### 2.4.1 服务器端改造

引入pom依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
spring-cloud-starter-netflix-eureka-client
spring-cloud-config-server
spring-cloud-stater-config
spring-boot-starter-actuator
spring-cloud-bus
spring-cloud-strean-binder-rabbit
```

配置文件

```yml
# 配置 eureka
eureka:
  instance:
    # 主机名称:服务名称修改，其实就是向eureka server中注册的实例id
    instance-id: config-server:${server.port}
    # 显示IP信息
    prefer-ip-address: true
  client:
    service-url: # 此处修改为 Eureka Server的集群地址
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

##### 

## P134、SpringcloudConfig高可用-下

##### 2.4.2 客户端改造

bootstrap.yml

```yaml
spring:
  cloud:
    config:
      name: product # 应用名称，需要对应git中配置文件名称的前半部分
      profile: dev # 开发环境，需要对应git中配置文件名称的后半部分
      label: master # 分支名称
      #uri: http://localhost:9010 # config-server的请求地址
      discovery: # 服务发现
        service-id: config-server
        enabled: true #从Eureka中获取配置信息

#配置 eureka
eureka:
  instance:
    #主机名称:服务名称修改，其实就是向eureka server中注册的实例id
    instance-id: service-product-dev:${server.port}
    # 显示IP信息
    prefer-ip-address: true
  client:
    service-url: # 此处修改为 Eureka Server的集群地址
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/

# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: '*'
```



##### 2.4.3 高可用

## P135、SpringcloudConfig：结合消息总线bus动态修改配置文件信息

#### 2.5 消息总线bus

在微服务架构的系统中，通常会使用轻量级的消息代理来构建一个共用的消息主题，并让系统中所有微服务实例都连接上来。由于该主题中产生的消息会被所有实例监听和消费，所以称其为消息总线。

Spring Cloud Bus配合Spring Cloud Config使用可以实现配置的动态刷新。

在总线上的各个实例，都可以方便的传播一些需要让其他连接在该主题上的实例都知道的消息。

![img](https://img2020.cnblogs.com/blog/1128804/202010/1128804-20201022161725770-1541598355.png)



根据上图我们可以看出Spring Cloud Bus做配置更新的步骤：

- 1️⃣提交代码触发POST请求给bus/refresh。
- 2️⃣server端接收到请求并发送给Spring Cloud Bus。
- 3️⃣Spring Cloud Bus接收到消息并通知给其他客户端。
- 4️⃣其他客户端接收到通知，请求Server端获取最新配置。
- 5️⃣全部客户端获取到最新的配置。

#### 2.6 消息总线整合配置中心

##### 2.6.1 引入依赖

```xml
		<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus</artifactId>
        </dependency>
```



##### 2.6.2 服务端配置

```yaml
server:
  port: 9010
spring:
  application:
    name: config-server
  # ----修改部分------
  # 配置rabbitmq
  rabbitmq:
    host: 192.168.1.57
    port: 5672
    username: guest
    password: guest
  # ----修改部分------
  cloud:
    config:
      server:
        git:
          # git服务地址
          uri: https://gitee.com/AncientFairy/config-repo.git
          # 配置git的用户名
          username:
          # 配置git的密码
          password:
          search-paths:
            - config-repo
      # 分支
      label: master

# 配置 eureka
eureka:
  instance:
    # 主机名称:服务名称修改，其实就是向eureka server中注册的实例id
    instance-id: config-server:${server.port}
    # 显示IP信息
    prefer-ip-address: true
  client:
    service-url: # 此处修改为 Eureka Server的集群地址
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/

# ----修改部分------
# 配置端点
management:
  endpoints:
    web:
      exposure:
        include: 'bus-refresh'
# ----修改部分------
```



##### 2.6.3 微服务客户端配置

引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus</artifactId>
</dependency>
spring-cloud-starter-netflix-client
spring-cloud-statter-config
spring-boot-starter-actuator
spring-cloud-stream-binder-rabbit
```

修改配置文件bootstrap.yml

```yaml
spring:
  # ----修改部分------
  # 配置rabbitmq
  rabbitmq:
    host: 192.168.1.57
    port: 5672
    username: guest
    password: guest
  # ----修改部分------
  cloud:
    config:
      name: product # 应用名称，需要对应git中配置文件名称的前半部分
      profile: dev # 开发环境，需要对应git中配置文件名称的后半部分
      label: master # 分支名称
      #uri: http://localhost:9010 # config-server的请求地址
      discovery: # 服务发现
        service-id: config-server
        enabled: true # 从Eureka中获取配置信息

# 配置 eureka
eureka:
  instance:
    # 主机名称:服务名称修改，其实就是向eureka server中注册的实例id
    instance-id: service-product-dev:${server.port}
    # 显示IP信息
    prefer-ip-address: true
  client:
    service-url: # 此处修改为 Eureka Server的集群地址
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

git giteee上的application.yml需要增加 RabbitMQ的配置

```yaml
spring:
  rabbitmq:
    host: 192.168.1.57
    port: 5672
    username: guest
    password: guest
```



## P136、开源配置中心Apollo：概述

### 3 开源配置中心Appllo

Apollo（阿波罗）是携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景。

服务端基于Spring Boot和Spring Cloud开发，打包后可以直接运行，不需要额外安装Tomcat等应用容器。

#### 3.1 Applo概述

Apollo（阿波罗）是携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景。

正式基于配置的特殊性，所以Apollo从设计之处就立志成为一个有治理能力的配置发布平台，不去提供了以下特性：

- **统一管理不同环境、不同集群的配置**
  - Apollo提供了一个统一界面集中式管理不同环境（environment）、不同集群（cluster）、不同命名空间（namespace）的配置。
  - 同一份代码部署在不同的集群，可以有不同的配置，比如zk的地址等
  - 通过命名空间（namespace）可以很方便的支持多个不同应用共享同一份配置，同时还允许应用对共享的配置进行覆盖
  - 配置界面支持多语言（中文，English）
- **配置修改实时生效（热发布）**
  - 用户在Apollo修改完配置并发布后，客户端能实时（1秒）接收到最新的配置，并通知到应用程序。
- **版本发布管理**
  - 所有的配置发布都有版本概念，从而可以方便的支持配置的回滚。
- **灰度发布**
  - 支持配置的灰度发布，比如点了发布后，只对部分应用实例生效，等观察一段时间没问题后再推给所有应用实例。
- **权限管理、发布审核、操作审计**
  - 应用和配置的管理都有完善的权限管理机制，对配置的管理还分为了编辑和发布两个环节，从而减少人为的错误。
  - 所有的操作都有审计日志，可以方便的追踪问题。
- **客户端配置信息监控**
  - 可以方便的看到配置在被哪些实例使用
- **提供Java和.Net原生客户端**
  - 提供了Java和.Net的原生客户端，方便应用集成
  - 支持Spring Placeholder，Annotation和Spring Boot的ConfigurationProperties，方便应用使用（需要Spring 3.1.1+）
  - 同时提供了Http接口，非Java和.Net应用也可以方便的使用
- **提供开放平台API**
  - Apollo自身提供了比较完善的统一配置管理界面，支持多环境、多数据中心配置管理、权限、流程治理等特性。
  - 不过Apollo出于通用性考虑，对配置的修改不会做过多限制，只要符合基本的格式就能够保存。
  - 在我们的调研中发现，对于有些使用方，它们的配置可能会有比较复杂的格式，如xml, json，需要对格式做校验。
  - 还有一些使用方如DAL，不仅有特定的格式，而且对输入的值也需要进行校验后方可保存，如检查数据库、用户名和密码是否匹配。
  - 对于这类应用，Apollo支持应用方通过开放接口在Apollo进行配置的修改和发布，并且具备完善的授权和权限控制
- **部署简单**
  - 配置中心作为基础服务，可用性要求非常高，这就要求Apollo对外部依赖尽可能地少
  - 目前唯一的外部依赖是MySQL，所以部署非常简单，只要安装好Java和MySQL就可以让Apollo跑起来
  - Apollo还提供了打包脚本，一键就可以生成所有需要的安装包，并且支持自定义运行时参数



## P137、开源配置中心Apollo：实现过程

#### 3.2 Apollo的实现方式

![img](https://img2018.cnblogs.com/blog/1179709/201811/1179709-20181119235119112-153211437.png)

上图简要描述了Apollo客户端的实现原理：

1. 客户端和服务端保持了一个长连接，从而能第一时间获得配置更新的推送。（通过Http Long Polling实现）
2. 客户端还会定时从Apollo配置中心服务端拉取应用的最新配置。
   - 这是一个fallback机制，为了防止推送机制失效导致配置不更新
   - 客户端定时拉取会上报本地版本，所以一般情况下，对于定时拉取的操作，服务端都会返回304 - Not Modified
   - 定时频率默认为每5分钟拉取一次，客户端也可以通过在运行时指定System Property: `apollo.refreshInterval`来覆盖，单位为分钟。
3. 客户端从Apollo配置中心服务端获取到应用的最新配置后，会保存在内存中
4. 客户端会把从服务端获取到的配置在本地文件系统缓存一份
   - 在遇到服务不可用，或网络不通的时候，依然能从本地恢复配置
5. 应用程序可以从Apollo客户端获取最新的配置、订阅配置更新通知



## P138、开源配置中心Apollo：搭建服务端

#### 3.3 搭建Apollo服务端

##### 3.3.1 环境要求

**Java**

- Apollo服务端：1.8+

- Apollo服务端： 1.7+

**MySQL**

- 版本要求：5.6.5+
- Apollo的表结构对timestamp使用了多个default声明，需哦一需要5.6.5 以上

##### 3.3.2 环境搭建

**(1) 下载Applo**

通过官网下载安装包 [地址](https://github.com/ctripcorp/apollo)

通过[网盘链接](https://pan.baidu.com/s/1Ieelw6y3adECgktO0ea0Gg)下载，提取码: 9wwe

**(2) 配置数据库 **

**Apollo服务端需要两个数据库，分别为ApolloPortalDB和ApolloConfigDB**，因此我们需要先创建两个数据库。

我们把数据库、表的创建 在安装包中 保护了两个sql文件，只需导入即可

> 注意如果你的本地 已经创建了Apollo的数据库，请注意备份数，准备的SQL脚本会清空Apollo相关的表

**Apollo建议将服务安装到Linux服务上**

将zip包上传到linux服务器

```bash
mkdir /usr/local/apollo
mv apollo-quick-start-1.6.0.zip /usr/local/apollo 
unzip apollo-quick-start-1.6.0.zip
vim demo.sh  #修改mysql连接地址 用户名密码
#修改好之后 保存退出
./demo start #启动 apollo

```

**(3) 配置数据库连接 **


## P139、开源配置中心Apollo：管理后台介绍


http://ip:8070  Apollo的服务端地址

![img](https://img-blog.csdnimg.cn/20200929132436642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzNDU3MA==,size_16,color_FFFFFF,t_70#pic_center)

**apollo账号密码**
账号:apollo
密码:admin



## P140、开源配置中心Apollo：客户端集成开发

#### 3.4 客户端集成

##### 3.4.1 引入依赖

```xml
<dependency>
    <group>com.ctrip.framework.appollo</group>
    <artifactId>apollo-client</artifactId>
    <version>1.1.0</version>
</dependency>
```



##### 3.4.2 SpringBoot集成

配置文件

```yml
apollo:
  bootstrap:  #开启apollo工作空间
    enable: true
  meta: http://192.168.1.110:8080  # 连接apollo的服务端的 eureka
app:
  id: product-service  #指定apollo配置中心中 创建的应用id
```

配置中心中，新增配置 ： key 为yml或者properties的属性名， value为 属性值，修改之后保存提交，默认是未发布状态

点击发布之后，这个属性值 就在客户端自动生效了

启动时 需要加入启动参数 指明 是开发环境 还是测试环境 或者 正式环境



## P141、第四天课程总结

##### Spring Cloud Stream

##### Spring Cloud Config

##### Spring Cloud Bus

##### Appllo