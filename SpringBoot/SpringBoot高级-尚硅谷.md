

# SpringBoot高级 / SpringBoot核心技术 后八章 

> 教程地址：https://www.bilibili.com/video/BV1KW411F7oX

## 八、SpringBoot自定义starters

# SpringBoot高级

## 九、SpringBoot与缓存

### P1 尚硅谷_缓存-JSR107简介

##### 本章内容

JSR-107缓存规范、Spring缓存抽象、整合Redis

##### 9.1 JSR 107规范

Java Caching定义了5个核心接口：CachingProvider，CacheManager, Cache，Entry，Expiry

- **CachingProvider** 定义了创建、配置、获取、管理和控制多个CacheManager，一个应用可以在运行期访问多个CacheManager
- **CacheManager**
- **Cache**
- **Entry**
- **Expiry**

JSR107规范用的比较少

### P2 尚硅谷_缓存-Spring缓存抽象简介

#### 9.2 Spring缓存抽象

Spring 从3.1开始定义了org.springframe.cache.Cache和org.springframe.cache.CacheManager接口来统一不同的缓存技术；

并支持使用JCache(JSR-107)注解简化我们的开发



##### 9.3几个重要的概念和缓存注解

| 接口名称       | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| Cache          | 缓存接口，定义缓存操作，实现有：RedisCache、EhCacheCache、ConcurrentMapCache等 |
| CacheManager   | 缓存管理器，管理各种缓存（Cache）组件                        |
| @Cacheable     | 主要针对方法配置，能够根据方法的请求参数对其结果进行缓存     |
| @CacheEvict    | 清空缓存                                                     |
| @CachePut      | 保证方法被调用，又希望结果被缓存，用于缓存更新，通常在更新方法上使用 |
| @EnableCaching | 开启基于注解的缓存                                           |
| keyGenerator   | 缓存数据时key生成策略                                        |
| serialize      | 缓存数据时value序列化策略                                    |

### P3 尚硅谷_缓存-基本环境搭建

#### 1- 创建springboot项目

引入模块 cache、web、MySQL、MyBatis

#### 2- 导入数据库文件

spring_cache数据库

department表

employee表

#### 3- 创建bean

```java
public class department{
	private Integer id;
    private String departmentName;
}
```



```java
public class Employee {
	Integer id;
	String lastName;
	String email;
	Integer gender; //性别 1男 0女
    Integer deptId; //部门id
}
```

#### 4- 整合Mybatis操作数据库

##### 4.1整合数据源

```properties
spring.datasource.url=jdbc:mysql://localhosst:3306/spring_chache
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driver-class-name=

#开启驼峰命名
mybatis.configuration.map-under-scope-to-camer-case=true

#开启mybatis日志
logging.level.con.beyondsoft.cache.mapper=debug
```

##### 4.2使用注解版的Mybatis

###### 4.2.1@MapperScan指定需要扫描的mapper接口所在的包

```java
@MapperScan("com.beyondsoft.cache.mapper")
@SpringBooApplication
public  class SpringBootCacheApplication{

}
```

创建Mapper接口

```java
@Mapper
public interface EmployeeMapper{
	@Select("select * from employee where id=#{id}") 
	Employee getEmployeeById(Integer id);
    
    @Update("update employ et lastname=#{lastName},email=#{email},gender=#{gender},deptId=#{deptId} where id=#{id}")
    void updateEmp(Employee employee);
    
    @Delete("delete from employee where id=#{id}")
    void deleteEmpById(Integer id);
    
    @Insert("insert into employee(lastName,email,gender,deptId) values (#{lastName},#{email},#{gender},#{deptId})")
    void insertEmp(Employee employee)
}
```

```java
@Mapper
public interface DepartmentMapper{
	
}
```

单元测试

```
@RunWith()
@SpringBooTest
public class SpringbootCacheApplicationTests{
	
}
```

EmployeeService

```java
@Service
public class EmployeeService{
    @Autowired
    private EmployMapper employMapper;
    
	public Employee getEmplyee(){
		Employee emp= employeeMapper,getEmployeeById(id);
		return emp
	}
}
```

EmployeeController

```java
@RestController
public class EmployeeController{
    @
    public Employee getEmployee(@PathVariavle("id")Integer id){
        
    }
}
```

### P4 尚硅谷_缓存-@Cacheable初体验

快速使用缓存步骤

- 开启基于注解的缓存 @EnableCaching
- 标注缓存注解即可

```java
@EnableCaching
@MapperScan("com.beyondsoft.cache.mapper")
@SpringBooApplication
public  class SpringBootCacheApplication{

}
```

```properties
#开启mybatis日志
logging.level.con.beyondsoft.cache.mapper=debug
```

```java
/**
* CacheManager管理多个Cache组件，对缓存的真正CRUD操作在Cache组件中
  几个属性：
  	cahceName/value 指定缓存组件的名字
 	key 缓存数据使用的key；可以用它来指定，默认是使用方法参数的值，
 		可以使用SpEl表达式#id 参数的id值 等同于#a0 #p0 #root.args[0]
 		 key="#root.args[0]"
 	keyGenerator:key的生成器 可以自己指定key的生成器的组件id
 	keyGenerator/key 二选一
 	cacheManager 指定缓存管理器，或者cacheResolver指定获取解析器
 	condition: 指定符合条件的情况下才缓存 如 condition="#id>0"
 	unless 否定缓存，当unless指定的条件为true买饭饭的返回值就不会被缓存 可以获取到结果进行缓存
 	sync 是否使用异步模式
*/

@Cacheable(cacheName="emp")
public Employ getEmployee(Integer id){
	Employee emp= employeeMapper,getEmployeeById(id);
	return emp
}
```

**Cache SpEL表达式**

| 名字          | 位置        | 描述                 | 示例             |
| ------------- | ----------- | -------------------- | ---------------- |
| methodName    | root object | 当前被调用的方法名   | #root.methodName |
| method        | root object | 当前被调用方法       |                  |
| target        | root object | 当前被调用的目标对象 |                  |
| targetClass   | root object |                      |                  |
| args          | root object |                      |                  |
| caches        | root object |                      |                  |
| argument name |             |                      |                  |
| result        |             |                      |                  |

### P5 尚硅谷_缓存-缓存工作原理&@Cacheable运行流程

##### 原理：

###### 1 自动配置类CacheAutoConfiguration

###### 2- 缓存的配置类 11个缓存配置

- GenericCacheConfiguration

- JCacheCacheConfiguration

- EhCacheConfiguration

- HazelCacheConfiguration

- InfinispanCacheConfiguration

- CouchebaseCahceConfiguration

- RedisCacheConfiguration

- CaffeineCahceConfiguration

- GuavaCacheConfiguration

- SimpleCacheConfiguration

- NoOpCacheConfiguration

###### 3-哪个配置类默认生效

在配置application.properties中配置

```properties
debug=ture #打开自动配置报告
```

SimpleCacheConfiguration 为默认配置

###### 4- 给容器中注册了一个CacheManager ConcurrentMapCacheManager

###### 5- 可以获取和创建ConcurrentMapCache缓存的组件 

​	它的作用是将数据保存在ConcurrentMap中实现

##### 运行流程：

@cacheable

1- 方法运行之前，先去查询Cache(缓存组件)，按照cacheNames指定的名字获取；

​	CacheManager先获取相应的缓存 getCache 第一次获取缓存 如果没有Cache组件，会自动创建，

2- 去Cache中查找 缓存的内容，使用一个key 默认是方法的参数

​	key是按照某种策略生成的；默认是使用KeyGenerator生成的；默认使用SimpleKeyGenerator生成key

​		SimpleKeyGenerator生成Key的默认策略

​				如果没有参数， key=new SimpleKey();

​				如果只有一个参数 key=参数的值

​				如果有多个参数  key= new SimpleKey(params);

3- 没有查到缓存就调用目标方法

4-将目标方法返回的结果，放进缓存中

方法执行之前@cacheable标注的方法，执行之前先检查缓存中有没有这个数据，默认安装参数的值作为key去查询缓存，如果没有就运行方法并将结果放入缓存;以后再来调用就可以直接使用缓存中的数据

**核心**：

1- 使用CacheManager[ConcurrentMapCacheManager] 按照名字得到Cache【ConcurrentMapCache】组件

2-key使用keyGenerator生成，默认是SimpleKeyGenerator

