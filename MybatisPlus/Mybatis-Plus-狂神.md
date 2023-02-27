### MyBatisPlus最新完整教程

> 课程地址   https://www.bilibili.com/video/BV17E411N7KN
>
> 课程作者  遇见狂神说

### 1、Mybatis-Plus简介

#### Mybatis-Plus概述

需要基础：MyBatis、Spring、SpringMVC就可以学习这个了

为什么要学习Mybatis-Plus，它可以节省我们大量的开发时间，所有的CRUD操作代码都可以自动化完成

类似：JPA、TK-Mapper、Mybatis-Plus



##### 官网

https://baomidou.com/

为简化而生

##### 简介

[MyBatis-Plus (opens new window)](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis (opens new window)](http://www.mybatis.org/mybatis-3/)的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生

##### 特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作 **BaseMapper**
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求，以后见到的CRUD操作，他就
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作



### 2、快速入门体验

#### 快速入门

使用第三方组件：（套路）

1、导入对应的依赖

2、研究依赖如何配置

3、代码如何编写

4、提高扩展能力技术

快速开始 https://baomidou.com/guide/quick-start.html

**步骤**

1、创建数据库 mybatis_plus

2、创建 user表

```sql
create table user(
    id bigint(20) not null comment '主键ID',
    name varchar(30) null default null comment '姓名',
    age int(11) null default null comment '年龄',
    email varchar null default null comment '邮箱',
    primary key (id)
);

-- 真实开发中：
-- version 乐观锁
-- deleted 逻辑删除
-- gmt_create 创建时间  阿里规范
-- gmt_moidify 修改时间
```

3、编写项目 使用SpringBoot初始化项目

4、导入依赖

```xml
<!-- web -->
<dependency>
    <groupId></groupId>
    <artifactId>web</artifactId>    
</dependency>
<!--mysql驱动 -->

<!--lombok -->

<!--mybatis-plus -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>    
    <version>3.0.5</version>
</dependency>
```

说明：尽量不要同时导入mybatis和mybatisplus

5、连接数据库

```properties
#mysql 5.x
spring.datasource.usrname=root
spring.dtasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf8

# mysql 8驱动不同 需要增加时区配置
spring.datasource.usrname=root
spring.dtasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8

```

6、项目中使用

传统方式：pojo-dao-service-controller

​					dao (mybatis 配置maper.xml)

使用 mybatis-plus之后：

- ###### 6.1 创建实体类pojo

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User{
	private Long id;
	private String name;
	private Integer age;
	private String email;
}
```

- ###### 6.2 mapper接口

```java
@Repository //代表是持久层
public interface UserMapper extends BaseMapper<User>{
	//所有的CRUD操作都已经完成了
    //不需要向以前 配置一堆文件了
}
```

- ##### 6.3  使用

```java
@MapperScan("com.beyondsoft.mapper") //扫描mapper文件夹
@SpringBootApplication
public class MybatisPlusDemoApplication{
	public static void main(String args[]){
		SpringApplication.run();
	}
}
```

- ##### 6..4 测试代码

  测试类中测试

```java
@SpringBootTest
public class MybatisPlusApplicationTests{
    
    //继承了BaseMapper 所有的方法都来自父类
    // 可以编写自己的扩展方法
	@Autowired
    private userMapper
    
    @Test
    void test1(){
        //参数是一个Wrapper 条件构造器 这里我们先不用，使用null
        //查询全部用户
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println)
    }        
}
```



注意点： 在主启动类(或者配置类上) 去扫描我们的mapper包下的所有接口

- ```java
  @MapperScan("com,beyondsoft.mapper")
  ```

> 思考问题

​	SQL谁帮我们写的

​	方法哪里来的



### 3、配置日志输出

SQL现在是不可见的，希望看到SQL的

```properties
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.Stdoutimpl
```



### P4 插入及雪花片算法

测试插入

```java
public void testInsert(){
	User user = new User();
	user.setName("");
	user.setAge(3);
	user.setEmail("123@qq.com");
	int result= userMapper.insert(user);
	System.out.println(result);
	System.out.println(user);  //user 自动带了id
}
```

#### 主键生成策略

常用的主键生成策略（uuid，自增id，redis）

博客：分布式系统唯一ID生成方案

https://www.cnblogs.com/haoxinyue/p/5208136.html

- 自增id

- uuid：

没有排序

- redis

- 雪花片算法

  snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。

### P5 不同的主键策略

- IdType.AUTO  自增策略

```
1 实体类字段上 增加 @TableId(Type.AUTO)
2 数据库字段 也要是自增！
```

- IdType.NONE(0)  未设置主键
- INPUT 手动输入  需要自己配置ID
- ID_WORKER(3)   默认的全局唯一ID
- UUID(4)   全局唯一ID UUID
- ID_WORKER_STR(5)  ID_WORKER的字符串表示法



### P6 更新操作

```java
//测试更新
public void testUpdate(){
    User user = new User();
    user.setId(1);
    user.setName("张三变李四");
    user.setAge(18);  //通过条件自动拼接参数
    int i = userMapper.updateById(user); //参数是一个对象
}
```

### P7、自动填充处理

##### 自动填充

创建时间、修改时间，这些操作一般都是自动完成的，我们不希望手动更新！

阿里开发手册，gmt_create gmt_modify 格林威治时间  几乎所有的表都要配置上，需要自动化写入数值

##### 方式一： 数据库级别

创建时间：设置默认值 CURRENT_TIMESTAMP； 更新时间 ：navicat 设计表勾选：更新

实际工作中，不允许操作数据库

```java
public class User{
    //......
    private Date createTime;
    private Date updateTime;
}
```



##### 方式二：代码级别

先去掉前面方式一种设置的数据库的默认值 和勾选的更新

```java
public class User{
    //......
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
}
```

编写处理器

```java
package com.xx.handler; //与mapper同目录

