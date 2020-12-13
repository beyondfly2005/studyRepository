> #### 4天从浅入深精通SpringCloud微服务架构
>
> https://www.bilibili.com/video/BV1eE41187Ug?p=75



### P8、CAP原理



### P9、SpringBoot概述

ServiceComB

ZeroC ICE

SpringCloud 服务注册于发现、配置中心、消息总线、负载聚恒、断路器、数据监控等，

- Eureka 

- Ribbon
- Feign
- Hystrix
- Zuul

SpringCloud Alibaba组件

- Nacos

- 



### P10、模拟微服务环境

创建父工程

引入坐标

- spring-boot-start-parent 2.1.6.RELEASE
- 编码格式UTF-8
- starter-web
- starter-logging
- starter-test
- lombok
- spring-cloud-dependencies Greenwich.RELEASE版本

创建product-service子工程

- mysql-connector-java
- spring-boot-starter-data-jpa

创建数据库表

- 商品表
- 字段：id、 name status price desc caption标题 inventory

- 商品实体类Product entity包
- JPA 接口继承JpaRepository<Product,Long> , JpaSpecificationExecutor<Product>
- Service接口ProductService
- findById 查询 save 保存 update更新 delete删除接口
- ServiceImpl实现 注入Product接口
- Controller控制器
- 启动类ProductApplication @EntitySacan("com.itcast.product.entity")
- 配置文件Application.yml 
- server端口 musql连接 JPA配置 application名称
- 测试浏览器访问

### P12、RestTemplate

远程服务调用http接口返回json的方法

java的urlConnection、Httpclient、OKhttp等

RestTemplate远程调用

使用方法：

1、注入RestTemplate 创建对象 由spring管理

```
// #在启动类中或配置类中加入如下代码
@Bean
public RestTemplate restTemplate(){
	return new RestTemplate;
}
```

2、使用RestTemplate.的方法 getXX PostXX等

```
Product product = restTemplate.getForObject(url,Product.class);
return product
```



### P13、模拟微服务中存在的问题

问题：

调用路径 硬编码到java代码中

如果要用多个微服务调用，在调用者方需要管理多个服务

如果同一个微服务部署多份，如何实现负载均衡 让每一个服务都轮询的被调用到

API网关实现多个为服务器的调用管理

配置的统一管理

调用的链路追踪



### P14、注册中心概述

### P15、注册中心Eureka概述

#### P16、搭建EurekaServer

#### 



### P16、搭建Eureka注册中心

1、搭建Eureka Server

1.1 创建工程

1.2 导入pom依赖 坐标

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
```

1.3 配置Application.yml配置文件

```yml
server:
  port: 9000

spring:
  application:
    name: Eureka-Server

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

1.4 配置启动类

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }

}

```

1.4 测试Eureka是否配置成功

```
浏览器访问
http://localhost:9000/
```



P17、将服务注册到Eureka注册中心

2.1 引入Eureka的pom坐标

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

2.2 修改配置文件application.yml 添加EurekaServer的信息

```

```

2.3 修改启动类添加服务发现的支持

```

```

### P17、Eureka将服务注册到注册中心

### P18、获取微服务的调用路径



### P19、Eureka高可用集群



### P20、EurekaServer的相互注册

```yml
server:
  port:8000
spring:
  application:
    name: Eureka-Server
eureka:
  client:
    register-with-eureka: true #是否将自己注册到注册中心
    fetch-registry: true #是否从eureka中获取信息
    service-url:
      defaultZone: http://127.0.0.1:9000/eureka
      
```

```yml
server:
  port:9000
spring:
  application:
    name: Eureka-Server
eureka:
  client:
    register-with-eureka: true #是否将自己注册到注册中心
    fetch-registry: true #是否从eureka中获取信息
    service-url:
      defaultZone: http://127.0.0.1:8000/eureka
      