### P6 尚硅谷_缓存-@Cacheable其他属性

#### 1-cacheName/vaue属性

cacheName/vaue：指定缓存组件的名字；将方法的返回结果放在那个缓存中，是数组方式，可以指定多个缓存；

#### 2-key属性

指定key为 getEmp[2] 形式

```java
@Cacheable(cacheName="emp",key="#root.methodName+'['+#id+']'")
public Employ getEmployee(Integer id){
}
```

#### 3-keyGenerator属性

##### 自定义Key的生成器

```java
@Configuration
public class MyCacheConfig{
    public KeyGenerator(){
        new KeyGenerator(){
            public Object generate(Object target,Method method,Object... params){
                return methodn.name()+"[" +Arrays.asList(param).toString()+"]";
            }
        }
    }
}

@Cacheable(cacheName="emp",keyGenerator="myKeyGenerator")
public Employ getEmployee(Integer id){
}
```

#### 4-condition属性

```java
/**
condition="#a0>0"
condition 缓存条件
第一个参数的值大于0时 才进行缓存
*/
@Cacheable(cacheName="emp",key="#root.methodName+'['+#id+']'",condition="#a0>0")
public Employ getEmployee(Integer id){
}
```

```
,condition="#a0>0 and #root.methodName eq 'aaa'"
```

#### 5- unless属性 否定缓存

unless 否定缓存，当unless指定的条件为true，方法的返回值就不会被缓存，

```java
@Cacheable(cacheName="emp",key="#root.methodName+'['+#id+']'",condition="#a0>0",unless ="#a0==2")
//unless 如果第一个参数的值a0==2时 不进行缓存，虽然 conditiaon条件满足，但是 unless条件成立 不进行缓存。
```

#### 6-sync 是否异步

sync是否异步模式 默认是false 以同步方式

异步模式下，不支持unless属性

### P7 尚硅谷_缓存-@CachePut

@CachePut 即调用方法同时更新缓存；

```java
@CachePut(value="emp") //缓存更新
public Employee updateEmp(Employee employee){
	employeeMapper.update(employee);
	return employee;
}
```

@CachePut 运行时机：

​	1、先调用目标方法 

​	2、再将目标方法的执行结果缓存起来



默认使用参数作为key 也就是employee对象作为参数

但是查询方法getEmp使用的是id作为参数，所以修改方法更新的缓存 并没有将之前查询方法生成的缓存给覆盖掉，不是同一个key

所以需将 修改/更新方法的key设置为id

```
key="emplyee.key"  使用传入的参数的员工id作为key
key="result.key"   使用返回后的结果员工对象的id作为key @Cacheable是不能使用 key="#result.id" 因为是方法执行之前调用 此时result为null 而不是方法之后调用
```

### P8 尚硅谷_缓存-@CacheEvict

@CacheEvict 缓存清除 

key指定要清除的数据

```java
@CacheEvict(value="emp",key="#id")  //默认的key 就是以方法的全部参数为key的，索引 key="id" 在此时（方法仅有一个参数时） 在这个方法上可以不写
public void deleteEmp(Integer id){
	System.out.pringln("deleteMap"+id);
	employeeMapper.deleteById(id);
	
}
```

```java
@CacheEvict(value="emp",key="#id", allEntires=ture)
属性 allEntires 是否删除value="emp"的所有缓存，默认是false
allEntires=ture 会将emp的所有缓存id 全部删除
allEntires=ture 清除这个缓存中的所有数据
beforeInvocation=false 默认值是false，是否在方法之前执行；默认false 在方法之后执行
    之前/之后清空缓存 在于如果方法有异常 
    之后清空缓存 由于方法执行出错，缓存不会被清除
    之前清空缓存 不管方法是否出错，都会清空缓存
```



### P9 尚硅谷_缓存-@Caching&@CacheConfig

@Caching 是@Cacheable @CachePut @CacheEvict 组合注解。按需调用

@Caching 定义复杂的缓存规则

```java
@Caching(
    cacheable={
        @Cacheable(value="emp",key="lastName")
    },
    put={
        @CachePut(value="emp",key="result.id"),
        @CachePut(value="emp",key="result.email")
    }
)
public Employee getEmpByLastName(String lastName){
	return employeeMapper.getEmpByLastName(lastName);
}
```

@CacheConfig

设置在类上，指定类的所有方法，设置缓存的名字，key的生成器 ，keyManager

```java
@CacheConfig(value="emp")
public class EmployeeService{
	//里面的方法的 所有Cache 的value就都不用写了
	
	@Cacheable(key="#id")
	public Employee getEmplyee(Integer id){
        //。。。。。
    }
}
```

### P10 尚硅谷_缓存-搭建redis环境&测试

默认使用的是ConcurrentMapManager==ConcurrentMapCache;将数据操作在ConcurrentMap<Object,Object>中

但实际开发中经常使用的是缓存中间件：Memcache、Ehcache、RedisCache

整合Redis作为缓存

##### 什么是Redis

redis官网 https://redis.io

Redis中文网 http://www.redis.cn

**概念**

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。

Redis支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

##### 安装Redis

使用docker方式安装

```bash
docker images
docker pull redis # 国外网站 比较慢 甚至下载失败

#镜像中国加速
# www.docker-cn.com/
#docker pull regustry.docker-cn.com/library/要下载的镜像名
docker pull registry.docker-cn.com/library/redis

docker images
docker run -d -p 6379:6379 --name myredis  regustry.docker-cn.com/library/redis
docker ps
#使用docker的客户端工具测试连接

```

##### Redis常用操作

```bash

sadd myset zhagnsan lisi
smemebers myset

sismember myset wangwu # wangwu是否 集合myset的成员

```



### P11 尚硅谷_缓存-RedisTemplate&序列化机制

##### 引入Redis的starter依赖

```xml
<depency>
    <groupId>org.springframework.boot</groupId>
	<artifictId>spring-boot-starter-data-redis</artifictId>
</depency>
```

##### 配置redis连接

```properties
spring.redis.host=192.168.1.110

```

自动添加了redisTemplate和StirngRedisTemplate

可以通过注入直接使用

```java
@Autowired
private RedisTemplate redisTemplate;
@Autowired
private StirngRedisTemplate stirngRedisTemplate;
```

**两者的区别**

redisTemplate 操作k-v都是对象

StirngRedisTemplate  操作k-v都是对象

##### 测试Redis

测试操作 Redis常见的五大数据类型

String字符串  List列表   Set集合  Hash散列 Zest有序集合

- stringRedisTemplate.opsForValue()  操作字符串
- stringRedisTemplate.opsForList()   操作List列表
- stringRedisTemplate.opsForSet() 操作Set集合
- stringRedisTemplate.opsForHash()  操作Hash
- stringRedisTemplate.opsForZSet()
- 

```java
@Autowired
private RedisTemplate redisTemplate;
@Autowired
private StirngRedisTemplate stirngRedisTemplate;

@Test
public void test01(){
    //字符串
    stirngRedisTemplate.opsForValue.addend("msg","hello");  //追加    
    stirngRedisTemplate.opsForValue.get("msg") //读出
    //list    
    stirngRedisTemplate.opsForList().leftPush("mylist","1");
    stirngRedisTemplate.opsForList().leftPush("mylist","2");
    
}

public void test02(){
    redisTemplate.opsForValue().set("emp-01",employee);
    //employee 必须是序列化的类的实例
    // 可以通过 implements Serivalizable 将类序列化
    //默认如果保存对象，使用jdk序列化机制，将序列化后的数据保存到redis中
    //将数据以json的方式实现，
    //1-自己将对象转为json
    //2-将默认的序列化器 改为json
}
```

##### 将默认的序列化器 改为json

```java
@Configuration
public class MyRedisConfig{
	@Bean
	public RedisTemplate<Object,Employee> empRedisTemplate(RedisConnectionFactory redisConnectionFactory) throw UnKnownHostException {
        RedisTemplate<Object,Employee> template = new RedisTemplate<Object,Employee>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer<Employee> set = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
        template.setDefaultSerializer(ser);
        return template;
    }
}
```

测试类中 注入

```java
@Autowired
private RedisTemplate redisTemplate;
@Autowired
private StirngRedisTemplate stirngRedisTemplate;
@Autowired
private RedisTemplate<Object,Employee> empRedisTemplate;

@Test
public void test01(){
        
    empRedisTemplate.opsForValue().set("emp-01",employee);
    
}
```

