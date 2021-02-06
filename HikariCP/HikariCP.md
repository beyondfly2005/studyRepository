# HikariCP 数据库连接池 

### HikariCP介绍



#### **什么是数据库连接池：**

  连接池是一种常用的技术，为什么需要连接池呢？这个需要从TCP说起。假如我们的服务器跟数据库没有部署在同一台机器，那么，服务器每次查询数据库都要先建立连接，一般都是TCP链接，建立连接就需要3次握手了，假设后台服务跟数据库的单程的访问时间需要10ms，那么光是建立连接就花了30ms，并且TCP还有慢启动的机制，实际上一次查询可能还不止1次TCP来回，查询效率就会大大降低。



#### **为什么需要连接池：**

  为了解决上述问题，我们就需要维护一些长链接，这样就不用每次都去建立连接，毕竟建立连接除了占用时间，还需要一些其他的系统资源。另外的好处，连接池让我们更加容易地管理，一方面是可以避免数据库资源都被某几个API占据，另一方面也可以避免资源泄露。



#### **什么是HikariCP**

HikariCP是由日本程序员开源的一个数据库连接池组件，代码非常轻量，并且速度非常的快。根据官方提供的数据，在i7,开启32个线程32个连接的情况下，进行随机数据库读写操作，HikariCP的速度是现在常用的C3P0数据库连接池的数百倍。在SpringBoot2.0中，官方也是推荐使用HikariCP。



#### **为什么HikariCP会那么快**

1.字节码更加精简，所以可以加载更多代码到缓存。

2.实现了一个无锁的集合类型，来减少并发造成的资源竞争。

3.使用了自定义的数组类型，相对与ArrayList极大地提升了性能。

4.针对CPU的时间片算法进行优化，尽可能在一个时间片里面完成各种操作。



#### **与Druid对比**

在github上有网友贴出了阿里巴巴Druid与hikari的对比，认为hikari在性能上是完全秒杀阿里巴巴的Druid连接池的。对此，阿里的工程师也做了一定的回应，说Druid的性能稍微差点是锁机制的不同，并且Druid提供了更丰富的功能，两者的侧重点不一样。

