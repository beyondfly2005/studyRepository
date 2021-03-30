> 课程地址   https://www.bilibili.com/video/BV17E411N7KN
>
> 课程作者  遇见狂神说

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

```


-- 真实开发中
version 乐观锁
delete 逻辑删除
gmt_create 创建时间
gmt_moidify 修改时间
```

3、编写项目 使用SpringBoot初始化项目

4、导入依赖



尽量不要同时导入mybatis和mybatisplus

5、连接数据库





```
#mysql 5.x
spring.datasource.usrname=root
spring.dtasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf8

# mysql 8驱动不同 需要增加时区配置
spring.datasource.usrname=root
spring.dtasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8

```

6、pojo-dao-service-controller

dao (mybatis 配置maper.xml)

使用 mybatis-plus

- 创建实体类pojo

```
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



```
@Repository //代表是持久层
public interface UserMapper extends BaseMapper<User>{
	//所有的CRUD操作都已经完成了
}
```



```
@MapperScan("com.beyondsoft.mapper")
@SpringBootApplication
public class MybatisPlusDemoApplication{
	public static void main(String args[]){
		SpringApplication.run();
	}
}
```

测试代码

```
public 
```



##### 注意点

- 扫描
- A



### P3 配置日志

```
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