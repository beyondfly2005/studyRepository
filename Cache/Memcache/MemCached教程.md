## Memcache教程

## 一、Memcache简介

#### 1.1 概念

##### 1.1.1 Memcache

什么是Memcache？

Memcache集群环境下缓存解决方案

Memcache是一个高性能的分布式的内存对象缓存系统，通过在内存里维护一个统一的巨大的hash表，它能够用来存储各种格式的数据，包括图像、视频、文件以及[数据库](http://lib.csdn.net/base/mysql)检索的结果等。简单的说就是将数据调用到内存中，然后从内存中读取，从而大大提高读取速度。　　

Memcache是danga的一个项目，最早是LiveJournal 服务的，最初为了加速 LiveJournal 访问速度而开发的，后来被很多大型的网站采用。　　

Memcached是以守护程序方式运行于一个或多个服务器中，随时会接收客户端的连接和操作

##### 1.1.2 Memcached

Memcached是一个自由开源的，高性能，分布式内存对象缓存系统。

Memcached是以LiveJournal旗下Danga Interactive公司的Brad Fitzpatric为首开发的一款软件。现在已成为mixi、hatena、Facebook、Vox、LiveJournal等众多服务中提高Web应用扩展性的重要因素。

Memcached是一种基于内存的key-value存储，用来存储小块的任意数据（字符串、对象）。这些数据可以是数据库调用、API调用或者是页面渲染的结果。

Memcached简洁而强大。它的简洁设计便于快速开发，减轻开发难度，解决了大数据量缓存的很多问题。它的API兼容大部分流行的开发语言。

本质上，它是一个简洁的key-value存储系统。

一般的使用目的是，通过缓存数据库查询结果，减少数据库访问次数，以提高动态Web应用的速度、提高可扩展性。

Memcached 官网：https://memcached.org/

##### 1.1.3 Memcache和memcached

为什么会有Memcache和memcached两种名称？

其实Memcache是这个项目的名称，而memcached是它服务器端的主程序文件名，知道我的意思了吧。一个是项目名称，一个是主程序文件名，在网上看到了很多人不明白，于是混用了。

#### 1.2 特征

emcached作为高速运行的分布式缓存服务器，具有以下的特点。

- 协议简单
- 基于libevent的事件处理
- 内置内存存储方式
- memcached不互相通信的分布式

#### 1.3 支持的语言

许多语言都实现了连接memcached的客户端，其中以Perl、PHP为主。仅仅memcached网站上列出的有：

- Perl
- PHP
- Python
- Ruby
- C#
- C/C++
- Lua
- 等等

#### 1.4 Memcached 用户

- LiveJournal
- Wikipedia
- Flickr
- Bebo
- Twitter
- Typepad
- Yellowbot
- Youtube
- WordPress.com
- Craigslist
- Mixi

## 二、Memcache的安装

#### 2.1 Window Memcached安装

##### 2.1.1 Memcached 安装

　　32位请下载这里： 链接: https://pan.baidu.com/s/1qPxMDPEtsCFBWaG3bsVT6Q 密码: hrih

　　下载之后解压就行。

　　64位先下载32位，进行解压，接着下载这里：链接: https://pan.baidu.com/s/1X3MeLgB5QObksm35LKZ-eg 密码: xbn4 

　　解压之后，把里面的三个文件复制到32位的里面，覆盖即可。

　　我解压之后放在E盘：

![img](https://images2018.cnblogs.com/blog/1415794/201809/1415794-20180910183432444-899872071.png)

　　使用管理员权限运行以下命令

　　E:\memcache\memcached.exe -d install

##### 2.1.2 启动关闭卸载memcache

　　启动： E:\memcache\memcached.exe -d start

　　关闭： E:\memcache\memcached.exe -d stop

　　卸载： E:\memcache\memcached.exe -d uninstall



#### 2.2 Linux Memcached 安装

Linux系统安装memcached，首先要先安装libevent库。

```bash
sudo apt-get install libevent ibevent-dev         自动下载安装（Ubuntu/Debian）

yum install libevent libevent-devel                    自动下载安装（Redhat/Fedora/Centos）
```

##### **自动安装方式 Centos**

```
yum install memcached
```

##### 源代码安装

```bash
wget http://memcached.org/latest                   # 下载最新版本

tar -zxvf memcached-1.x.x.tar.gz                   # 解压源码

cd memcached-1.x.x                                 # 进入目录

./configure --prefix=/usr/local/memcached          # 配置

make && make test                                  # 编译

sudo make install                                  # 安装
```



##### Memcached 运行

```bash
$ /usr/local/memcached/bin/memcached -h                          # 命令帮助
```



注意：如果使用自动安装 memcached 命令位于 **/usr/local/bin/memcached**。

**启动选项：**

- -d是启动一个守护进程；
- -m是分配给Memcache使用的内存数量，单位是MB；
- -u是运行Memcache的用户；
- -l是监听的服务器IP地址，可以有多个地址；
- -p是设置Memcache监听的端口，，最好是1024以上的端口；
- -c是最大运行的并发连接数，默认是1024；
- -P是设置保存Memcache的pid文件。

##### 以前台程序运行

从终端输入以下命令，启动memcached:

```bash
/usr/local/memcached/bin/memcached -p 11211 -m 64m -vv
```

##### 以后台服务程序运行

```bash
# /usr/local/memcached/bin/memcached -p 11211 -m 64m -d
```

或者

```bash
/usr/local/memcached/bin/memcached -d -m 64M -u root -l 192.168.0.200 -p 11211 -c 256 -P /tmp/memcached.pid
```

#### 2.3 Docker方式安装

##### 2.3.1 拉取镜像

```bash
docker pull memcached
```

##### 2.3.2 启动容器

```bash
docker run -p 11211:11211 --name my-memcache -d memcached
```



## 三、Memcache客户端

##### **Memcache客户端**

Memcache Clinet 目前支持3种。

- Memcached Client for Java停止更新
- SpyMemcached 停止更新
- XMemcached 主流

##### **XMemcached介绍**

  是使用最为广泛的Memcache Java客户端，是一个全新的 Java Memcache Client。Memcache 通过它自定义协议与客户端交互，而XMemcached就是它的Java客户端实现，相比较于其他客户端来说XMemcached 的优点如下

##### **XMemcached主要特性**

   XMemcached 支持设置连接池、宕机报警、使用二进制文件、一致性Hash、进行数据压缩等操作，总结下来有以下一些点

- 性能优势，使用的NIO
- 协议支持广泛
- 支持客户端分布，提供了一致性Hash 实现
- 允许设置节点权重，XMemcached允许通过设置节点的权重来调节Memcached的负载，设置的权重越高，该Memcached节点存储的数据就越多，所要承受的负载越大
- 动态的增删节点，Memcached允许通过JMX或者代码编程来实现节点的动态的添加或者删除操作。方便扩展或者替换节点。
- XMemcached 通过 JMX 暴露的一些接口，支持Client本身的监控和调整，允许动态设置调优参数、查看统计数据、动态增删节点等；
- 支持链接池操作。
- 可扩展性强，XMemcached是基于Java NIO框架 Yanf4j 来实现的，所以在结构上相对清晰，分层明确。

## 四、Memcache与SpringBoot集成

SpringBoot对于Memcached的支持是最差的，并不是原生支持。

由于Memcache有三种客户端 所以集成方式也略有不同

### 方式一： 以XMemcached 作为客户端

#### 4.1.1 引入pom依赖

添加关于memcache的客户端 XMemcached 的依赖

```xml
<dependency>
     <groupId>com.googlecode.xmemcached</groupId>
     <artifactId>xmemcached</artifactId>
     <version>2.4.5</version>
 </dependency>
```

#### 4.1.2 配置文件读取类

在添加配置文件的时候，由于SpringBoot默认没有支持memcache的自动配置，所以需要使用SpringBoot提供的配置文件机制来编写自己的配置文件。

**构建MemcachedProperties 首先编写一个配置文件的Properties对象用来与配置文件进行映射**

```java
/**
 * @Classname XMemcachedProperties
 * @Description TODO
 */
@Component
@ConfigurationProperties(prefix = "memcache")
public class MemcachedProperties {
    private String servers;
    private int poolSize;
    private long opTimeout;
    private boolean sanitizeKeys;

    public String getServers() {
        return servers;
    }

    public void setServers(String servers) {
        this.servers = servers;
    }

    public int getPoolSize() {
        return poolSize;
    }

    public void setPoolSize(int poolSize) {
        this.poolSize = poolSize;
    }

    public long getOpTimeout() {
        return opTimeout;
    }

    public void setOpTimeout(long opTimeout) {
        this.opTimeout = opTimeout;
    }
}

```

#### 4.1.3 **编写Memcache的配置类**

**使用@Configration注解进行标注表明这个是一个配置类**

```java
@Configuration
public class MemcachedConfig {
    protected static Logger logger = LoggerFactory.getLogger(MemcachedBuilder.class);

    @Resource
    private XMemcachedProperties xMemcachedProperties;

    @Bean
    public MemcachedClient getMemcachedClinet(){
        MemcachedClient memcachedClient = null;
        try {
            MemcachedClientBuilder builder = new XMemcachedClientBuilder(AddrUtil.getAddresses(xMemcachedProperties.getServers()));
            builder.setConnectionPoolSize(xMemcachedProperties.getPoolSize());
            builder.setOpTimeout(xMemcachedProperties.getOpTimeout());
            memcachedClient = builder.build();
        }catch (IOException e){
            logger.error("init MemcachedClient failed"+e);
        }
        return memcachedClient;
    }
}
```

#### 4.1.3 启动时加载客户端组件MemcacheClient 

如上代码在配置类中注入一个MemcacheClient的Bean对象，并且设置了一些基本的参数，而这些参数都是由配置文件来提供

```java
  @Bean
    public MemcachedClient getMemcachedClinet(){
        MemcachedClient memcachedClient = null;
        try {
            MemcachedClientBuilder builder = new XMemcachedClientBuilder(AddrUtil.getAddresses(xMemcachedProperties.getServers()));
            builder.setConnectionPoolSize(xMemcachedProperties.getPoolSize());
            builder.setOpTimeout(xMemcachedProperties.getOpTimeout());
            memcachedClient = builder.build();
        }catch (IOException e){
            logger.error("init MemcachedClient failed"+e);
        }
        return memcachedClient;
    }
```

#### 4.1.4 编写配置文件

根据上面配置类的内容，可以知道需要配置如下的三个参数,服务地址、线程池大小、超时时间，当然为了高可用的话还可以将服务地址设置成一个数组。这里是测试就先不用高可用。

```properties
#缓存机制配置  @ConfigurationProperties(prefix = "memcache")
memcached.servers=10.2.116.178:11111
memcached.poolSize=10
memcached.opTimeout=6000


#缓存机制配置 @ConfigurationProperties(prefix = "memcached")
memcache.server=127.0.0.1:11211
memcache.poolSize=20
memcache.sanitizeKeys=false
memcache.opTimeout=3000
```

#### 4.1.5 使用MemcachedClient操作memcached

```java
@RestController
public class HelloController {
    private Logger logger = LoggerFactory.getLogger(this.getClass());
    
    @Autowired
    private MemcachedClient memCachedClient;
    @ResponseBody
    @RequestMapping("/get")
    public String get(){
        try {
            memCachedClient.set("access_token", 3600, "aaaaaaaaaaaaa");
            String access_token = memCachedClient.get("access_token");
            logger.info("从memcached当中获取到的值是："+access_token);
        }catch (Exception e){
            logger.info("发生了异常，异常信息是："+e.getMessage());
        }
        return "success";
    }
}
```

 

### 方式二：以SpyMemcached 作为客户端

#### 4.2.1 引入pom依赖

引入SpyMemcached 客户端依赖

```xml
<!-- spymemcached -->
<dependency>
  <groupId>net.spy</groupId>
  <artifactId>spymemcached</artifactId>
  <version>2.12.3</version>
</dependency>
```

#### 4.2.2 配置文件读取类

```java
@Component
@ConfigurationProperties(prefix = "memcache")
public class MemcacheSource {

    private String ip;

    private int port;

    public String getIp() {
        return ip;
    }

    public void setIp(String ip) {
        this.ip = ip;
    }

    public int getPort() {
        return port;
    }

    public void setPort(int port) {
        this.port = port;
    }
}
```

#### 4.2.3 编写配置文件

```properties
memcache.ip=192.168.0.161
memcache.port=11211
```

#### 4.2.4 启动时初始化MemcachedClient

```java
@Component
public class MemcachedRunner implements CommandLineRunner {
    protected Logger logger =  LoggerFactory.getLogger(this.getClass());

    @Resource
    private  MemcacheSource memcacheSource;

    private MemcachedClient client = null;

    @Override
    public void run(String... args) throws Exception {
        try {
            client = new MemcachedClient(new InetSocketAddress(memcacheSource.getIp(),memcacheSource.getPort()));
        } catch (IOException e) {
            logger.error("inint MemcachedClient failed ",e);
        }
    }

    public MemcachedClient getClient() {
        return client;
    }

}
```

#### 4.2.5 控制层测试类

```java
@RestController
public class DemoController{
    @Resource
    private MemcacheRunner memcacheRunner;
    
    @RequestMapping("/set")
    public void set(String key,String value){
        memcacheRunner.getClient().set(key,100,value);
    }
    @RequestMapping("/get")
    public String set(String key){
        return memcacheRunner.getClient().get(key).toString();
    }
}
    
```

#### 4.2.6 测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class RepositoryTests {

  @Resource
    private MemcachedRunner memcachedRunner;

  @Test
  public void testSetGet()  {
    MemcachedClient memcachedClient = memcachedRunner.getClient();
    memcachedClient.set("testkey",1000,"666666");  //1000 为过期时间单位为 毫秒
    System.out.println("***********  "+memcachedClient.get("testkey").toString());
  }
}
```

#### 4.2.7 启动类

省略...

### 方式三：以Memcached Client for Java作为客户端

#### 4.3.1 引入pom依赖

```xml
    <!--memcache-->
    <dependency>
        <groupId>commons-pool</groupId>
        <artifactId>commons-pool</artifactId>
        <version>1.5.6</version>
    </dependency>
    <dependency>
        <groupId>com.whalin</groupId>
        <artifactId>Memcached-Java-Client</artifactId>
        <version>3.0.2</version>
    </dependency>
```

#### 4.3.2 配置文件读取类

MemcacheConfiguration

```java
@Configuration
public class MemcacheConfiguration {

    @Value("${memcache.servers}")
    private String[] servers;
    @Value("${memcache.failover}")
    private boolean failover;
    @Value("${memcache.initConn}")
    private int initConn;
    @Value("${memcache.minConn}")
    private int minConn;
    @Value("${memcache.maxConn}")
    private int maxConn;
    @Value("${memcache.maintSleep}")
    private int maintSleep;
    @Value("${memcache.nagle}")
    private boolean nagel;
    @Value("${memcache.socketTO}")
    private int socketTO;
    @Value("${memcache.aliveCheck}")
    private boolean aliveCheck;

    @Bean
    public SockIOPool sockIOPool () {
        SockIOPool pool = SockIOPool.getInstance();
        pool.setServers(servers);
        pool.setFailover(failover);
        pool.setInitConn(initConn);
        pool.setMinConn(minConn);
        pool.setMaxConn(maxConn);
        pool.setMaintSleep(maintSleep);
        pool.setNagle(nagel);
        pool.setSocketTO(socketTO);
        pool.setAliveCheck(aliveCheck);
        pool.initialize();
        return pool;
    }

    @Bean
    public MemCachedClient memCachedClient(){
        return new MemCachedClient();
    }

}
```

#### 4.3.3 配置文件

```properties
memcache.servers=127.0.0.1:11211
memcache.failover=true
memcache.initConn=10
memcache.minConn=20
memcache.maxConn=1000
memcache.maintSleep=50
memcache.nagle=false
memcache.socketTO=3000
memcache.aliveCheck=true
```

#### 4.3.4 控制层测试类

MemcacheController

```java
@Controller
public class MemcacheController {

    @Autowired
    private MemCachedClient memCachedClient;

    /**
     * memcache缓存
     */
    @RequestMapping("/memcache")
    @ResponseBody
    public String memcache() throws InterruptedException{
        // 放入缓存
        boolean flag = memCachedClient.set("mem", "name");
        // 取出缓存
        Object value = memCachedClient.get("mem");
        System.out.println(value);
        // 3s后过期
        memCachedClient.set("num", "666", new Date(3000));
        /*memCachedClient.set("num", "666", new Date(System.currentTimeMillis()+3000));与上面的区别是当设置了这个时间点
        之后，它会以服务端的时间为准，也就是说如果本地客户端的时间跟服务端的时间有差值，这个值就会出现问题。
        例：如果本地时间是20:00:00,服务端时间为20:00:10，那么实际上会是40秒之后这个缓存key失效*/
        Object key = memCachedClient.get("num");
        System.out.println(key);
        //多线程睡眠3s
        Thread.sleep(3000);
        key  = memCachedClient.get("num");
        System.out.println(value);
        System.out.println(key );
        return "success";
    }
}
```

####  