### P12 尚硅谷_缓存-自定义CacheManager

原理：CacheManager 创建Cache缓存组件来时间给缓存中的

1、 引入redis的starter后，容器中保存的是RedisCacheManager

2、RedisCacheManager 帮我们创建RedisCache来作为缓存组件，RedisCache通过操作redis来缓存数据的

3、默认保存数据 k-v都是Object 利用序列化保存； 如何自动保存为json

​		1)、由于引入了redis的starter，CacheManager变为RedisCacheManager

​		2)、默认创建的RedisCacheManager，操作redis的时候使用的RedisTemplate<Object,Object>

​		3)、RedisTemplate<Object,Object>是 默认使用jdk的序列化机制，需要自定义RedisCacheManager

4、自定义RedisCacheManager

##### 自定义RedisCacheManager

```java
@Bean
public RedisCacheManager employeeCacheManager( RedisTemplate<Object,Employee> empRedisTemplate){
    RedisCacheManager cacheManager = new RedisCacheManager(empRedisTemplate);
    //key多了一个前缀
    cacheManager.setUsePrefix(true); //使用前缀 默认会将CacheName作为前缀
    //CacheManagerCustonizers可以来定制缓存的一些规则
    return cacheManager;
}
```

DeptService

```java
@Service
public class DeptService{
	@Autowired
    private DeptMapper detpMapper;
    
    @Cacheable(cacheNames="dept")
    public getDpetById(Integer id){
        System.out.print("");
        return detpMapper.getDpetById(id);
    }
}
```

缓存的数据能存入redis  dept可以转为json

第二次从缓存中查询就不能反序列回来

存是是dept的json数据，CacheManager默认是使用RedisTemplate<Object,Employee>操作redis的

只能将Employ反序列化 

```java
@Bean
public RedisCacheManager employeeCacheManager( RedisTemplate<Object,Employee> deptRedisTemplate){
    RedisCacheManager cacheManager = new RedisCacheManager(deptRedisTemplate);
    //key多了一个前缀
    cacheManager.setUsePrefix(true); //使用前缀 默认会将CacheName作为前缀
    //CacheManagerCustonizers可以来定制缓存的一些规则
    return cacheManager;
}

	@Primary //有多个CacheManager的情况下 一定要把其中一个设置为 主Primary  也就是默认配置的缓存管理器CacheManager
	@Bean
	public RedisTemplate<Object,Deparment> deptRedisTemplate(RedisConnectionFactory redisConnectionFactory) throw UnKnownHostException {
        RedisTemplate<Object,Employee> template = new RedisTemplate<Object,Deparment>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer<Deparment> set = new Jackson2JsonRedisSerializer<Dept>(Deparment.class);
        template.setDefaultSerializer(ser);
        return template;
    }
```

在Service中指定CacheManager

```java
@CacheConfig(CacheManaer="deptRedisTemplate")
@Service
public class DeptService{
    //也可以在每个方法中指定cacheManager
    @Cacheable(cacheManager="deptRedisTemplate",cacheName="dept")
    public Deparment getEmpById(Integer id){
        
    }
}
```

```java
@CacheConfig(CacheManaer="empRedisTemplate")
@Service
public class EmployeeService{
    
}
```

实际 开发中 应将默认的CacheManager作为@Primary 作为主



##### 以代码方式使用缓存

上面都是以注解方式使用缓存，下面使用编码方式，在方法内部，使用缓存管理器得到缓存，再对缓存进行操作/api调用

```java
@Qualifier("deptCacheMmanaer")
@Autowired
private CacheManager deptCacheMmanaer; //在有多个缓存管理器的情况下，默认使用名称自动注入 使用@Qualifier可以进行明确的指定id="deptCacheMmanaer"

//这里方法上不使用任何缓存注解
public Department getDeptById(Integer id){
	Department dept=departmentMapper.getDeotById(id);
    //获取某个缓存
	Cache dept = deptCacheManager.getCache("dept");
    dept.put("dept:1",dept);
    return dept;
}
```

## 十、Spring Boot与消息

本章内容：JMS AMQP RabbitMQ

### P13 尚硅谷_消息-JMS&AMQP简介

#### 1、概述

##### 1.1 消息中间件的使用场景

大多数应用中，可以通过消息服务中间件来提示系统异步通信，扩展解耦能力

- **异步处理**

- ##### 应用解耦

- **流量削峰**

##### 1.2 消息服务中的两个概念

- 消息代理（Message Broker）服务器
- 目的地（destination）

##### 1.3 目的地的两种形式

- **队列（Queue）**：点对点(point-to-point)方式消息通信
- **主题（Topoic）**：发布(publish)/订阅(subscribe)方式消息通信

##### 1.4 点对点

- 消息发送者发送消息，消息代理将其放入一个队列中，消息接收者从队列中获取消息内容，消息读取后被溢出队列
- 消息只有唯一的发送者和接收者，但并不是说只能有一个接收者

##### 1.5 发布订阅模式



##### 1.6 JMS(Java Message Service) Java消息服务

- 基于JVM消息代理的规范，ActiveMQ HornetMQ是JMS的实现

##### 1.7 AMQP（Advanced Message Queuing Protocol）

- 高级消息队列协议，也是一个消息代理的规范，兼容JMS
- RabbitMQ是QMQP的一个实现。

**JMS与AMQP对比**

| 对比项         | JMS                                                          | AMQP                                                         |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 定义           | JAVA API                                                     | 网络级协议                                                   |
| 跨语言         | 否                                                           | 是                                                           |
| 跨平台         | 否                                                           | 是                                                           |
| Model模型      | 提供两种消息模型：<br/>(1) peer-2peer<br/>(2)Pub/Sub         | 提供了五种消息模型<br>1、direct exchange<br>2、fanout exchange<br>3、topic exchange<br>4、headers exchange<br>5、system exchange<br>本质上来讲，后四张和JMS的Pub/Sub模型没有太多区别，仅在路由机制上做了更详细的划分 |
| 支持的消息类型 | 多种消息类型：<br>TextMessage<br>MapMessage<br>BytesMessage<br>StreamMessage<br>ObjectMessage（只有消息头和属性） | Byte[]<br>当实际应用时，有复杂的消息，可以将消息序列化后发送。 |
| 综合评价       | JMS定义了JAVA API层面的标准；在java体系中，多个Client均可以通过JMS进行交互，不需要应用修改代码；但是它对跨平台的支持较差 | AMPQ定义了wire-level层的协议标准；天然具有跨平台、跨语言特性。 |

##### 1.8 Spring对MQ的支持

- Spring-jms提供了对JMS的支持
- spring-rabbit提供了对AMQP的支持
- 需要ConnectionFactiry的实现来连接消息代理
- 提供JmsTemplate、RabbitTemplate来发送消息
- @JmsListener（JMS）、@RabbitListener(AMQP)注解在方法上监听消息代理发布的消息
- @EnableJms、@EnableRabbitit开启支持

##### 1.9 SpringBoot自动配置

- JmsAutoConfiguration
- RabbitAutoConfiguration

### P14 尚硅谷_消息-RabbitMQ基本概念简介

#### 二、RabbitMQ简介

##### RabbitMQ简介

RabbitMQ是一个有erlang开发的AMQP(Advanved Message Queue Proocol)的开源实现

##### 核心概念

**Message**

消息，消息不是具名的，他有消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key(路由键)、priority(相对于其他消息的优先权)、delivery-mode(指出该消息可能需要持久性存储)等。

**Publisher**

消息的生产者，也是一个向交换器发布消息的客户端应用程序。

**Exchange**

交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。

Exchange有四种类型：direct(默认)、fanout、topic、headers，不同类型的Exchange转发消息的策略有所区别

**Queue**

消息队列，用来保存消息知道发送给消费者，它是消息的容器，也是消息的终点。，一个消息可以投入一个或者多个队列，消息一直在队列里面，等待消费者连接到这个队列将其取走。。

**Binding**

绑定，用于消息对了和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解为一个由绑定构成的路由表。

Exchange和Queue的绑定可以是多对多关系

**Connection**

网络连接， 比如一个TCP连接

**Channel**

信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的TCP链接内的虚拟连接，AMQP命令都是通过信道发出去的，不管是发布消息，订阅队列合适接收消息， 这些动作都是通过信道完成。因为对于操作系统来说建立和销毁TCP都是非常昂贵的开销，，所以引入了信道的概念，以复用一条TCP连接。

