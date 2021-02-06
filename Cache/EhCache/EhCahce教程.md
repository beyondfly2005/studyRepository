### EhCache教程

### 一、EhCache简介

#### 1.1、基本介绍

EhCache 是一个纯Java的进程内缓存框架，具有快速、精干等特点，是Hibernate中默认CacheProvider。

Ehcache是一种广泛使用的开源Java分布式缓存。主要面向通用缓存,Java EE和轻量级容器。它具有内存和磁盘存储，缓存加载器，缓存扩展，缓存异常处理程序，一个gzip缓存servlet过滤器，支持REST和SOAP api等特点。



Ehcache 在应用程序中的位置：

![img](http://www.oschina.net/uploads/img/200812/28190647_yVIv.png)

#### 1.2、 主要的特性

1. 快速
2. 简单
3. 多种缓存策略
4. 缓存数据有两级：内存和磁盘，因此无需担心容量问题
5. 缓存数据会在虚拟机重启的过程中写入磁盘
6. 可以通过RMI、可插入API等方式进行分布式缓存
7. 具有缓存和缓存管理器的侦听接口
8. 支持多缓存管理器实例，以及一个实例的多个缓存区域
9. 提供Hibernate的缓存实现

#### 1.3、 集成

　　可以单独使用，一般在第三方库中被用到的比较多（如mybatis、shiro等）ehcache 对分布式支持不够好，多个节点不能同步，通常和redis一块使用

#### 1.4、常间的缓存方案

​	JAVA缓存实现方案有很多，最基本的自己使用Map去构建缓存，或者使用memcached或Redis，但是上述两种缓存框架都要搭建服务器，而Map自行构建的缓存可能没有很高的使用效率，那么我们可以尝试一下使用Ehcache缓存框架。

- 使用Map构建

  SpringBoot默认的缓存方案SimpleCache 就是基于Map(ConcurrentMapCacheManager)构建的

- EhCache

- memcached

- Redis

Ehcache主要基于内存缓存，磁盘缓存为辅的，使用起来方便。

#### 1.5 ehcache 和 redis 比较

　　ehcache直接在jvm虚拟机中缓存，速度快，效率高；但是缓存共享麻烦，集群分布式应用不方便。

　　redis是通过socket访问到缓存服务，效率比Ehcache低，比数据库要快很多，处理集群和分布式缓存方便，有成熟的方案。如果是单个应用或者对缓存访问要求很高的应用，用ehcache。如果是大型系统，存在缓存共享、分布式部署、缓存内容很大的，建议用redis。

 　　ehcache也有缓存共享方案，不过是通过RMI或者Jgroup多播方式进行广播缓存通知更新，缓存共享复杂，维护不方便；简单的共享可以，但是涉及到缓存恢复，大数据缓存，则不合适。

### 二、EhCache的入门示例

这里我通过一个简单的Demo项目，来学习使用EhCache。

##### 1.1 创建项目

创建一个Maven项目 项目名称：ehcache-demo

##### 1.2 引入依赖

在pom.xml文件中，引入对EhCache的maven依赖

```xml
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
    <version>2.10.3</version>
</dependency>
```

##### 1.3  添加EhCache配置文件

在resources目录下，新建ehcache.xml文件，并写入如下内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache>
   <!-- 
		diskStore:磁盘存储，将缓存中暂时不使用的对象,转移到硬盘,类似于Windows系统的虚拟内存
            path:指定在硬盘上存储对象的路径
   -->
   <diskStore path="C:\ehcache" />
    
   <!-- 
        defaultCache:默认的缓存配置信息,如果不加特殊说明,则所有对象按照此配置项处理
        maxElementsInMemory:设置了缓存的上限,最多存储多少个记录对象
        eternal:代表对象是否永不过期
        overflowToDisk:当内存中Element数量达到maxElementsInMemory时，Ehcache将会Element写到磁盘中
   -->
   <defaultCache
      maxElementsInMemory="100"
      eternal="true"
      overflowToDisk="true"/>
      
    <cache 
      name="mycache"
      maxElementsInMemory="100"
      eternal="true"
      overflowToDisk="true"/>
 
</ehcache>
```

##### 1.4 EhCache的测试使用

这里，我们再新建一个测试类，来学习使用EhCache

```java
package com.beyondsoft.ehcache;
 
import net.sf.ehcache.Cache;
import net.sf.ehcache.CacheManager;
import net.sf.ehcache.Element;
 
public class EhcacheDemo {
 
    public static void main(String[] args) {
        //1. 根据ehcache.xml配置文件创建Cache管理器
        CacheManager manager = CacheManager.create("./src/main/resources/ehcache.xml");
        //2. 获取指定Cache
        Cache cache = manager.getCache("mycache");
        //3. 实例化一个元素
        Element elemet = new Element("msg","hello world!"); 
        //4. 将元素添加到Cache中
        cache.put(elemet); 
        
        //6. 根据Key获取缓存元素
        Element element2=cache.get("msg"); 
        System.out.println(element2);
        System.out.println(element2.getObjectValue());
         
        cache.flush(); // 刷新缓存
        manager.shutdown(); // 关闭缓存管理器
    }
}
```

### 三、EhCache 常用配置项详解

EhCache 给我们提供了丰富的配置来配置缓存的设置；

这里列出一些常见的配置项：

##### diskStore元素

​		diskStore为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。

​		参数解释如下：
​		user.home – 用户主目录
​		user.dir  – 用户当前工作目录
​		java.io.tmpdir – 默认临时文件路径

- path 缓存路径

##### defaultCache元素

​		默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。

##### cache元素

​		以下是cache元素的属性： 

- name：缓存名称  

- maxElementsInMemory：内存中最大缓存对象数  

- maxElementsOnDisk：硬盘中最大缓存对象数，若是0表示无穷大  

- eternal：true表示对象永不过期，此时会忽略timeToIdleSeconds和timeToLiveSeconds属性，默认为false  

- overflowToDisk：true表示当内存缓存的对象数目达到了maxElementsInMemory界限后，会把溢出的对象写到硬盘缓存中。

  **注意**：如果缓存的对象要写入到硬盘中的话，则**该对象必须实现了Serializable接口**才行。  

- diskSpoolBufferSizeMB：磁盘缓存区大小，默认为30MB。每个Cache都应该有自己的一个缓存区。  

- diskPersistent：是否缓存虚拟机重启期数据  

- diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认为120秒  

- timeToIdleSeconds： 设定允许对象处于空闲状态的最长时间，以秒为单位。当对象自从最近一次被访问后，如果处于空闲状态的时间超过了timeToIdleSeconds属性值，这个对象就会过期，EHCache将把它从缓存中清空。

  只有当eternal属性为false，该属性才有效。如果该属性值为0，则表示对象可以无限期地处于空闲状态  

- timeToLiveSeconds：设定对象允许存在于缓存中的最长时间，以秒为单位。当对象自从被存放到缓存中后，如果处于缓存中的时间超过了 timeToLiveSeconds属性值，这个对象就会过期，EHCache将把它从缓存中清除。

  只有当eternal属性为false，该属性才有效。如果该属性值为0，则表示对象可以无限期地存在于缓存中。

  timeToLiveSeconds必须大于timeToIdleSeconds属性，才有意义  

- memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。

  可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。  

**配置参数说明**

```xml
<!--
       diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
       user.home – 用户主目录
       user.dir  – 用户当前工作目录
       java.io.tmpdir – 默认临时文件路径
     -->
    <diskStore path="java.io.tmpdir/Tmp_EhCache"/>
    <!--
       defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
     -->
    <!--
      name:缓存名称。
      maxElementsInMemory:缓存最大数目
      maxElementsOnDisk：硬盘最大缓存个数。
      eternal:对象是否永久有效，一但设置了，timeout将不起作用。
      overflowToDisk:是否保存到磁盘，当系统当机时
      timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
      timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
      diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
      diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
      diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
      memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
      clearOnFlush：内存数量最大时是否清除。
      memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
      FIFO，first in first out，这个是大家最熟的，先进先出。
      LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
      LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
   -->

```

### 四、Ehcache持久化配置

Ehcache默认配置的话 为了提高效率，所以有一部分缓存是在内存中，然后达到配置的内存对象总量，则才根据策略持久化到硬盘中，这里是有一个问题的，假如系统突然中断运行 那内存中的那些缓存，直接被释放掉了，不能持久化到硬盘；这种数据丢失，对于一般项目是不会有影响的，但如果做的是爬虫系统，EhCache是用来判断Url是否重复的，这时候数据不能丢失，不能将放入EhCache的数据放入内存中；

这时候我们就需要通过Ehcache配置，来实现缓存的持久化，不存内存中。

最关键的配置是 

```
maxElementsInMemory 设置成1  #只要有1个就存入硬盘
```

参考配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
 
<ehcache>
   <!-- 
         磁盘存储:将缓存中暂时不使用的对象,转移到硬盘,类似于Windows系统的虚拟内存
          path:指定在硬盘上存储对象的路径
   -->
   <diskStore path="C:\ehcache" />
    
   <!-- 
        defaultCache:默认的缓存配置信息,如果不加特殊说明,则所有对象按照此配置项处理
        maxElementsInMemory:设置了缓存的上限,最多存储多少个记录对象
        eternal:代表对象是否永不过期
        overflowToDisk:当内存中Element数量达到maxElementsInMemory时，Ehcache将会Element写到磁盘中
   -->
   <defaultCache
      maxElementsInMemory="100"
      eternal="true"
      overflowToDisk="true"/>
 
    <!-- 
        maxElementsInMemory设置成1，overflowToDisk设置成true，只要有一个缓存元素，就直接存到硬盘上去
        eternal设置成true，代表对象永久有效
        maxElementsOnDisk设置成0 表示硬盘中最大缓存对象数无限大
        diskPersistent设置成true表示缓存虚拟机重启期数据 
     -->
    <cache 
      name="mycache"
      maxElementsInMemory="1" 
      eternal="true"
      overflowToDisk="true" 
      maxElementsOnDisk="0"
      diskPersistent="true"/>
 
</ehcache>
```



### 五、EhCache与Sping集成

Spring 提供了对缓存功能的抽象：即允许绑定不同的缓存解决方案（如Ehcache），但本身不直接提供缓存功能的实现。它支持注解方式使用缓存，非常方便。

#### 5.1引入依赖

pom.xml 引入spring和ehcache

```xml
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <junit.version>4.10</junit.version>
    <spring.version>4.2.3.RELEASE</spring.version>
  </properties> 
  <dependencies>
    <!-- springframework -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-support</artifactId>
        <version>${spring.version}</version>
    </dependency>
    
    <dependency>
        <groupId>net.sf.ehcache</groupId>
        <artifactId>ehcache</artifactId>
        <version>2.10.3</version>
    </dependency>
      <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>${junit.version}</version>
    </dependency>
    <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-test</artifactId>
         <version>${spring.version}</version>
     </dependency>
  </dependencies>    
```

#### 5.2 配置ehcache

resources目录下添加ehcache.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd">

  <!-- 磁盘缓存位置 -->
  <diskStore path="java.io.tmpdir/ehcache" />

  <!-- 默认缓存 -->
  <defaultCache
          maxEntriesLocalHeap="10000"
          eternal="false"
          timeToIdleSeconds="120"
          timeToLiveSeconds="120"
          maxEntriesLocalDisk="10000000"
          diskExpiryThreadIntervalSeconds="120"
          memoryStoreEvictionPolicy="LRU">
    <persistence strategy="localTempSwap"/>
  </defaultCache>

  <!-- helloworld缓存 -->
  <cache name="HelloWorldCache"
         maxElementsInMemory="1000"
         eternal="false"
         timeToIdleSeconds="5"
         timeToLiveSeconds="5"
         overflowToDisk="false"
         memoryStoreEvictionPolicy="LRU"/>

  <cache name="UserCache"
         maxElementsInMemory="1000"
         eternal="false"
         timeToIdleSeconds="1800"
         timeToLiveSeconds="1800"
         overflowToDisk="false"
         memoryStoreEvictionPolicy="LRU"/>
</ehcache>
```

#### 5.3 配置spring-ehcache

resources目录中配置spring-base.xml和spring-ehcache.xml

spring-base.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

    <context:component-scan base-package="com.beyondsoft.ehcache.*"/>
</beans>
```

spring-ehcache.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:cache="http://www.springframework.org/schema/cache"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/cache
        http://www.springframework.org/schema/cache/spring-cache-3.2.xsd">

  <description>ehcache缓存配置管理文件</description>

  <!-- 启用缓存注解开关 -->
  <cache:annotation-driven cache-manager="cacheManager"/>

  <bean id="cacheManager" class="org.springframework.cache.ehcache.EhCacheCacheManager">
    <property name="cacheManager" ref="ehcache"/>
  </bean>

  <bean id="ehcache" class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">
    <property name="configLocation" value="classpath:ehcache.xml"/>
  </bean>

</beans>
```

#### 5.4 Service中使用

com.beyondsoft.ehcache.service创建EhcacheService和EhcacheServiceImpl

EhcacheService.java

```java
public interface EhcacheService {
 
    // 测试失效情况，有效期为5秒
    public String getTimestamp(String param);
 
    public String getDataFromDB(String key);
 
    public void removeDataAtDB(String key);
 
    public String refreshData(String key);
  
    public User findById(String userId);
 
    public boolean isReserved(String userId);
 
    public void removeUser(String userId);
 
    public void removeAllUser();
}
```

EhcacheServiceImpl.java

```java
import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.CachePut;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;
 
@Service
public class EhcacheServiceImpl implements EhcacheService{
 
    // value的值和ehcache.xml中的配置保持一致
    @Cacheable(value="HelloWorldCache", key="#param")
    public String getTimestamp(String param) {
        Long timestamp = System.currentTimeMillis();
        return timestamp.toString();
    }
 
    @Cacheable(value="HelloWorldCache", key="#key")
    public String getDataFromDB(String key) {
        System.out.println("从数据库中获取数据...");
        return key + ":" + String.valueOf(Math.round(Math.random()*1000000));
    }
 
    @CacheEvict(value="HelloWorldCache", key="#key")
    public void removeDataAtDB(String key) {
        System.out.println("从数据库中删除数据");
    }
 
    @CachePut(value="HelloWorldCache", key="#key")
    public String refreshData(String key) {
        System.out.println("模拟从数据库中加载数据");
        return key + "::" + String.valueOf(Math.round(Math.random()*1000000));
    }
 
    // ------------------------------------------------------------------------
    @Cacheable(value="UserCache", key="'user:' + #userId")   
    public User findById(String userId) { 
        System.out.println("模拟从数据库中查询数据");
        return new User(1, "mengdee");          
    } 
 
    @Cacheable(value="UserCache", condition="#userId.length()<12")   
    public boolean isReserved(String userId) {   
        System.out.println("UserCache:"+userId);   
        return false;   
    }
 
    //清除掉UserCache中某个指定key的缓存   
    @CacheEvict(value="UserCache",key="'user:' + #userId")   
    public void removeUser(String userId) {   
        System.out.println("UserCache remove:"+ userId);   
    }   
 
    //allEntries：true表示清除value中的全部缓存，默认为false
    //清除掉UserCache中全部的缓存   
    @CacheEvict(value="UserCache", allEntries=true)   
    public void removeAllUser() {   
       System.out.println("UserCache delete all");   
    }
}
```

User Pojo

User .java

```java
public class User {
    private int id;
    private String name;
     
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
     
    public User() {
    }
     
    public User(int id, String name) {
        super();
        this.id = id;
        this.name = name;
    }
     
    @Override
    public String toString() {
        return "User [id=" + id + ", name=" + name + "]";
    }
}
```

#### 5.5 基于注解的使用方法

Spring对缓存的支持类似于对事务的支持。
首先使用注解标记方法，相当于定义了切点，然后使用Aop技术在这个方法的调用前、调用后获取方法的入参和返回值，进而实现了缓存的逻辑。

##### **@Cacheable**注解

表明所修饰的方法是可以缓存的：当第一次调用这个方法时，它的结果会被缓存下来，在缓存的有效时间内，以后访问这个方法都直接返回缓存结果，不再执行方法中的代码段。
这个注解可以用condition属性来设置条件，如果不满足条件，就不使用缓存能力，直接执行方法。
可以使用key属性来指定key的生成规则。

@Cacheable 支持如下几个参数：

**value**：缓存位置名称，不能为空，如果使用EHCache，就是ehcache.xml中声明的cache的name, 指明将值缓存到哪个Cache中

**key**：缓存的key，默认为空，既表示使用方法的参数类型及参数值作为key，支持SpEL，如果要引用参数值使用井号加参数名，如：#userId，一般来说，我们的更新操作只需要刷新缓存中某一个值，所以定义缓存的key值的方式就很重要，最好是能够唯一，因为这样可以准确的清除掉特定的缓存，而不会影响到其它缓存值 ，本例子中使用实体加冒号再加ID组合成键的名称，如"user:1"、"order:223123"等

**condition**：触发条件，只有满足条件的情况才会加入缓存，默认为空，既表示全部都加入缓存，支持SpEL

```java
// 将缓存保存到名称为UserCache中，键为"user:"字符串加上userId值，如 'user:1'
@Cacheable(value="UserCache", key="'user:' + #userId")
public User findById(String userId) {
    return (User) new User("1", "mengdee");
}
// 将缓存保存进UserCache中，并当参数userId的长度小于12时才保存进缓存，默认使用参数值及类型作为缓存的key
// 保存缓存需要指定key，value， value的数据类型，不指定key默认和参数名一样如："1"
@Cacheable(value="UserCache", condition="#userId.length() < 12")
public boolean isReserved(String userId) {
    System.out.println("UserCache:"+userId);  
    return false;
}　　
```

##### **@CachePut**注解

与@Cacheable不同，@CachePut不仅会缓存方法的结果，还会执行方法的代码段。它支持的属性和用法都与@Cacheable一致。

##### **@CacheEvict注解**

与@Cacheable功能相反，@CacheEvict表明所修饰的方法是用来删除失效或无用的缓存数据。

@CacheEvict 支持如下几个参数：

**value**：缓存位置名称，不能为空，同上
**key**：缓存的key，默认为空，同上

**condition**：触发条件，只有满足条件的情况才会清除缓存，默认为空，支持SpEL
**allEntries**：true表示清除value中的全部缓存，默认为false

```java
//清除掉UserCache中某个指定key的缓存  
@CacheEvict(value="UserCache",key="'user:' + #userId")  
public void removeUser(User user) {
    System.out.println("UserCache"+user.getUserId());  
}
//清除掉UserCache中全部的缓存  
@CacheEvict(value="UserCache", allEntries=true)  
public final void setReservedUsers(String[] reservedUsers) {
    System.out.println("UserCache deleteall");  
}
```

#### 5.6 测试

SpringTestCase.java

```java
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.AbstractJUnit4SpringContextTests;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
 
@ContextConfiguration(locations = {"classpath:spring-base.xml","classpath:spring-ehcache.xml"})
@RunWith(SpringJUnit4ClassRunner.class)
public class SpringTestCase extends AbstractJUnit4SpringContextTests{
 
}
```

EhcacheServiceTest.java

```java
import org.junit.Test;
import org.springframework.beans.factory.annotation.Autowired;
import com.gdut.ehcache.EhcacheService;
 
 
 
public class EhcacheServiceTest extends SpringTestCase{
 
    @Autowired //@Autowired 是通过 byType 的方式去注入的， 使用该注解，要求接口只能有一个实现类。
    private EhcacheService ehcacheService;
 
    // 有效时间是5秒，第一次和第二次获取的值是一样的，因第三次是5秒之后所以会获取新的值
    @Test
    public void testTimestamp() throws InterruptedException{
        System.out.println("第一次调用：" + ehcacheService.getTimestamp("param"));
        Thread.sleep(2000);
        System.out.println("2秒之后调用：" + ehcacheService.getTimestamp("param"));
        Thread.sleep(4000);
        System.out.println("再过4秒之后调用：" + ehcacheService.getTimestamp("param"));
    }
 
    @Test
    public void testCache(){
        String key = "zhangsan";
        String value = ehcacheService.getDataFromDB(key); // 从数据库中获取数据...
        ehcacheService.getDataFromDB(key);  // 从缓存中获取数据，所以不执行该方法体
        ehcacheService.removeDataAtDB(key); // 从数据库中删除数据
        ehcacheService.getDataFromDB(key);  // 从数据库中获取数据...（缓存数据删除了，所以要重新获取，执行方法体）
    }
 
    @Test
    public void testPut(){
        String key = "mengdee";
        ehcacheService.refreshData(key);  // 模拟从数据库中加载数据
        String data = ehcacheService.getDataFromDB(key);
        System.out.println("data:" + data); // data:mengdee::103385
 
        ehcacheService.refreshData(key);  // 模拟从数据库中加载数据
        String data2 = ehcacheService.getDataFromDB(key);
        System.out.println("data2:" + data2);   // data2:mengdee::180538   
    }
 
 
    @Test
    public void testFindById(){
        ehcacheService.findById("2"); // 模拟从数据库中查询数据
        ehcacheService.findById("2");
    }
 
    @Test
    public void testIsReserved(){
        ehcacheService.isReserved("123");
        ehcacheService.isReserved("123");
    }
 
    @Test
    public void testRemoveUser(){
        // 线添加到缓存
        ehcacheService.findById("1");
 
        // 再删除
        ehcacheService.removeUser("1");
 
        // 如果不存在会执行方法体
        ehcacheService.findById("1");
    }
 
    @Test
    public void testRemoveAllUser(){
        ehcacheService.findById("1");
        ehcacheService.findById("2");
 
        ehcacheService.removeAllUser();
 
        ehcacheService.findById("1");
        ehcacheService.findById("2");
 
//      模拟从数据库中查询数据
//      模拟从数据库中查询数据
//      UserCache delete all
//      模拟从数据库中查询数据
//      模拟从数据库中查询数据
    } 
}
```



### 六、EhCache与SpingBoot集成

#### 6.1 引入依赖

```xml
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

#### 6.2 配置文件

##### 配置项目属性文件

application.properties

```properties
spring.cache.ehcache.config=classpath:ehcache.xml
```

application.yml

```yml
spring:
  cache:
    ehcache:
      config: classpath:ehcache.xml
```

##### 配置ehcache的配置文件

新建ehcache.xml 放在resources目录 并写入如下配置

```xml
<ehcache>
    <diskStore path="java.io.tmpdir/Tmp_EhCache"/>
    <defaultCache            
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>
 
    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>

</ehcache>
```

#### 6.3 开启缓存支持

##### 方法1：启动类标注@EnableCaching

```java
@EnableCaching
@SpringBootApplication
public class EhCacheServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(EhCacheServerApplication.class, args);
    }
}
```

##### 方法2：配置类上标注

```java
@Configuration
@EnableCaching
public class EhCacheConfig {

}
```

#### 6.4 项目中使用

**一般情况下，我们在Sercive层进行对缓存的操作**

```java
@Service
@CacheConfig(cacheNames = "GoodsType")
public class GoodsTypeService {

    @Autowired
    private GoodsTypeDao goodsTypeDao;

    @Cacheable
    public String save(String typeId) {
        return goodsTypeDao.save(typeId);
    }

    @CachePut
    public String update(String typeId) {
        return goodsTypeDao.update(typeId);
    }

    @CacheEvict
    public String delete(String typeId) {
        return goodsTypeDao.delete(typeId);
    }

    @Cacheable
    public String select(String typeId) {
        return goodsTypeDao.select(typeId);
    }
}
```

#### 6.5 **常用注解说明**

###### 方法上的注解

- @cache这个相当于save()操作
- @cachePut相当于update()操作，只要他标示的方法被调用，那么都会缓存起来，而@cache则是先看下有没已经缓存了，然后再选择是否执行方法。
- @CacheEvict相当于delete()操作。用来清除缓存用的。
- @Caching 是@Cacheable @CachePut @CacheEvict 组合注解；@Caching用来定义复杂的缓存规则

###### 类上的注解

- @CacheConfig  @CacheConfig(cacheNames = "GoodsType")指定整个类的缓存名称



**参考文档**

https://blog.csdn.net/qq_28988969/article/details/78210873

https://www.bilibili.com/video/BV1jt411T7Mj?t=58

https://www.sojson.com/blog/352.html

https://www.cnblogs.com/myseries/p/11370109.html