```

需要将微服务同时注册到 两个Eureka-Server

```yml
eureka:
  client:
    service-url
     default-zone:http://192.168.1.254:8000,http://192.168.1.254:9000
```

### P22、Eureka显示IP与服务续约时间

在服务提供者，通过eureka.instance,instance-id配置控制台显示服务的ip

```yum
eureka:
  instance:
   instance-id:${spring.cloud.client.ip-address}:${server.port} #向注册中心注册服务id
```

Eureka的服务剔除问题

每隔30秒发送一次心跳

如果90秒内没有发送心跳，则代表，服务已经宕机

正式系统 这个时间会显得很长

在服务提供者 续约时间

```
eureka:
  instance:
   instance-id:${spring.cloud.client.ip-address}:${server.port} #向注册中心注册服务id
   lease-renewal-duration-in-seconds: 10 #10秒续约时间
   
   
```



### P28、Ribbon概述及基于Ribbon的远程调用

#### Ribbon概述

Ribbon是Netflix发布的一个负载均衡器，有助于控制http和tcp客户端行为。在springCloud中Eureka一般配合Ribbon进行使用，

#### 6.2.2 Ribbon的主要作用

##### (1) 服务调用

基于Ribbon实现服务调用，是通过拉取到的所有服务列表组成（服务名-请求路径）映射关系，借助RestTemplate最终进行调用

##### (2) 负载均衡

当有多个服务提供者时，Ribbon可以根据负载聚恒的算法自动的选择需要调用的服务地址

eurka内部集成了ribbon

- 在创建RestTemplate的时候，声明@LoadBalanced
- 使用RestTemplate调用远程微服务，不需要拼接微服务的url 以待请求的服务名替换IP地址

```java
#使用微服务名称替换ip地址
@RequestMapping(value="/buy/{id}",method=RequestMethod.GET)    
public Produc findByid(){
    Prodct  product = null;
    product = restTemplate.getForObject("http://127.0.0.1:9001/rpoduct/1",Product.class);
    //改为
    product = restTemplate.getForObject("http://service-product/rpoduct/1",Product.class);
}
```

#### 6.3 基于Ribbon实现订单调用商品服务

不论是基于Eureka的注册中心还是基于Consul的注册中心，SpringCloudRibbon统一进行了封装，所以对于服务调用，两者的方式是一样的。

1、引入坐标

Eureka内部集成了Ribbbon 不需要在引入其他pom依赖

2、修改调用RestTemplate 调用方式







### P34、Consul概述 替换Necos

Consul是HashiCorp公司推出的开源工具，用于实现分布式系统的服务发现与配置，与其他分布式服务注册于发现

Consul的优势：

- 官方提供web页面



Consul与Eureka的区别

- Consul是go语言编写的, Necos是java编写的
- Eureka保证的是AP 一致性差一点，Consul是强一致性CP 牺牲的是可用性
- Eurka 短时间内 可能额存在 数据不一致，但是能保证最终一致性



### P44、Feign概述

Ribbin远程调用存在的问题

如果调用多个微服务 需要配置多个

需要拼接url 及参数，如果参数过多 ，拼接会很麻烦

### 1 服务调用Feign入门

#### 1.1 Feign简介

Feign是Netf开发的声明式模板化http客户端，其灵盖来自于Retrofit、JAXRS-2.0以及WebSocket

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
    Product product = product FeignClient.findById(id);
    return product
}
```

### P45、入门案例搭建

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

#### 

### P46、

### P47、Feign:负载均衡策略
### P48、Feign:打印Feigin日志

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

  

### P49、Feign源码分析

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



### P50、高并发环境：模拟环境

#### 3 服务注册与发现总结

#### 3.1 注册中心

##### （1）Eureka

- 搭建注册中心
- 服务注册

##### （2）Consul

- 搭建注册中心
- 服务注册

#### 3.2 服务调用

##### （1）Ribbon