**Consumer**

消息的消费者，表示一个从消息队列中取得消息的客户端应用程序

**VirtualHost**

虚拟主机，表示一批交换器、消息队列和相关对象。虚拟注解是共享相同的身份认证和加密环境的独立服务器域，每个vhost本质上是一个mini版的RabbotMQ服务器，拥有自己的队列、交换器、绑定和权限机制。Vhost是AMQP概念的基础，必须在连接时指定，RabbitMQ默认的vhost是 /。

虚拟主机之间是隔离的

**Broker**

消息代理，表示消息队列服务器实体

### P15 尚硅谷_消息-RabbitMQ运行机制

#### 三、RabbitMQ运行机制

##### AMQP中的消息路由

AMQP中消息的路由过程和Java开发者熟悉的JMS存在一些差别，AMQP中增加了**Exchange和Binding**的角色。生产者把消息发布到Exchange上，消息最终到达队列并被消费者接收，而Binding决定交换器的消息应该发送到那个队列。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200206121432892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvbGRfX19wbGF5,size_16,color_FFFFFF,t_70)

##### Exchange类型

Exchange分发消息时根据类型的不同分发策略有区别，目前共有4中类型：**direct、fanout、topic、headers**。

headers匹配AMQP消息的header而不是路由键，headers交换器和direct交换器完全一致，但性能差很多，目前几乎用不到了，所以直接看另外三中类型。

- **Direct Exchange：**
  消息中的路由键（routing key）如果和Binding中的binding key一致，交换器将消息发到对应的队列中。路由键与队列名完全匹配，如果一个队列绑定到交换机要求路由键为"dog"，则只转发routing key 标记为"dog"的消息，不会转发"dog.puppy"，也不会转发"dog.guard"等等。它是完全匹配、单播的模式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200206122448774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvbGRfX19wbGF5,size_16,color_FFFFFF,t_70)

- **Fanout Exchange：**
  每个发到fanout类型交换器的消息都会分到所有绑定的队列上去。fanout交换器不处理路由键，只是简单的将队列绑定到交换器上，每个发送到交换器的消息都会被转发到与该交换器绑定的所有队列上。

  很像子网广播，每台子网内的主机都获得了一份复制的消息。fanout类型转发消息是最快的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200206122733260.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvbGRfX19wbGF5,size_16,color_FFFFFF,t_70)

- **Topic Exchange：**
  topic交换器通过模式匹配分配消息的路由键属性，将路由键和某个模式进行匹配，此时队列需要绑定到一个模式上。它将路由键和绑定键的字符串切分成单词，这些单词之间用点隔开。它同样也会识别两个通配符：符号"#“和符号”*"。#匹配0个或多个单词，*匹配一个单词。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200206122948254.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvbGRfX19wbGF5,size_16,color_FFFFFF,t_70)

### P16 尚硅谷_消息-RabbitMQ安装测试

#### RabbitMQ的安装

```
docker images
docker search rabbitmq
```

安装带有managemet标记的 说明是带有管理界面的

```bash
docker pull rabbitmq 	##最新版的 已经带有管理端
docker pull rabbitmq:3-management
doucker registry.docker-cn.com/rabbitmq:3-management # docker-cn.com已经停用了
```

```bash
docker run -d -p 5672:5672 -p 15672:15672 --name my-rabbitmq 镜像id
或
docker run -d -p 5672:5672 -p 15672:15672 --name my-rabbitmq rabbitmq
```

```
192.168.0.110:5672
```



##### 1-添加交换器

##### 2-添加消息队列

##### 3-绑定交换器和消息队列

### P17 尚硅谷_消息-RabbitTemplate发送接受消息&序列化机制

#### 四、RabbitMQ整合

##### 4.0 整合步骤

1 引入spring-boot-starter-amqp

2 application,.yml配置

3 测试RabbitMQ

​	AmqpAdmin 管理组件

​	RabbitTemplate 消息发送处理组件

##### 4.1 新建项目

新建项目spring-rabbitmq，选择模块：Web模块，RabbitMQ模块 

##### 4.2 **自动配置原理**

RabbitAutoConfiguration

	- ConnectionFactory 自动连接工厂
	- RabbitProperties 封装了RabbitMQ的所有配置
	- RabbitTemplate 给RabbitMQ发送接收消息
	- AmqpAdmin RabbitMQ系统管理功能组件
	- 

##### 4.3 配置文件

```properties
sprign,rabbitmq.host=192.168.1.100
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
spring.rabbitmq.port=5672
spring.rabbitmq.virtual-host=/
```

##### 4.4 测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringbootRabbitmqTest{
	@Autowired
    RabbitTemplate rabbitTemplate;
    
    //点对点 发送数据
    @Test
    public void send(){
        //Message需要自己构造 定制消息体内容和消息体
        //rabbitTemplate.send(exchange,routKey,message);
        //传入要发送的对象，自动序列化发送给rabbitmq object默认当做消息体
        //rabbitTemplate.convertAndSend(exchange,routKey,object);
        Map<String,Object> map = new HashMap();
        map.put("msg","这是第一个消息");
        map.put("data",Arrys.asList("helloworld",123,true));
        //对象被jdk默认序列化后发送出去
        rabbitTemplate.convertAndSend("exchange.direct","atguigu.news",map);        
        rabbitTemplate.convertAndSend("exchange.direct","atguigu.news",new Boot("西游记","吴承恩"));        
    }
    
    //接受数据
    @Test
    public void recieve(){
        Object o = rabbit Template.revieveAndConvert("atguigu.news");
        System.out.println(o.getClass());
        System.out.println(o);
    }
}
```

**序列化器设置为Json方式的序列化器**

```java
@Configuration
public class MyAMQPConfig{
    @Bean
	public MessageConverter messageConverter(){
        return new Jackson2JsonMessageConverter();
    }
}
```

```java
//发送广播消息
public void sentMsg(){
    rabbitTemplate.convertAndSend("exchange.fanout","",new Boot("三国演义","罗贯中"));  //exchange.fanout 广播类型的交换器 广播模式下 不需要指定路由键
}
```

### P18 尚硅谷_消息-@RabbitListener&@EnableRabbit

##### 监听方式接收消息

```java
@Service
public class BookService{
	
	@RabbitListener(queues="atguigu.news")
	public void receive(Book book){
		System.out.println(book);
	}
}
```

##### 启动类加入@EnableRabbit

```java
@SpringBootApplication
@EnableRabbit  //开启基于注解的RabbitMQ
public class SpringbootRabbitmqApplication{
	public static void main(String[] args){
		SpringApplication.run(SpringbootRabbitmqApplication.class,args);
	}	
}
```

##### 接收带消息头的消息

```java
	@RabbitListener(queues="atguigu")
	public void receive(Message message){
		System.out.println(message.getBody());  //消息体
		System.out.println(message.getMessageProperties());  //消息头信息
	}
```

### P19 尚硅谷_消息-AmqpAdmin管理组件的使用

AmqpAdmin 通过程序创建 交换器 队列

```java
@Autowired
private AmqpAdmin amqpAdmin;


/**
declare开头的方法 是创建对象
remove开的的方法 是删除对象
*/

@Test
public void createExchange(){
    amqpAdmin.declareExchange(new DirectExchange("amqpadmin.exchange"));
    System.out.println("创建完成");
}

@Test
public void createQueue(){
    amqpAdmin.declareQueue(new Queue("amqpadmin.queue",true)); //true 是否持久化
    System.out.println("创建完成");
}

@Test
public void createBinding(){
    amqpAdmin.declareBinding(new Binding("amqpadmin.queue",Binding.DestinationType.QUEUE,"amqpadmin.exchange","amqp.routeKey")); 
    //第一个参数是交换器或队列的名称 第二个参数是 目的地类型， 第三个参数 是交换机名称
    System.out.println("创建完成");
}
```

## 十一、SpringBoot与检索

### P20 尚硅谷_检索-Elasticsearch简介&安装

#### 1 检索

我们的应用经常需要添加检索功能，开源的ElasticSearch是目前全文搜索引擎的首选。它可以快速的存储、搜索和分析海量数据。Spring Boot通过整合Spring Data ElasticSearch为我们提供了非常便捷的检索功能支持；

Elasticsearch是一个分布式搜索服务，提供Restful API，底层基于Lucene，采用多shard（分片）的方式保证数据安全，并且提供自动resharding的功能，github等大型的站点也是采用了ElasticSearch作为其搜索服务，

#### 2 Elasticsearch安装

```bash
# 以docker方式安装
docker search elasticsearch
docker pull elasticsearch
docker pull elasticsearch:6.8.13
docker images