@Component //加入到IoC容器中
@Slf4j
public class MyMetaObjectHandler implements MetaObjectHandler {
	@Override
    public void insertFill(MetaObject metaObject){
        log.info("start update fill......");
        this.setFieldValByName("createTime",new Date(),metaObject);
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
    
    public void insertFill(MetaObject metaObject){
        log.info("start update fill......");
        this.setFieldValByName("createTime",new Date(),metaObject);
    }
}
```

### P8、乐观锁

juc原子引用

 **乐观锁：**顾名思义：十分乐观，总是认为不会出现问题，无论干什么都不会上锁；如果出现了问题 就再次更新值测试

> ​	使用version字段 



**悲观锁：**顾名思义十分悲观，他总是认为总是出现问题，无论干什么都会上锁，再去操作！

乐观锁实现方式：

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

```sql
-- A线程
update user set name="aa" ,version=version+1 where id=2 and version=1

-- B线程 抢险完成，这个时候 version
update user set name="bb" ,version=version+1 where id=2 and version=1

```
1、数据库增加version字段

```sql
--增加乐观锁
alter table user add version int(11) default 1 comment '乐观锁';
```

2、实体类中增加对应的字段

```java
@Version
private Integer version
```

3、增加配置类

```java
@EnableTransactionManagement
@MapperScan("com.xx.mapper") //扫描接口类
@Configuration
public class MyBatisPlusConfig{
    
    //老版本的配置方式
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor(){
        return new OptimisticLockerInterceptor();
    } 
    
    //以下是最新版本的配置
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }
}
```

4、测试乐观锁

```java
//测试成功的案例
public void testOptimisticLocker(){
    //查询用户信息
	User user = userMapper.selectById(1L);
    //修改用户信息
    user.setName("new name");
    user.setEmail("123.qq.com");
    userMapper.updateById(user);
}

//测试失败的案例
public void testOptimisticLocker(){
    //查询用户信息
	User user = userMapper.selectById(1L);
    user.setName("new name");
    user.setEmail("123.qq.com");
    
    //模拟另外衣蛾线程执行了插队操作
    User user2 = userMapper.selectById(1L);
    user2.setName("new name222");
    user2.setEmail("123.qq.com");
    userMapper.updateById(user2)
    
    //自旋锁操作
    userMapper.updateById(user);
}
```

### P9、查询操作

```java
//测试：按ID查询
@Test
public void testSelectById(){
	User user = userMapper.selectById(1);
    System.out.printlt(user);
}

//批量查询
@Test
public void testSelectById(){
    List<User> users = userMapper.selectByBatchIds(Arrays.asList(1L,2L,3L);
    users.forEach(System.out::println);
}
                                                   
//条件查询 Map查询
public void testSelectByMap(){
    Map<String ,Object> map = new HashMap<>();
    map.put("name","AAA");
    map.put("age",10);
    List<User> users= userMapper.selectByMao(map);
    users.forEach(System.out::println);
}
```

### P10、分页查询

分页在网站使用的很多，

1、原始的使用limit进行分页

2、使用第三方插件PageHelper分页

3、Mybatis-plus 分页查询

使用方法：

1、配置分页插件

```java
//分页插件
@Bean
public PaginationInterceptor paginationInterceptor(){
    return new PaginationInterceptor();
}
```

2、使用Page对象进行分页

```java
@Test
public void testPage(){
    //第一个参数是当前页码， 第二个参数是每页数量/大小
    Page<User> page = new page<>(1,5);
    userMapper.selectPage(page,null); //第二个参数null是条件构造器
    page,getRecords().forEach(System.out::println);
    System.out.println(page.getTotal);
}
```

### P11、删除操作

```java
@Test
public void testDeleteById(){
    userMapper.deleteById
    userMapper.deleteBatchIds(集合);
    userMapper.deleteByMap(map);
    userMapper.delete(wapper); //条件构造器删除
}
```

### P12、逻辑删除



### P13、性能分析插件



### P14、条件构造器



### P15、代码生成器