- 通过Ribbon结合RestTemplate方式进行服务调用只需在声明RestTemplate的方法上添加注解@LoadBalanced即可
- 可以通过{服务名称}.ribbon.NFLoadBalancerRuleClassName配置负载均衡策略。

##### （2）Feign

- 服务消费者引入spring-cloud-starter-openfeign依赖
- 通过@FeignClient声明一个调用远程微服务接口
- 启动类上通过@EnabledFeignClients激活Feign

### P51、高并发问题：使用Jmeter模拟高负载存在的问题

#### 4、微服务架构的高并发问题

#### 4.1 性能工具Jmetter

##### 4.1.1 安装Jmetter

apache-jmeter.2.13.zip

##### 4.1.2 配置Jemetter

#### 4.2 系统负载过高存在的问题

##### 4.2.1 问题分析

##### 4.2.2 线程池的形式实现服务隔离

### P52、高并发问题：问题分析

Tomcat会以线程池的形式对所有的请求进行统一的管理，对于某个方法可能存在的耗时问题的时候，随着外边积压的请求越来越多，势必会造成系统的崩溃，

为了一个方法不影响其他方法API接口的访问：可以对多个服务之间进行隔离

1 线程池隔离

2 信号量隔离（计数器，最大阈值 如5）

### P53、高并发问题：线程池隔离的方式处理请求积压问题
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
    @overrride
    protected Product getFallback(){
        return null;
        Product product = new Product();
        product.setName("不好意思出错了。。。");
        return product;
    }
}
```



### P54、高并发问题：服务容错的核心知识

#### 5、服务熔断Hstrix入门

##### 5.1 服务容错的核心知识

##### 5.1.1 雪崩效应

在微服务架构中，一个请求需要调用多个微服务的非常常见的，如客户端访问A服务，而A服务需要调用B服务，B服务调用C服务，由于网络原因或自身的原因，如果B服务或C服务不能及时响应，A服务就处于阻塞状态，直到B服务或C服务响应，此时如果有大量的请求涌入，容器的线程资源就会被消耗完毕，导致系统瘫痪。服务于服务之间的依赖性，故障会传播，造成连锁反应，会对整个未付系统造成灾难性的严重后果，这就是微服务故障的“雪崩”效应。

##### 5.1.2 服务隔离

顾名思义，它是指将系统安装一定的原则划分为若干个服务模块，各个模块之间相对独立，无强依赖。当有故障发生时，能将问题和影响隔离在某个模块内部，而不扩散风险，不波及其他模块，不影响整体的系统服务。

##### 5.1.3 熔断降级

熔断这一概念来源于电子工程中的断路器（Circuit Breaker），在互联网系统中，当下游服务因访问压力过大而想要变慢，或失败，上游服务为了保护整个系统的可用性，可以暂时切断对下游服务的调用，这种牺牲局部，保护整体的措施叫熔断。

##### 5.1.4 服务限流

### P55、基于RestTemplate的熔断配置

#### 5.2 Hystrix介绍

Hystrix是Netflix开源的一个延迟和容错库，用于隔离访问远程系统，服务或者第三方库，防止级联失败，从而提升系统的可用性与容错性，Hystrix主要通过以下几点实现延迟和容错

- 包裹请求：使用HystrixCommand包裹对依赖的调用逻辑，每个命令在肚里线程中执行，这使用了设计模式中的命令模式
- 跳闸机制：当某个服务的错误率超过一定的阈值时，Hystrix可以自动或手动跳闸，停止请求该服务一段时间。
- 资源隔离：Hystrix为每个依赖都维护了一个小型线程池（或信号量）。如果线程池已满，发往该依赖的请求就会被立即拒绝，而不是排队等待，从而加速失败判定。
- 监控：Hystrix可以近乎实时的监控运行指标和配置的变化，例如成功、失败、超时、以及被拒绝的请求等。
- 回退机制：当请求失败、超时、被拒绝、或者当断路器打开是，执行回退逻辑，回退逻辑由开发人员自行提供，例如返回一个缺省值。
- 自我修复：断路器打开一段时间后，会自动进入“半开”状态。

#### 5.3 RestTemplate实现服务熔断

引入hystrix的先关依赖

启动类中激活Hystrix

配置熔断触发的降级逻辑

在需要受到保护的接口上使用@HystrixCommand注解



#### 5.4 Feign 实现服务熔断

### 6 服务熔断Hystrix高级

### 
### P56、
### P57、
### P58、

### P75、 微服务网关

#### 1.1 微服务网关的概念

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



### P76、Nginx模拟微服务网关

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



### P78、Zuul网关

#### 1.1 Zuul简介

Zuul是Netflix开源的微服务网关，它可以和Eureka、Ribbon、Hystrix等组件配合使用，Zuul组件的核心是一系列的过滤器 ，这些过滤器可以完成以下功能：

- **动态路由：**动态将请求路由到不同后端集群

- **压力测试：**主键增加指向集群的流量，以了解性能

- **负载分配：**为每一种负载类型分配对应容量，并启用超出限定值的请求

- **静态响应处理：**编译位置进行响应，避免转发到内部集群

- **身份认证和安全：** 识别每一个资源的验证要求，并拒绝那些不符的请求。

SpingCloud对Zuul进行了整合和增强



#### 1.2 搭建zuul网关服务器

1、创建工程 导入pom坐标

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
        </dependency>
```