#运行  默认占用2G的堆内存空间 可以使用 -e ES_JAVA_OPTS="Xms256m -Xmx256m" 来指定初始化堆内存大小
#9300 分布式情况下 各节点之间的通信端口
docker run -e ES_JAVA_OPTS="Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name elasticsearch elasticsearch
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:tag
docker run -e ES_JAVA_OPTS="Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name elasticsearch --net somenetwork -e "discovery.type=single-node" elasticsearch:tag
docker ps
```

以上安装版本较旧，按以上方法安装容易出现问题，参照以下

##### 安装

```bash
## 拉取镜像
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.3.0
## 创建网络
docker network create esnet
## 查看镜像
docker images
## 创建运行
docker run --name es  -p 9200:9200 -p 9300:9300  --network esnet -e "discovery.type=single-node" bdaab402b220
```

测试

```
http://192.168.1.110:9200
```

### P21 尚硅谷_检索-Elasticsearch快速入门

官方文档  https://www.elastic.co

权威指南-中文版  https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html

#### 1、概念

- 面向文档

  Elasticsearch 是 *面向文档* 的，意味着它存储整个对象或 *文档*。

- JSON

  Elasticsearch 使用JSON为文档的序列化格式

- 适应新环境

- 索引

  存入数据的行为 叫索引

- 

#### 2、概念

以 员工文档 的形式存储为例：一个文档代表一个员工数据。存储数据到 ElasticSearch 的行为叫做 索引 ，但在索引一个文档之前，需要确定将文档存储在哪里。 一个 ElasticSearch 集群可以 包含多个 索引 ，相应的每个索引可以包含多个 类型 。 这些不同的类型存储着多个 文档 ，每个文档又有 多个 属性 。 类似关系： 索引-数据库 类型-表 文档-表中的记录 属性-列

![img](https://img-blog.csdnimg.cn/20181205000317930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1YmluMTI4NTU3MDkyMw==,size_16,color_FFFFFF,t_70)

类别Mysql

es的索引  mysql的数据库

es的类型  mysql的数据表

es的文档 mysql的每条记录

es的属性  mysql的字段值/列值

#### 3、使用rest风格的API 进行增删改查

PUT 修改

GET 查询

DELETE 删除

#### 4、查询

###### 4.1 get查询

  查询字符串 q=

###### 4.2 json查询（Post查询）

JSON形式的查询表达式

```json
{
	"query":{
		
	}
}
```

###### 4.3 全文检索

模糊搜索

```json
{
	"query":{
		"match":{   ### 分词匹配
			"about": "rock climb"
		}
	}
}
```

全文匹配

```json
{
	"query":{
		"match_phrase":{  #### 全文匹配
			"about": "rock climb"
		}
	}
}
```

搜索结果高亮

```
{
	"query":{
		"match_phrase":{  #### 全文匹配
			"about": "rock climb"
		}
	},
	"highlight":{
		"fields":{
			"about":{}
		}
	}
}
```

https://blog.csdn.net/yubin1285570923/article/details/84801044



### P22 尚硅谷_检索-SpringBoot整合Jest操作ES

#### 三、整合ElasticSearch测试

- 引入spring-boot-starter-data-elasticsearch
- 安装对应版本的 ElasticSearch
- application.yml配置
- Spring Boot自动配置的

  - ElasticsearchRepository、ElasticsearchTemplate、Client
- ElasticSearch

##### 3.1 创建工程

springboot-elasticsearch

选择模块web、ElasticSearch

springboot 默认是使用springdata elastriseach模块进行操作的

##### 3.2 自动配置

springBoot默认支持两种技术 和ElasticSearche进行交互

1、Jest (默认不生效， 需要导入jest工具包 才会生效)

2、SpringBoot ElasticSearch

		- 客户端 Client  clusterNodes clusterName
		- ElasticsearchTemplate 操作ES
		- ElasticsearchRepository 类似JPA的编程方式 编写一个ElasticsearchRepository 的子接口操作ES

##### 3.2 引入依赖（方式一）

使用jest作为客户端

```xml
    <dependency>
        <groupId>io.searchbox</groupId>
        <artifactId>jest</artifactId>
        <version>6.3.1</version>
    </dependency>
```



##### 3.2 配置

```
spring.elastaicsearch.uris=http://192.168.0.110:9200
```

##### 3.3 测试

```java
@Data
public class Article{
    @JestId
    private Integer id;
    private String title;
    private String article;
    private String content;
}
```



```java
@RunWith(SrpingRunner.class)
@SpringBootTest
public class SpringBootApplicationTest{

    @Autowired
    JestClient jestClient; 
    
    @Test
    public void saveIndex() thorw Exception{
        Article article = new Article();
        article.setTitle("消息");
        article.setAuthor("张三");
        article.setContent("hello world");
            
        //构建一个索引
        Index index =new Index.Builder(article).index("beyodsoft").type("news").build();
    }
}
```

测试搜索

```java
public void search(){
    //查询表达式
    String json="{\n" +
                "    \"query\" : {\n" +
                "        \"match\" : {\n" +
                "            \"content\" : \"hello\"\n" +
                "        }\n" +
                "    }\n" +
                "}";

    
    //构建搜索功能
    Search search = new Search.Builder(json).addIndex("beyondsoft").addType("news").build();
    SearchResult result = jestClient.execute(search);
    System.oout.println(result.getJsonString());
}
```



### P23 尚硅谷_检索-整合SpringDataElasticsearch

#### 方式二：使用SpringData-Elasticsearch 

##### 23.1 引入依赖

```xml
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

##### 23.2 配置

```properties
spring.data.elasticsearch.cluster-name=elasticsearch
spring.data.elasticsearch.cluster-nodes=192.168.0.110:9300
```

报错：es版本不合适

版本对照关系

