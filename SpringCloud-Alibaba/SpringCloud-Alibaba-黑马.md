> https://www.bilibili.com/video/BV1Nf4y1d7KN
>
> 黑马-4天

### 从浅入深精通SpringCloud 微服务架构



### P30 Sentinel 服务容错



### P34 服务网关

当前架构下的问题：

1、客户端需要维护服务器的各个地址 代码困难

2、认证鉴权

3、存在跨域请求

#####  什么是API网关

所谓API网关，就是指系统的统一入口，它封装了应用程序的内部结构，为客户端提供统一服务、一些与业务本身功能无关的公共逻辑可言在这里实现，  诸如：认证、鉴权、监控、路由

在业界比较流行的网关，有下面这些

- ##### Nginx+lua

  使用Nginx

- ##### Kong

  基于Nginx+lua开发，性能高，稳定，有多个可用的插件（限流、鉴权）

- ##### Zuul

  Netflix开源的网关，功能丰富，使用Java开发，易于二次开发

  问题：缺乏管控，无法动态配置；依赖组件较多，处理Http请求以来的是web容器

- ##### Spring Cloud GateWay

  Spring公司为了替换Zuul而开发的网关服务，将在下面具体介绍。

注意：SpingCloud Alibaba是没有网关组件的，使用的是SpingCloud





#### P35 SpringCloud Gateway



#### 5.2 Gateway简介

SpringCloud Gateway是Spring公司 



优点：

- 性能强劲：是第一台网关Zuul的1.6倍
- 功能强大
- 设计优雅，易于扩展

##### 缺点：

- 其 实现依赖Netty与WebFlux，不是传统的Servlet编程模型，学习成本高
- 不能将其部署在Tomcat，Jetty等Servlet容器里，只能打成jar包运行
- 需要SpringBoot2.0 以上的版本，才能够支持

### 5.3 Gateway快速入门

#### 5.3.1  基础版

第一步：创建一个api-gateway的模块，并导入相关依赖 pom.xml

千万不要导入springboot-starter-web

```xml

<denpency>
	 <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</denpency>
```

**第二步：** 创建主启动类 ApiGatewayApplication

```java
@SpringBootApplication
public class ApiGatewayApplication{
    public static void main(String[] args){
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}
```

**第三步** 创建配置文件application.properties

```yml
server:
  port: 7000
spring:
  application:
    name: api-gatewway
cloud:
  gateway:
    routes:
      - id: product-service#当前路由的表示，默认是uuid
        uri: http://localhost:8081 #请求转发的地址
        order: 1 #路由的优先级，数字越小优先级越高
        predicates:  #断言 条件判断 返回值是boolean类型，转发请求要满足的条件
          - Path=/product-service/** #当请求路径满足Path指定的规则是，才会被转发
        filters:  #过滤器 在请求传递过程中 对请求做一些手脚 加入header 
          - StriipPrefix=1  #在请求转发之前去掉一层路径
          
```

#### 5.3.2 增强版

**第一步** 配置文件中加入nacos依赖

```xml
<denpency>
	 <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</denpency>
```

**第二步** 主程序加入注解

```java
@SpringBootApplication
@EnableDiscoveryClient
public class ApiGatewayApplication{
    public static void main(String[] args){
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}
```



**第三步** 将微服务地址改为从 nacos中获取

```yaml
server:
  port: 7000
spring:
  application:
    name: api-gatewway
cloud:
  nacos:
    discovery:
      server-addr: localhost:8848 #将gateway注册到nacos
  gateway:
    discovery:
      locator:
        enabled: true # 让gateway从nacos获取信息
    routes:
      - id: product-service#当前路由的表示，默认是uuid
        #uri: http://localhost:8081 #请求转发的地址
        uri: lb://service-product #lb指的是负载均衡，后面跟的是具体微服务在nacos中的标识        
        order: 1 #路由的优先级，数字越小优先级越高
        predicates:  #断言 条件判断 返回值是boolean类型，转发请求要满足的条件
          - Path=/product-service/** #当请求路径满足Path指定的规则是，才会被转发
        filters:  #过滤器 在请求传递过程中 对请求做一些手脚 加入header 
          - StripPrefix=1  #在请求转发之前去掉一层路径
          
```

#### 5.3.3 简写版

不写路由，routes：及其下面的节点 全部注释或者删掉 使用默认的路由规则实现

使用默认的微服务的spring application命名

```yaml
server:
  port: 7000
spring:
  application:
    name: api-gatewway
cloud:
  nacos:
    discovery:
      server-addr: localhost:8848 #将gateway注册到nacos
  gateway:
    discovery:
      locator:
        enabled: true # 让gateway从nacos获取信息
#    routes:
#      - id: product-service#当前路由的表示，默认是uuid
#        #uri: http://localhost:8081 #请求转发的地址
#        uri: lb://service-product #lb指的是负载均衡，后面跟的是具体微服务在nacos中的标识        
#        order: 1 #路由的优先级，数字越小优先级越高
#        predicates:  #断言 条件判断 返回值是boolean类型，转发请求要满足的条件
#          - Path=/product-service/** #当请求路径满足Path指定的规则是，才会被转发
#        filters:  #过滤器 在请求传递过程中 对请求做一些手脚 加入header 
#          - StriipPrefix=1  #在请求转发之前去掉一层路径
```

### 5.4 Gateway核心架构

路由Route是gateway中最基本的组件之一，标识一个具体的路由信息载体，主要定义了下面的几个信息：

- ##### id ,路由标识符 区别其他路由

- **uri，** 路由指定的目的地uri 即客户端请求最终被转发的微服务

- **order,**

- **predicate,**

- **filter,**