2、配置启动类 开启网关服务器功能

```java
@SpringBootApplication
@EnableZuulProxy  //而非@EnableZuulServer
public class ZuulServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ZuulServerApplication.class, args);
    }
}
```

3、配置文件

```yml
server:
  port: 8080
spring:
  application:
    name: api-zuul-server

```

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



#### 1.4  Zuul中的过滤器

##### 1.4.1 ZuulFilter简介

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
    * 是否使用过滤器：true 使用; false 不使用
    */
    public boolean shouldFilter(){
        return true;
    }
    /**
    *
    *指定过滤器中的业务逻辑
    */
    public Object run() throws ZuulException{
        System.out.pringln("执行了过滤器");
        return null;
    }
}
```

使用最多的是pre过滤器，用于做身份验证

示例：认证客户端token

1\ 所有的请求都协大一个参数 acccess-toker

2\  获取request请求

3、通过request获取参数access-token

4、判断token是否合法

在zuul网关中 通过RequestContext的上下文对象，可以获取request，（Zuul是 通过threadLocal实现）

```java

```

#### 1.5  Zuul内部源码



### P79、SpringCloud gateway网关





### P85、SpringCloud GateWay概述

Zuul存在的问题

- 不支持长连接 如websocket

- 基于阻塞IO的API GetWay

Spring Cloud GateWay



#### SpringCloud gataway 网关

#### 路由配置

##### 路由Route

##### 断言

##### 过滤器 GatewayFilter GlobolFilter

API-gateway-server

#### 过滤器

springcloudgateway 的内部是通过netty+webfex 实现的

webflex会和springmvc web冲突，解决方法是不要在父工程中引入web的依赖

断言条件：

Before

Ofter

Between

Path

#### 3.3.3 动态路由/面向服务的路由

1、引入依赖Eureka-client依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2、启动类中开启路由发现功能

```java
@EnableEurekaClient
@EnableDiscoveryClient
```

3、yml配置文件中 进行配置

```yaml
spring:  
  cloud:
    gateway:
      routes:
        - id: product-service #路由id 保持唯一即可
          #uri: http://127.0.0.1:9001 #目标服务器IP地址
          uri: lb:service-product #lb://根据微服务名称从注册中心拉取服务请求路径
          predicates:
            - Path=/product/*  #路由条件 path：路由匹配条件
            - 
            
```

#### 2.1.4 类似Zuul带前缀的访问规则

```yml
- Path=/product/*
- Path=/product-service/** 
filters: # 配置一个路由过滤器
- Rewritepath=/product-service/(?<segment>.*),/$\{segment} #路径重写过滤器
 # 将中间部分重写为空 
 # yml中$ 要携程$\ 进行转移
```

#### 2.1.5 开启微服务名称的转发规则

```yaml
spring:  
  cloud:
    gateway:
      routes:
      
      #配置自动根据微服务名称进行路由转发
      discovery:
        locator:
          enable: true #开启根据微服务名称自动转发
          lower-case-service-id: true #微服务名称以小写形式呈现
```

#### 2.2 过滤器

SpringCloudGateway过滤器没有Zuul那么丰富，它只有两个pre和post

过滤器类型

GatewayFilter 局部过滤器 针对某一个路由有效的路由配置

GlobalFilter 全局路由配置 应用到   如：身份认证，权限校验



#### 2.3 自定义全局过滤器

定义了GlobalFilter接口，用户实现GlobalFilter接口，就可以实现自定义的过滤器

```
filter.LoginFilter
#自定义全局过滤器
```

```java

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



#### 3.4 统一鉴权

```java

@Component
public class LoginFilter implements GlobalFilter, Ordered {

    /**
     * 执行过滤器中的业务逻辑
     *  对请求参数中的access-token 如果存在此参数代表认证成功
     *  不存在这个参数 认证失败
     *
     *  ServerWebExchange 相当于请求和响应的上下文 类似Zuul中的RequestContext 
     * @param exchange
     * @param chain
     * @return
     */
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        System.out.println("执行了自定义的全局过滤器");
        //1获取access-toker
        String token = exchange.getRequest().getQueryParams().getFirst("access-toen");      //请请参数
        
        //2判断是否存
        if(token==null){
            //3 如果不存在认证失败
            System.out.println("认证失败");
            exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
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



#### 3.5 网关限流

##### 3.5.1 常见的限流算法

###### 1 计数器算法

单位时间：每分钟  阈值：单位时间内最大的请求数量

计数器算法缺点：

流量现在不够平滑 

###### 2 漏桶算法

相当于在网关侧维护一块内存，相当于一个队列，请求都发送到网关，网关到微服务 可以设置队列的输出频率

漏桶溢出，那么请求数据包就会丢失

队列大小=漏桶的容量

输出频率

如果设置不当，可能导致网关宕机，漏桶算法主要是为了爆出别人的

###### 3 令牌桶算法

请求必须拿到令牌才可以访问，如果拿不到令牌或者令牌桶中没有可用的令牌，要么等待，要么访问被拒绝



请求过来之后，必须先拿到令牌，获取到了令牌

令牌桶算法主要是为了保护自己，网关挂了，所有请求都进不了，所有微服务中通常使用令牌桶算法

##### 2.4.1 基于filter的限流

cloud官方提供了令牌桶算法进行限流

RequestRateLimiterGatewayFilterFacory实现

1、准备工作

redis

```
monitor 命令监控redis 数据变化
```

工程中引入redis相应的依赖

```xml
    <!-- 监控依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <!--  -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
```

2、修改网关中的application.yml配置

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
          - name: RequestRateLimiter
            args:
              # 使用SpEL从容器中获取对象
              key-resolver: '#{@pathKeyResolver}'
              # 令牌桶每秒填充的评价速率
              redis-rate-limiter.replenishRate: 1
              # 令牌桶的上线
              redis-rate-limiter.burstCapacity:3
          - RewritePath=/product-service/(?<segment>.*),/$\{segment}
eureka:
  client:
    service-url:
      defaultZone: http://localhost:9000/eureka
    instance:
      prefer-ip-address: true #使用ip地址注册
```



3、配置一个redis 中key的解析器KeySesolver

```java
pathKeyResolver
```





##### 2.4.2 基于Sentinel的限流

#### 3.6 网关的高可用