![img](https://img-blog.csdnimg.cn/20190618205519594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkwNzMzMg==,size_16,color_FFFFFF,t_70)

**解决方发：**

- 升级springboot版本
- 安装对应版本的es

```
dockerimages
docker pull elasticsearch:2.4.6
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p9201:9201 -p 9301:9301
```

```
http://192.168.1.110:9201
```

**两种用法**

使用Repository

```java
public interface BookRepositry extends ElasticsearchRepository<Book,Integer>{

}
```

```java
@Doucmnet(indexName"beyondsoft",type="book")
public class Book{

}
```

测试类

```java
@Autowired
private BookRepository bookRepository;

@Test
public void testByRespository{
    Book book = new Book();
    book.setId(1);
    book.setName("西游记");
    book.setAuthor("吴承恩");
    bookRepository.index(book);
}
```

```
http://192.168.0.110:9200/beyondsoft/book/_search # 查询全部
http://192.168.0.110:9200/beyondsoft/book/1 #查询id为1的
```

基本的增删改查，在Repository中都有，还支持自定义的方法

```java
public interface BookRepositry extends ElasticsearchRepository<Book,Integer>{
	//自定义方发
    public List<Book> findByBookNameLike(String bookName); //不需要写实现
}
```

```java
@Test
public void test02(){
	for(Book book : bookRepository.findByBookNameLike("游记")){
		System.out.println(book.getName());
	}	
}
```

方法的命名 不需要写实现， 但是需要根据官方文档来命名 方法的名字 ，并不是随意一个方法 都不用实现 就可以使用

方法命名规范-官方文档

https://docs.spring.io/spring-data/elasticsearch/docs/3.2.12.RELEASE/reference/html/#elasticsearch.query-methods

https://docs.spring.io/spring-data/elasticsearch/docs/3.2.12.RELEASE/reference/html/

使用注解 自定义方法

https://docs.spring.io/spring-data/elasticsearch/docs/3.2.12.RELEASE/reference/html/#elasticsearch.query-methods

## 十二、SpringBoot 与任务

本章内容 异步任务、定时任务、邮件任务

### P24 尚硅谷_任务-异步任务

#### 一、异步任务

##### 4.1 创建工程

springboot-task   选择Web模块

##### 4.2 创建异步任务

```java
@Service
public class AsyncService{
    
    @Async  //标识这是个异步任务
	public void hello(){
        Thread.sleep(3000)
		System.out.println("异步任务");
	}
}
```

```java
public class AsyncController{
    
}
```

##### 4.3 主启动类上添加@EnableAsync 

```java
@EnableAsync  //开启异步注解功能
@SpringBootApplication
public class SpringbootTaskApplication{

}
```

### P25 尚硅谷_任务-定时任务

#### 二、定时任务

项目开发中检查需要执行一些定时任务，比如需要在每天凌晨的时候，分析异常前一天的日志新，Spring为我们提供了异步执行任务的调度方式，提供了

**TaskExecutor**、**TaskScheduler**接口。

两个注解@EnableScheduling、@Scheduled

**Cron表达式**

| 字段                     | 允许值                                  | 允许的特殊字符             |
| ------------------------ | --------------------------------------- | -------------------------- |
| 秒（Seconds）            | 0~59的整数                              | , - * /   四个字符         |
| 分（*Minutes*）          | 0~59的整数                              | , - * /   四个字符         |
| 小时（*Hours*）          | 0~23的整数                              | , - * /   四个字符         |
| 日期（*DayofMonth*）     | 1~31的整数（但是你需要考虑你月的天数）  | ,- * ? / L W C   八个字符  |
| 月份（*Month*）          | 1~12的整数或者 JAN-DEC                  | , - * /   四个字符         |
| 星期（*DayofWeek*）      | 1~7的整数或者 SUN-SAT （1=MON）0,7是SUN | , - * ? / L C #   八个字符 |
| 年(可选，留空)（*Year*） | 1970~2099                               | , - * /   四个字符         |

| 特殊字符 | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| ,        | 枚举                                                         |
| -        | 区间                                                         |
| *        | 任意                                                         |
| /        | 步长                                                         |
| ?        | 日/星期冲突匹配                                              |
| L        | 最后                                                         |
| W        | 工作日                                                       |
| C        | 和calendar联系后计算过的值                                   |
| #        | 星期，4#2 第2个星期四    1-6代表 周一到周六   0和7都代表周日 |



##### 2.1 创建定时任务

```java
public class ScheduledService{

    //共6位 秒 分 时 日 月 周几
    
    @Scheduled(cron="0 * * * * MON-SAT") //周一到周六 每分钟的零秒执行一次
    @Scheduled(cron="0,1,2,3,4 * * * * MON-SAT")  //周一到周六 每分钟的前4秒都执行一次
    @Scheduled(cron="0-4 * * * * MON-SAT")  //周一到周六 每分钟的前4秒都执行一次
    @Scheduled(cron="0/4 * * * * MON-SAT")  //周一到周六  每4秒执行一次
	public void hello(){
		System.out.print("hello 定时任务");
	}
}
```

##### 2.2 主启动类开启定时任务

```java
@SpringBootApplication
@EnableScheduling  //开启基于注解的定时任务
public class SpringbootTaskApplication{

}
```

### P26 尚硅谷_任务-邮件任务

#### 三、邮件任务

邮件发送需要引入spring-boot-starter-mail

SpringBoot自动配置MailSenderAutoConfiguration

定义MailProperties内容，配置在application.yml中

自动装配JavaMailSender

测试邮件发送

##### 3.1 引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifictId>spring-boot-starter-mail</artifictId>
</dependency>
```

##### 3.2 自动配置

SpringBoot自动配置MailSenderAutoConfiguration

MailProperties

##### 3.3 配置

```properties
spring.mail.username=xx@qq.com
spring.mail.password=  # 授权码 非qq密码
spring.mail.host=smtp.qq.com

#使用qq邮箱 需要是安全连接
spring.mail.properties.mail.smtp.ssl.enable=true  #开启安全连接
```

##### 3.4 测试发送邮件

###### 注入JavaMailSenderImpl

```java

@Autowired
JavaMailSenderImpl mailSender;

@Test
public void sendMail(){
    SimpleMailMessage message = new SimpleMailMessage();
    message.setTitle("通知");  //邮件标题
    message.setTest("本月工资翻倍"); //邮件内容
    message.setTo("xx@qq.com"); //发送给数
    message.setFrom("xx@qq.com"); //发件人有效
	mailSender.send();
}
```

##### 

```java
@Test
public void sendMail02() throws Exception {
	//创建复杂邮件
	MimeMessage mimeMessage = mailSender.createMimeMessage();
	MimeMessageHellper helper = new MimeMessageHellper(mimeMessage,true); //第二个参数 是否上传附件
    //邮件设置
    helper.setTitle("通知");  //邮件标题
   //helper.setTest("<b style='cloro:red'>本月工资翻倍<b>"); //邮件内容
    helper.setTest("<b style='cloro:red'>本月工资翻倍<b>",true ); //邮件内容 第二个参数 是否发送html格式
    helper.setTo("xx@qq.com"); //发送给数
    helper.setFrom("xx@qq.com"); //发件人有效
    
    //上传附件
    helper.addAttachment("1.jpg", new File("D:\images\1.jpg"));
    helper.addAttachment("2.jpg", new File("D:\images\1.jpg"));
    mailSend.send(mimeMessage);
}
```



## 十三、SpringBoot 与安全

安全 Spring Security

### P27 尚硅谷_安全-测试环境搭建

##### 5.1 创建项目springboot-security

引入Web、Thymeleaf

##### 一、安全

Spring Securit 是针对Sping项目的安全框架，也是Spring Boot底层安全模块默认的技术选型。他可以实现强大的web安全控制。对于安全控制控制，我们仅需要引入依赖spring-boot-starter-securit模块，进行少量配置，即可实现强大的安全管理。

几个类：

WebSecurityConfigurationAdapter：自定义Security策略

AuthenticationManagerBuilder：自定认证策略

@EnableWebSecurity：开启WebSecurity模式

- 应用程序在安全方面的两个主要问题是：**认证** 和 **授权**（或者访问控制）；这两个问题也是SpringSecurity的两个目标
- **认证Authentication** 是建立一个它声明的主体过程（一个主体 一般是指用户，设备或一些可以在你的应用程序中执行动作的其他系统）
- **授权Authorization** 是指确定一个主体是否运行在你的应用程序执行一个动作的过程。为了抵达需要授权的功能，主体的身份已经有人在过程的建立
- 这两个概念是通用的，不仅仅在SpringSecurity中 在类似的其他系统中 如shiro中也一样。

### P28 尚硅谷_安全-登录&认证&授权

步骤

##### 8.1 引入POM

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifictId>spring-boot-starter-security</artifictId>
</dependency>
```

##### 8.2 编写SpringSecurity配置类

```java
@EanbleWebSecurity
public class SecurityConfig extends WebSecuritConfigurerAdapter{
    
}
```

##### 8.3 控制请求的访问权限

```java
@EanbleWebSecurity
public class SecurityConfig extends WebSecuritConfigurerAdapter{
    
    //定义授权规则
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //定制请求的授权规则
	    http.authorizeRequests()
          .antMatcher("/").permitAll();
          .antMatcher("/level1/**").hasRole("VIP1")
          .antMatcher("/level2/**").hasRole("VIP2")
          .antMatcher("/level3/**").hasRole("VIP3")
		;
        //开启登录功能
      	http.formLogin();
        //1 /login 会重定向到登录页
        //2 登录失败 会
        
        //开启自动配置的退出登录
        http.logout();
    }   
    
    //定义认证规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception{
        auth.inMemoryAuthentication().withUser("zhagnsan").password("123456").roles("VIP1","VIP2")
            .and()
            .withUser("lisi").password("123456").roles("VIP2","VIP3")
            .and()
            .withUser("wangwu").password("123456").roles("VIP1","VIP3");
    }
}
```



### P29 尚硅谷_安全-权限控制&注销

```java
    @Override
    protected void configure(HttpSecurity http) throws Exception 
		//开启自动配置的退出登录
        http.logout();
		// 1 访问 /logout 表示注销并清空session
		// 2 注销成功 会返回到/login?logout
		http.logout().logoutSuccessUrl("/"); //修改注销成功后 访问的页面
	}
```

```
sec:authprize=isAuthenticated()
```

##### 在thymeleaf模板中 使用springsecurity 相关标签

引入 springsecurity和thymeleaf的整合模块

```xml
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifictId>thymeleaf-extras-springsecurity4</artifictId>
    <version3.0.2.RELEASE</versuib>
</dependency>
```

页面中引入security的命名空间

```html
<html xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4">
```

```html
<div sec:authprize="!isAuthenticated()">  //没有认证
    游客你好 ，请登录
</div>
<div sec:authprize="isAuthenticated()">  
    <span sec:authentication="name"></span> 你好，你的角色
    <span sec:authentication="principal.authorities"></span>
    <form th:action="@{/logout}" method="post">
        <input type="submit" value="注销"/>
    </form>
</div>
```

```html
<div sec:authorize="hasRole('VIP1')">
    普通武功秘籍
</div>
<div sec:authorize="hasRole('VIP2')">
    高级武功秘籍
</div>
<div sec:authorize="hasRole('VIP3')">
    绝世武功秘籍
</div>
```



### P30 尚硅谷_安全-记住我&定制登陆页

#### 记住我功能

```java
http.rememberMe();
//登录成功之后 将cookie发送给浏览器保存，以后登录带上这个cookie 只要通过检查就可以面登录
//点击注销就会删除这个cookie
//默认14天
//增加了remember-m 的一个Cookie
```

```java
@EanbleWebSecurity
public class SecurityConfig extends WebSecuritConfigurerAdapter{
    
    //定义授权规则
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //定制请求的授权规则
	    http.authorizeRequests()
          .antMatcher("/").permitAll();
          .antMatcher("/level1/**").hasRole("VIP1")
          .antMatcher("/level2/**").hasRole("VIP2")
          .antMatcher("/level3/**").hasRole("VIP3")
		;
        //开启登录功能
      	http.formLogin();
        //1 /login 会重定向到登录页
        //2 登录失败 会
        
        //开启自动配置的退出登录
        http.logout();
        
        //开启记住我功能
        http.rememberMe();
    }   
}
```

#### 使用自己定制的登录页面

```java
http.formLogin().loginPage("/userlogin");
```

```java
 protected void configure(HttpSecurity http) throws Exception {
 		//定制请求的授权规则
	    http.authorizeRequests()
          .antMatcher("/").permitAll();
          .antMatcher("/level1/**").hasRole("VIP1")
          .antMatcher("/level2/**").hasRole("VIP2")
          .antMatcher("/level3/**").hasRole("VIP3")
		;
        //开启登录功能
      	//http.formLogin();
      	http.formLogin().usernameParameter("user").passParameter("pwd").loginPage("/userlogin");;
        //1 /login 会重定向到登录页
        //2 登录失败 会重定向到/login?error
     	//3 更多详细规定
       	//4 默认post形式的/login表示处理登录 使用username password作为参数
     	// 一旦自定义了登录页/userlogin 那么/userlogin get方式是访问登录页 默认/userlogin的post方式是处理请求 如果想要修改处理请求的地址 按如下方式
        //http.formLogin().usernameParameter("user").passParameter("pwd").loginPage("/userlogin").loginProcessingUrl("/login");
        //使用.loginProcessingUrl("/login") 指定处理请求的url地址	
     
     
     	//自定义登录页
	    //http.formLogin().loginPage("/userlogin");
        
        //开启自动配置的退出登录
        http.logout();
        
        //开启记住我功能
        http.rememberMe();
 }
```

#### 自定义的登录页 记住我功能

自定义的 页面的记住我功能，需要向后台提交的参数默认是 rememberme

可以进行配置修改 指定参数名称

```java
http.rememberMe().rememberMeParameter("remeber");
```



## 十四、Spring Boot 与分布式

分布式 Dubbo/Zookeeper SpringBoot/Cloud

### P31 尚硅谷_分布式-dubbo简介

#### 一、分布式应用

在分布式系统中，国内常用zookeeper+dubbo组合；而Spring Boot推荐使用全栈的Spring，SpringBoot+SpringCloud

- **Zookeeper**

  Zookeeper是一个分布式、开源的分布式应用程序协调服务。它是一个为 分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。

- **Dubbo**

  Dubbo是Alibaba开源的分布式服务框架，它最大的特点是按照分层的方式来架构，使用这种方式可以使各个层之间解耦合（或者最大限度的松耦合）。从服务模型的角度来看，Dubbo采用的是一种非常简单的模型，要么是提供方提供服务，要么是消费方消费服务，所以基于这一点可以抽象出服务提供方（Provider）和服务消费方（Consumer）两个角色。

- **Dubbo架构图**

  ![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.west.cn%2Finfo%2Fupload%2F20180618%2Ffwxafvbvcrn.png&refer=http%3A%2F%2Fwww.west.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1614250224&t=b572df46857467774cf22126e3055580)

### P32 尚硅谷_分布式-docker安装zookeeper
##### Zookeeper安装

```bash
docker pull zookeeper
docker images
docker run --name zookeeper-01  -p 2181:2181 --restart always -d zookeeper 
#2181 与客户端进行交互
#2888 集群端口
#3888 选举端口 -p 2888:2888 -P3888:3888 
docker ps #检查zookeeper是否启动
```

### P33 尚硅谷_分布式-SpringBoot、Dubbo、Zookeeper整合

#### 3.1 搭建项目

##### 3.1.0 创建父工程 空工程

##### 3.1.1 创建子模块 服务提供者provider-ticket 

###### 3.1.1.1 创建工程时，选择Web依赖

###### 3.1.1.2 创建接口TicketService

```java
public inteface TicketService {
	public String getTicket();
}
```

###### 3.1.1.3 接口的实现 TicketServiceImpl

```java
public inteface TicketServiceImp implements  TicketService{
	public String getTicket(){
		return "xxx";
	}
}
```



##### 3.1.2 创建子模块 服务消费者consumer-user

###### 3.1.2.1 创建子模consumer-user 选中Web依赖

###### 3.1.2.2 创建接口UserService

```java
public interface UserService{
    void hello();
}
```

###### 3.1.2.3 创建实现类UserServiceImpl

```java
public interface UserServiceImpl implements UserService {
    
    //这里需要使用 TicketService中的方法
    
    void hello(){
    	// ticketService.getTicket();
    }
}
```

#### 3.2 服务提供者

服务提供者注册到注册中心

##### 3.2.1 整合dubbo 引入dubbo和zkclient依赖

```
dubbo-spring-boot-starter
<!--zookeeper 客户端工具 -->
zkClient
```

##### 3.2.2 配置文件 配置dubbo扫描包和注册中心地址

```properties
dubbo.application.name=provider-ticket
dubbo.registry.address=zookeeper://192.168.1.110:2181
dubbo.scan.base-packages=com.beyondsoft.ticket.service
```

##### 3.2.3 使用@Service将服务发布出去

```java
@Component //交由spring管理
@Service  //这里是alibaba的 @Service
public class TicketServiceImpl implements TicketService{
    public String getTicket(){ return "xxx"};
}
```

#### 3.3 服务消费者

##### 3.3.1 整合dubbo 引入dubbo和zkclient依赖

```xml
<dependency>
    <groupId>com.alibaba.boot</groupId>
    <artifictId>dubbo-spring-boot-stater</artifictId>
    <version>0.1.0</versuib>
</dependency>
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifictId>zkclient</artifictId>
    <version>0.1.0</versuib>
</dependency>
```

##### 3.3.2 配置dubbo的注册中心地址

```properties
dubbo.application.name=consumer-user
dubbo.registry.address=zookeeper://192.168.1.110:2181
```

##### 3.3.3 使用@Reference 调用用远程服务

Ticket接口如果在 服务提供者模块， 那么这里如果要使用 需要复制一份过来，并且要求 **全类名一致** （包名和类名完全一致）

另一种更好的实现方式 是将接口 提取到 单独的模块中 ，服务提供者和服务消费者 依赖这个模块

```java
@Service
public class UserService{
    @Reference //远程调用
    TicketService ticketService;
    
    public void hello(){
        String ticket = ticketService.getTicket();
        System.out.println("买到票了："+ticket);
    }
}
```



### P34 尚硅谷_分布式-SpringCloud-Eureka注册中心

#### 三、Spring Boot和Spring Cloud

##### Spring Cloud 与Dobbbo的区别

- Dubbo是一个分布式服务框架 ，它主要解决服务之间远程过程调用问题（RPC问题）
- SpringCloud 是一个分布式的整体解决方案，分布式系统中存在的问题都有解决方案，如服务配置管理、注册与发现、熔断机制、路由等，SpringCloud都有解决方案

##### SpringCloud分布式开发五大常用组件

- 服务发现——Netflix Eureka
- 客户端负载均衡 —— Netflix Ribbon
-  断路器——Netflix  Hystrix
- 服务网关——Netflix  Zuul
- 分布式配置——SPring Cloud Config

#### 4.1 项目环境搭建

##### 4.1.1 创建空项目 作为父工程

##### 4.1.2 子模块 注册中心eureka-server

​	选择Eureka Server 依赖

##### 4.1.3 子模块 服务提供者provider -ticket

​	选择Eureka Discovery依赖

##### 4.1.4 子模块 服务消费者consumer-user

#### 4.2 注册中心

##### 4.2.1 注册中心配置

```yml
server:
  port: 9761
eureka:
  instance:
    hostname: eureka-server  #eureka实例的主机名
  client:
    register-with-eureka: false  #不把自己注册到注册中心
    fetch-registry: false #不从eureka中获取 服务注册信息列表
    service-url: 
      defaultZone: http://localhost:8761/eureka
```

##### 4.2.2 启用注册中心

```java
@EnableEurekaServer  //启用注册中心
@SpringBootApplication
public class EurekaServer{

}
```

##### 4.2.3 测试 访问Eureka管理控制台

```
http://localhost:8761/eureka
```

### P35 尚硅谷_分布式-服务注册

#### 4.3 服务提供者

##### 4.3.1 编写服务Service和Controller

TicketService

TicketController

##### 4.3.2 服务提供者注册到Eureka

```yaml
server:
  port: 8001
spring:
  application:
    name: provider-ticket
eureka:
  instance:
    prefer-ip-address: #注册的时候 使用服务ip地址
  client:   
    service-url: true
      defaultZone: http://localhost:8761/eureka    
```

##### 4.3.3 打包为两个地址8001 8002

​	启动运行两个服务，两个服务提供者实例 都注册到Eureka

### P36 尚硅谷_分布式-服务发现&消费

#### 4.4 服务消费者

##### 4.3.1编写UserController

```java
@RestControll
public class UserController{
    
    @Autowired
    RestTemplate restTemplate;
    
	@GetMapping("/buy")
	public String byTicket(String name){
        String ticket = restTemplate.getForObject("http://PROVIDER-TICKET/ticket",String.class);
		return name+"购买了" +ticket;
	}
}
```

##### 4.3.2 配置文件

```yaml
server:
  port: 8200
spring:
  application:
    name: consumer-user
    
eureka:
  instance:
    prefer-ip-address: #注册的时候 使用服务ip地址
  client:   
    service-url: true
      defaultZone: http://localhost:8761/eureka    
```

##### 4.3.3 发现服务

```java
@EnableDiscoveryClient //开启服务发现功能
@SpringBootApplication
public class ConsmerUserApplication{
    public void main(String[] args){
        SpringApplication.run(ConsmerUserApplication.class,args);        
    }
    
    @Bean //注入RestTemplate
    @LoadBalanced //启用负载均衡
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```



## 十五、Spring Boot 与热部署

### P37 尚硅谷_热部署-devtools开发热部署

#### 一、热部署

在开发中我们修改了一个Java文件后，想要看到效果不得不重启应用，这导致大量的实际花费，我们希望不重启应用的情况下，程序可以自动部署（热部署）。

有以下四种方式，可以实现热部署。

##### 1、模板引擎

在使用SpringBoot开发的情况下，金庸用模板引擎的cache

页面模板概念ctrl+F9可以重新编译当前页面并生效

##### 2、Spring Loaded

Spring官方提供的热部署程序，实现修改类文件的热部署

- 下载Spring Loaded(项目地址 http://github.com/spring-proejcts/spring-loded)

- 添加运行时参数

  ```
  -javaagent:C:/springloaded-1.2.5.RELEASE.jar -noverify
  ```

##### 3、JReble

- 收费的热不是插件 Idea 和Eclipse
- 安装插件使用即可

##### 4、Spring Boot Devtools(推荐)

###### 4.1 引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

###### 4.2 使用方法

IDEA中 ： **Ctrl + F9**

Eclipse中：**Ctrl+S**

## 十六、Spring Boot 与监控管理

### P38 尚硅谷_监管-监管端点测试

#### 一、监控管理

通过引入spring-boot-starter-actuator ,可以使用Spring Boot为我们提供的准生产环境下的应用监控和管理功能，我们可以通过Http JMX SSH协议来进行操作，自动得到审计、健康及指标信息等。

##### 使用步骤

- 引入spring-boot-starter-actuator
- 通过http方式访问监控端点
- 可以进行shutdown(POST提交，默认关闭)

##### 搭建工程环境

​	创建工程springboot-actuator,	并添加DevTools、Web、Actuator模块依赖

​	启动时会看到 监控信息的接口

​	访问这些端点(接口)时，默认是401 未授权

##### 配置开放端点

访问这些端点(接口)时，默认是401 未授权，需要在application.yml中 设置不被安全认证管理

```properties
management.security.enabled=false  #监控管理是否被权限认证所管理
```



##### 监控和管理端点

| 端点名      | 描述                        |
| ----------- | --------------------------- |
| autoconfig  | 所有自动配置信息            |
| auditevents | 审计事件                    |
| beans       | 所有Bean的信息              |
| configprops | 所有配置属性                |
| dump        | 线程状态信息                |
| env         | 当前环境信息                |
| health      | 应用健康状况                |
| info        | 当前应用信息                |
| metrics     | 应用的各项指标              |
| mappings    | 应用@RequestMapping映射路径 |
| shutdown    | 关闭当前应用（默认关闭）    |
| trace       | 追踪信息（最新的http请求）  |

##### info信息

```properties
info.app.id
info.app.version=1.0.1
```

##### info信息中显示 git提交信息

git.properties

GitProperties

```properties
git.branche=master
git.commit.id=aa
git,commite.time=2019-02-20
```

##### heapdump

/heapdump  下载当前内存快照 分析应用问题

##### 禁用部分属性

```properties
endpoints.metrics.enabled=false
```

##### 远程关闭应用

```properties
#启用
endpoints.shutdown.enabled=true
```

远程关闭

使用postman 发送一个 post请求 http://localhost:8080/shutdown



### P39 尚硅谷_监管-定制端点

#### 二、定制端点信息

- 定制端点一般通过endpoints+端点名+属性名来设置。

  ```properties
  endpint.beans.id=mybean  #定制端点 修改端点名
  ```

  ```
  http://localhost:8080/beans
  变为了
  http://localhost:8080/mybean
  ```

  ```properties
  #定制访问路径
  endpint.beans.path=/bean
  ```

  ```
  访问变为了
  http://localhost:8080/bean
  ```

- 修改端点id（endpoints.beans.id=mybeans）

- 开启远程应用关闭功能（endpoints.shutdown.enabled=true）

- 关闭端点（endpoints.beans.enabled=false）

  ```properties
  #关闭所有端点
  endpoints.enable=false
  #只开启某个
  endpoints.beans.enabled=true
  ```

- 开启所需端点

  ```properties
  endpoints.enabled=false
  endpoints.beans.enabled=true
  ```

- 定制端点访问根路径

  ```properties
  management.context-path=/manage  #这样可以接口springsecurity 对manage 继续访问控制
  ```

- 关闭http端点

  ```properties
  management.port=8081 # 指定管理端点的 端口
  management.port=-1  #指定不存在的端口 也就意味着禁用了管理端点
  ```

  

### P40 尚硅谷_监管-自定义HealthIndicator

```properties
management.port=8081
management.context-path=/manage
```

```
http://localhost:8080/health # 默认的健康信息
http://localhost:8081/manage/health # 指定端口和指定路径后的健康信息
```

监控Redis信息

引用了Redise后 如果连接正常 在 health中 就会返回redis正常的信息

#### 1- 自定义监控状态检查指示器步骤

##### 1.1、编写一个指示器 实现接口HealthIndicator

##### 1.2、指示器的名字，必须命名为XxxxHealthIndicator

##### 1.3、加入到容器中



##### 1.4- 自定义监控状态检查指示器示例

```java
import com.beyondsoft.springboot-actuator.health;

@Component //3-加入到容器中 被spring管理
public class MyAppHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {

        //自定义的检查方法
        //return Health.up().build(); //代表健康
        return Health.down().withDetail("msg","服务异常").build();
    }
}
```