![img](https:////upload-images.jianshu.io/upload_images/2532461-ef38095695c2bed7.png?imageMogr2/auto-orient/strip|imageView2/2/w/679/format/webp)

**如何选择：**

  选择哪一款就见仁见智了，不过两款都是开源产品，阿里的Druid有中文的开源社区，交流起来更加方便，并且经过阿里多个系统的实验，想必也是非常的稳定，而Hikari是SpringBoot2.0默认的连接池，全世界使用范围也非常广，对于大部分业务来说，使用哪一款都是差不多的，毕竟性能瓶颈一般都不在连接池。大家可根据自己的喜好自由选择。

参考文档：https://www.jianshu.com/p/0633b319503c

### Springboot 2.0选择HikariCP作为默认数据库连接池

Springboot2默认数据库连接池选择了HikariCP为何选择HikariCP

- 理由一、代码量

- 理由二、口碑

- 理由三、速度

- 理由四、稳定性

- 理由五、可靠性



HikariCP为什么这么快

- 优化并精简字节码

- 更好的并发集合类实现

- 使用FastList替代ArrayList


HikariCP与Druid相比哪个更好



#### 为何选择Spring Boot 2默认数据库连接池选择了HikariCP

从SrpingBoot2.0开始 将默认的数据库连接池由Tomcat换成HikariCP. 

HiKariCP是数据库连接池的一个后起之秀，号称性能最好，可以完美地PK掉其他连接池，是一个高性能的JDBC连接池，基于BoneCP做了不少的改进和优化。其作者还有另外一个开源作品——高性能的JSON解析器HikariJSON。

它速度超快，代码体积超小，只有130kb。好到连Spring Boot 2都宣布支持了。

https://github.com/brettwooldridge/HikariJSON



#### 从BoneCP说起

HiKariCP是基于BoneCP的，并做了不少的改进和优化

已经有了C3P0/DBCP这些成熟的数据库连接池吗？为什么又搞出一个BoneCP来？

BoneCP在快速这个特点上做到了极致，官方数据是C3P0等的25倍左右。

![img](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-1.jpeg)

HikariCP的性能远高于c3p0、tomcat等连接池，但是，后来BoneCP作者都放弃了维护，在Github项目主页推荐大家使用HikariCP。另外，Spring Boot将在2.0版本中把HikariCP作为其默认的JDBC连接池。

这个产品的口号是“快速、简单、可靠”。实际情况跟这个口号真的匹配吗？又是有图有真相（Benchmarks又来了）：

[![img](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-3.jpeg)](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-3.jpeg)

这个图，也间接地、再一次地证明了boneCP比c3p0强大很多，当然，跟“光”比起来，又弱了不少啊。

#### 什么是Hikari？

Hikari来自日文，是“光”（阳光的光，不是光秃秃的光）的意思。作者估计是为了借助这个词来暗示这个CP速度飞快。据说作者是日本人



#### **对比BoneCP的优化**

HikariCP是怎么做到的呢？官网详细地说明了HikariCP所做的一些优化，总结如下：

- **字节码精简** ：优化代码，直到编译后的字节码最少，这样，CPU缓存可以加载更多的程序代码；
- **优化代理和拦截器**：减少代码，例如HikariCP的Statement proxy只有100行代码，只有BoneCP的十分之一；
- **自定义数组类型（FastStatementList）代替ArrayList**：避免每次get()调用都要进行range check，避免调用remove()时的从头到尾的扫描；
- **自定义集合类型（ConcurrentBag**：提高并发读写的效率；
- **其他针对BoneCP缺陷的优化**，比如对于耗时超过一个CPU时间片的方法调用的研究（但没说具体怎么优化）。



#### BoneCP的优点

##### 一、代码量

几个连接池的代码量对比（代码量越少，一般意味着执行效率越高、发生bug的可能性越低）

![img](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-4.png)

##### 二、口碑

Hikari，用户的好评如潮也是有图有真相

![img](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-5.jpeg)

##### 三、速度

还有第三方关于速度的测试

![img](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-6.jpeg)

##### 四、稳定性

也许你会说，速度高，如果不稳定也是硬伤啊。于是，关于稳定性的图也来了：

[![img](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-7.jpeg)](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-7.jpeg)

##### 五、可靠性

关于可靠性方面，也是有实验和数据支持的。对于数据库连接中断的情况，通过测试getConnection()，各种CP的不相同处理方法如下：
（所有CP都配置了跟connectionTimeout类似的参数为5秒钟）

- **HikariCP**：等待5秒钟后，如果连接还是没有恢复，则抛出一个SQLExceptions 异常；后续的getConnection()也是一样处理；
- **C3P0**：完全没有反应，没有提示，也不会在“CheckoutTimeout”配置的时长超时后有任何通知给调用者；然后等待2分钟后终于醒来了，返回一个error；
- **Tomcat**：返回一个connection，然后……调用者如果利用这个无效的connection执行SQL语句……结果可想而知；大约55秒之后终于醒来了，这时候的getConnection()终于可以返回一个error，但没有等待参数配置的5秒钟，而是立即返回error；
- **BoneCP**：跟Tomcat的处理方法一样；也是大约55秒之后才醒来，有了正常的反应，并且终于会等待5秒钟之后返回error了；

可见，HikariCP的处理方式是最合理的。根据这个测试结果，对于各个CP处理数据库中断的情况，评分如下：

[![img](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-8.jpeg)](https://blog.didispace.com/content/images/posts/Springboot-2-0-HikariCP-default-reason-8.jpeg)



### Springboot2快速上手

springboot 2.0 默认连接池就是Hikari了，所以引用parents后不用专门加依赖

置一下就可以用

```properties
# jdbc_config   datasource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/datebook?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&useSSL=false&zeroDateTimeBehavior=convertToNull
spring.datasource.username=root
spring.datasource.password=root

# Hikari will use the above plus the following to setup connection pooling
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.maximum-pool-size=15
spring.datasource.hikari.auto-commit=true
spring.datasource.hikari.idle-timeout=30000
spring.datasource.hikari.pool-name=DatebookHikariCP
spring.datasource.hikari.max-lifetime=1800000
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.connection-test-query=SELECT 1
```



参考资料：https://blog.didispace.com/Springboot-2-0-HikariCP-default-reason/