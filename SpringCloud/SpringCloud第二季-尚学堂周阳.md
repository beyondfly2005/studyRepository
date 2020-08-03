`SpringCloud中文文档

官方文档

https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/

中文文档

http://www.bookstack.cn/read/spring-cloud-docs/docs-index.md

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

### Eureka

#### Eureka基础知识

```bash
Eureka
Zookeeper
Consul
Nacos

服务注册中心
服务注册于发现
1- Eureka 基础知识
什么是服务质量SpringCloud封装了NetFlix公司开发的Eureka模块来实现服务治理
在传统的RPC远程调用框架中，管理每个服务于服务之间的依赖关系比较复杂，，所以需要使用服务治理，用于管理服务之间的依赖关系，可以实现服务调用、负载均衡、容错等，实现服务注册于发现

2-什么是服务注册
Eureka采用了CS的设计架构，EurekaServer作为服务注册功能的服务器，它是服务注册中心
而系统中的其他微服务，使用Eureka的客户端连接到Eureka Server并

3-Eureka的两个组件：Eureka Server和EurekaCliend
Eureka Server提供注册服务
Eureak Client 用于连接到服务

4-






```

#### Eureka停更

Eureka（Discontinued） 停更

```bash
http://github.com/Netflix/eureka/wiki
```

### Zookeeper

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

  

### Consul

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



### Ribbon 客户端负载均衡工具

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

<dependency>

​	<groupId>org.springframework.cloud</groupId>

​    <artifactId> spring-cloud-starter-netflix-ribbon</artifactId>

</dependency>

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

### Feign和OpenFeign

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