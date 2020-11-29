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

#### 什么是Feign

Feign是 声明式



#### Feigin入门实例环境搭建

##### 1、导入依赖

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <!--<artifactId>spring-cloud-starter-netflix-feign</artifactId>-->
        <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>
```

##### 2、配置调用接口

```java
@FeignClient(name="service-product")
public interface ProductFeighClient {

    /**
     * 配置需要调用的微服务接口
     */
    @RequestMapping(value = "/product/{id}",method = RequestMethod.GET)
    public Product findById(@PathVariable("id")Long id);
}
```

##### 3、启动类上 激活Feign

```java
@EntityScan("com.beyondsoft.intelsecurity.model")
@EnableFeignClients
public class OrderServerApplication{
    
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

### P46、

### P47、
### P48、
### P49、
### P50、
### P51、
### P52、
### P53、
### P54、
### P55、
### P56、
### P57、
### P58、

### P75、 微服务网关

Nginx+Lua 非微服务环境下可以使用

Zuul 

SpringC



### P76、Nginx模拟微服务网关

准备多个Eureka-Server



### P78、Zuul网关

1.1 介绍

核心是一系列的过滤器 ，这些过滤器完成以下功能：

动态路由

压力测试

负载分配



##### 1.2 搭建zuul网关服务器

1、创建工程 导pom坐标

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
        </dependency>
```

2、配置启动类 开启网关服务器功能 zuul-server

```java
@SpringBootApplication
@EnableZuulProxy  //而非@EnableZuulServer
public class ZuulServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ZuulServerApplication.class, args);
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

##### 1.3  路由

1.3.1 基础路由配置

1.3.2 面向服务路由配置

1、Zuul与Eureka整合 添加 Eureka的依赖

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

2、开启Eureka的客户端服务发现

```
@SpringBootApplication
@EnableZuulProxy
//@EnableEurekaClient
@EnableDiscoveryClient
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
    prefer-ip-address: true  #使用ip地址

```

4、 修改路由中的映射配置

```yml
    system-server:
      path: /system-service/**
      #url: http://127.0.0.1:8086
      serviceId: system-service  # 配置转发的微服务的服务名称
```



1.3.3 简化路由配置

如果路由id 与service服务名称一致的话

```
service-product: /product-service/**
```

如果

##### 1.4  过滤器

请求的校验、服务的聚合

ZuulFilter 与Servlet不太一样

四种类型

- ###### PRE Filter

  转发到无服务之前执行请求之前，过滤，对身份信息进行识别校验

- ###### Routing Filter

  负载均衡

- ###### POST Filter

  添加请求头Header、收集日志等

- ###### Error Filter

  其他阶段错误 抛出异常时 执行的过滤器

自定义过滤器，实现ZuulFilter抽象类

##### 1.5  内部源码

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



