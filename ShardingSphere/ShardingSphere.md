https://www.bilibili.com/video/BV1LK411s7RX?seid=7779810564468416958

课程内容：

1、基本概念

​	什么是Sharding-Sphere

​	分库分表

2、Sharding-JDBC 分库分表操作

3、Sharding-Proxy分库分表操作



#### 什么ShardingSphere

官网：http://shardingsphere.apache.org

中文手册：http://shardingsphere.apache.org/index_zh.html

1、是一套开源的分布式的关系型数据库中间件解决方案

2、由三款产品：Sharding-JDBC、Sharding-Proxy

3、定位为关系型数据库中间件，合理地在分布式分布式环境下使用关系型数据库操作

2020年4月成为Apache的顶级项目

#### 什么 是分库分表

1、数据库中数据量时不可控的。随着时间和业务发展，造成表里的数据越来越多，如果再去对数据库crud操作时，会造成性能问题

2、解决方案1：升级硬件 方案2：分库分表

3、分库分表是为了解决为了由于数据量过大而造成的数据库性能降低的问题。

#### 分库分表的方式

1、分库分表有两种方式：垂直拆分、水平拆分

2、垂直拆分：垂直分表、垂直分库

3、水平拆分：水平分表、水平分库

4、垂直分表

（1）拆分数据库中的某张表，把这张表的一部分字段存放到另一张表，再把这张表中的其他字段存放到另一张表，例如商品表 拆分为商品基本信息和 商品描述信息

5、垂直分库

（1）把单一的数据库按照业务进行划分，专库专表。





##### 使用Sharding-JDBC实现读写分离

1、读写分离的概念

为了保证数据库产品的稳定性，很多数据库拥有双机热备功能，也就是，第一台数据库服务，是对外提供增删改业务的生产服务器，第二台数据库服务器主要进行读（查询）操作。

原理：让主数据库（master）处理事务性增删改操作，而从数据库（Slaver）处理查询Select操作

一主一从、一主多从、多主多从

###### 读写分离的原理

当主服务器有写入的时候，binlog日志 记录增删改查操作

从服务实时监控主服务器的binglog从

Sharding-JDBC通过SQL语义分析，实现读写分离；并不会实现数据同步，数据同步由MySQL数据库实现

2、Mysql配置读写分离



3、Sharding-JDBC操作



##### MySQL配置读写分离过程

（1）创建两个MySQL数据库服务，并且启动两个MySQL服务

​		复制MySQL5.5 目录 改名为MySQL-Slaver-01

（2）复制目录	

修改配置文件my.ini文件

​		修改端口号3306 改为3307

​		数据基本路径basedir=

​		复制数据目录并改名

​		数据目录改为datadir=

（3）修改后的数据库在windows安装为服务

​	第一步

​	使用命令	mysqld install mysql-slaver --default-file="D:\mysql-Slaver\my.ini"

​	进入windows服务窗口刷新

​	命令：sc delete 服务  删掉服务，重新配置

###### 	第二步  配置MySQL主从服务器

​	（1）在主服务器配置文件中配置

```properties
[mysqld]
#开启日志
log-bin=mysql-bin	#binlgo文件名
binlog_format=ROW	#选择ROW模式
server_id=1			#mysql实例id，不能喝slaverId重复
#设置需要同步的数据库
binlog-do-db=user_db
#屏蔽系统库同步
binlog-ingore-db=mysql
binlog-ingore-db=information_schema
binlog-ingore-db=performance_schema
```



​	（2）在从服务器配置文件中配置

```properties
[mysqld]
#开启日志
log-bin=mysql-bin	#binlgo文件名
binlog_format=ROW	#选择ROW模式
server_id=2			#mysql实例id，不能喝slaverId重复
#设置需要同步的数据库
binlog-do-db=user_db
#屏蔽系统库同步
binlog-ingore-db=mysql
binlog-ingore-db=information_schema
binlog-ingore-db=performance_schema
```

​	（3）重启主、从服务器

###### 	第三步 创建用于主从复制的账号

```properties
#切换至主库bin目录，登录主库
mysql -h localhost -uroot -p
#授权主从复制专用账号
GRANT REPLICATION SLAVE ON *.* TO 'db_sync'@'%' IDENTIFIED BY 'db-sync';
#刷新权限
FLUSH PRIVILEGES；
#确认位点  记录下文件名已经位点
show master status;
```

##### 	第四步 



#### Sharding-JDBC 简介

1、开源的轻量级java框架，是增强版JDBC驱动

2、Sharding-JDBC

(1) 并不是做分库分表

(2) 主要有两个功能，数据分片和读写分离

(3) 主要目的是：简化对分库分表之后的数据库相关操作

#### Sharding-JDBC 实现水平分表

##### 1、搭建环境

(1) SpringBoot+MyBatisPlus+Sharding-JDBC+Druid

(2) 创建SpringBoot工程，名称为sharding-JDBC

(3) 修改工程springboot版本为2.2.1

(4) 引入相关依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.20</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <!--<version>6.0.6</version>-->
        </dependency>

        <dependency>
            <groupId>org.apache.shardingsphere</groupId>
            <artifactId>sharding-jdbc-spring-boot-starter</artifactId>
            <version>4.1.0</version>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.0.5</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <!--<version></version>-->
        </dependency>
```



##### 2、按照水平分表的方式，创建数据库和数据库表

(1) 创建数据库course_db

(2) 在数据库创建两张表course_1 和 course_2

(3) 约定规则：如果添加的课程id是偶数，把数据添加到course_1，如果是奇数把数据添加到course_2

```sql
create table course_1(
	cid bigint(20) primary key,
    cname varchar(50) not null,
    user_id bigint(20) not null,
    cstatus varchar(10) not null
);

create table course_2(
	cid bigint(20) primary key,
    cname varchar(50) not null,
    user_id bigint(20) not null,
    cstatus varchar(10) not null
)
```

##### 3、编写代码实现对分库分表

(1) 创建实体类，mapper

```yaml
spring.
```

##### 4、配置Sharding-JDBC分片策略

(1) 在项目application.yml中进行配置

##### 5、编写测试代码

