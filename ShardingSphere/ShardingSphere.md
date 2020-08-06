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



#### 分库分表应用和问题



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

##### 3、编写代码实现对分库分表后的数据操作

(1) 创建实体类，mapper

```java
@Data
public class Course {
    private Long cid;
    private String cname;
    private Long userId;
    private String cstatus;
}
```

```java
@Repository
public interface CourseMapper extends BaseMapper<Course> {

}
```



##### 4、配置Sharding-JDBC分片策略

(1) 在项目application.yml中进行配置

```yaml
#需要加入下面的配值允许重载bean名称，主要用于后面sql对表的操作，MybatisPlus是根据类名作为表名的
spring.main.allow-bean-definition-overriding=true

spring.shardingsphere.datasource.names=m1

#配置数据源内容
spring.shardingsphere.datasource.m1.type=com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m1.driver-class-name=com.mysql.cj.jdbc.Driver
spring.shardingsphere.datasource.m1.url=jdbc:mysql://localhost:3306/course_db?serverTimezone=GMT%2B8
spring.shardingsphere.datasource.m1.username=root
spring.shardingsphere.datasource.m1.password=123456

#spring.shardingsphere.sharding.tables.course.actual-data-nodes=m1.course_$->{0..1}
# 表的名称m1.course_1,m1.course_2
spring.shardingsphere.sharding.tables.course.actual-data-nodes=m1.course_$->{1..2}

#指定course表里的主键生成策略  SNOWFLAKE是雪花算法
spring.shardingsphere.sharding.tables.course.key-generator.column=cid
spring.shardingsphere.sharding.tables.course.key-generator.type=SNOWFLAKE

# 指定分片策略 cid偶数添加到course_1表
spring.shardingsphere.sharding.tables.course.table-strategy.inline.sharding-column=cid
spring.shardingsphere.sharding.tables.course.table-strategy.inline.algorithm-expression=course_$->{cid % 2 +1}

#打开sql输出日志
spring.shardingsphere.props.sql.show=true

#配置mybatis-plus驼峰
mybatis-plus.configuration.map-underscore-to-camel-case=true
```



##### 5、编写测试代码

```java

@RunWith(SpringRunner.class)
@SpringBootTest
public class StudyShardingApplicationTests {

    @Autowired
    private CourseMapper courseMapper;

    //添加课程的方法
    @Test
    public void addCourse() {
        Course course = new Course();
        course.setCname("Java");
        course.setUserId(100L);
        course.setCstatus("Normal");
        courseMapper.insert(course);
    }

    //批量添加课程
    @Test
    public void batchAddCourse() {
        for (int i = 0; i <10 ; i++) {
            Course course = new Course();
            course.setCname("Java"+i);
            course.setUserId(100L);
            course.setCstatus("Normal"+i);
            courseMapper.insert(course);
        }
    }

    //查询课程的方法
    @Test
    public void findCourse(){
        QueryWrapper<Course> wrapper = new QueryWrapper<>();
        wrapper.eq("cid",498260737236402177L);
        Course course = courseMapper.selectOne(wrapper);
        System.out.println(course.toString());
    }
}
```

(1) 测试代码执行报错

```pr
Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true
```

(2) 解决方案

```
spring.main.allow-bean-definition-overriding=true 
#当遇到同样名字的时候，是否允许覆盖注册
```



#### Sharding-JDBC 实现水平分库

##### 1、需求分析

![](https://img-blog.csdnimg.cn/20200723104453173.png)

##### 2、 创建数据库和表

​	创建数据库：edu_db_1 和 edu_db_2

​	两个哭上同时创建表course_1 和 course_2



##### 3、在SpringBoot配置文件配置数据库分片规则

```yaml
#需要加入下面的配值允许重载bean名称，主要用于后面sql对表的操作，MybatisPlus是根据类名作为表名的
spring.main.allow-bean-definition-overriding=true

# 水平分库，配置两个数据源
spring.shardingsphere.datasource.names=m1,m2

#配置第1数据源内容
spring.shardingsphere.datasource.m1.type=com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m1.driver-class-name=com.mysql.cj.jdbc.Driver
spring.shardingsphere.datasource.m1.url=jdbc:mysql://localhost:3306/edu_db_1?serverTimezone=GMT%2B8
spring.shardingsphere.datasource.m1.username=root
spring.shardingsphere.datasource.m1.password=123456

#配置第2数据源内容
spring.shardingsphere.datasource.m2.type=com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m2.driver-class-name=com.mysql.cj.jdbc.Driver
spring.shardingsphere.datasource.m2.url=jdbc:mysql://localhost:3306/edu_db_2?serverTimezone=GMT%2B8
spring.shardingsphere.datasource.m2.username=root
spring.shardingsphere.datasource.m2.password=123456


#指定数据库表的粉笔情况 表的名称m1.course_1,m1.course_2
spring.shardingsphere.sharding.tables.course.actual-data-nodes=m${1..2}.course_$->{1..2}


#指定course表里的主键生成策略  SNOWFLAKE是雪花算法
spring.shardingsphere.sharding.tables.course.key-generator.column=cid
spring.shardingsphere.sharding.tables.course.key-generator.type=SNOWFLAKE

# 指定分片策略 cid偶数添加到course_1表
spring.shardingsphere.sharding.tables.course.table-strategy.inline.sharding-column=cid
spring.shardingsphere.sharding.tables.course.table-strategy.inline.algorithm-expression=course_$->{cid % 2 + 1}

#指定数据库分片侧列 越多user_id 是偶数添加到m1 奇数添加到m2  default-database 默认所有表规则
#spring.shardingsphere.sharding.default-database-strategy.inline.sharding-column=user_id
#spring.shardingsphere.sharding.default-database-strategy.inline.algorithm-expression=m$->{user_id % 2 + 1}

spring.shardingsphere.sharding.tables.course.database-strategy.inline.sharding-column=user_id
spring.shardingsphere.sharding.tables.course.database-strategy.inline.algorithm-expression=m$->{user_id % 2 + 1}


#打开sql输出日志
spring.shardingsphere.props.sql.show=true

#配置mybatis-plus驼峰
mybatis-plus.configuration.map-underscore-to-camel-case=true
```



##### 4、编写测试方法

```java
 //===============================测试水平分库===================================
    @Test
    public void addCourseDb() {
        Course course = new Course();
        course.setCname("Java");
        course.setUserId(100L);
        course.setCstatus("Normal");
        courseMapper.insert(course);
    }
    @Test
    public void addCourseDb1() {
        Course course = new Course();
        course.setCname("Java");
        course.setUserId(101L);
        course.setCstatus("Normal");
        courseMapper.insert(course);
    }

    //查询课程的方法
    @Test
    public void findCourseDb(){
        QueryWrapper<Course> wrapper = new QueryWrapper<>();
        wrapper.eq("user_id",100L);
        wrapper.eq("cid",498276104004435969L);
        Course course = courseMapper.selectOne(wrapper);
        System.out.println(course.toString());
    }
```

#### Sharding-JDBC 实现垂直分库

##### 1、需求分析

![](https://img2020.cnblogs.com/blog/1365514/202007/1365514-20200719233547730-1662781093.png)

##### 2、创建数据库和表

​	创建数据库user_db

```sql
create table t_user (
	user_id bigint(20) primary key,
	user_name varchar(100) not null,
	ustatus varchar(50) not null
)
```

##### 3、编写操作代码

(1) 创建User实体类和UserMapper接口类

(2) 配置垂直分库策略

```yaml
spring.main.allow-bean-definition-overriding=true
spring.shardingsphere.datasource.names=m1,m2,m0

#配置USER_DB数据源内容
spring.shardingsphere.datasource.m0.type=com.alibaba.druid.pool.DruidDataSource
spring.shardingsphere.datasource.m0.driver-class-name=com.mysql.cj.jdbc.Driver
spring.shardingsphere.datasource.m0.url=jdbc:mysql://localhost:3306/user_db?serverTimezone=GMT%2B8
spring.shardingsphere.datasource.m0.username=root
spring.shardingsphere.datasource.m0.password=123456

#配置user_db 数据库里的t_uesr表改为专库专表
spring.shardingsphere.sharding.tables.t_user.actual-data-nodes=m$->{0}.t_user

#指定t_user表里的主键生成策略  SNOWFLAKE是雪花算法
spring.shardingsphere.sharding.tables.t_user.key-generator.column=user_id
spring.shardingsphere.sharding.tables.t_user.key-generator.type=SNOWFLAKE

# 指定分片策略 user_id偶数添加到t_user表
spring.shardingsphere.sharding.tables.t_user.table-strategy.inline.sharding-column=user_id
spring.shardingsphere.sharding.tables.t_user.table-strategy.inline.algorithm-expression=t_user

```

(3) 编写测试代码

```java
    //===============================测试垂直分库==================================
    @Test
    public void addUserDb(){
        User user = new User();
        user.setUserName("zhangsan");
        user.setUstatus("Normal");
        userMapper.insert(user);
    }

    @Test
    public void findUserDb(){
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.eq("user_id",498286821667504129L);
        User user = userMapper.selectOne(wrapper);
        System.out.println(user.toString());
    }
```



测试报错

```
SQL: INSERT INTO user  ( user_name, ustatus )  VALUES  ( ?, ? )
Cause: java.lang.IllegalStateException: Missing the data source name: 'null'
```

实体类名为User 数据库表名为t_use，无法自动对应

User实体类增加@TableName注解

```
@TableName(value = "t_user")
```



#### Sharding-JDBC 操作公共表

###### 1、公共表

  字典表

（1）存储固定数据的表，表数据很少发生变化，查询时经常进行关联的表

（2）在每个数据库中创建出相同结果的公共表，对一个实体类进行操作，会关联到所有的表中

###### 2、在多个数据库中创建相同结构的公共表

​	在user_db、edu_db1、edu_db_2三个数据库上创建公共表

```sql
create table t_udict(
	dictid bigint(20) primary key,
    ustatus varchar(100) not null,
    uvalue varchar(100) not null
)
```

##### 3、在项目配置文件application.properties中进行公共表的配置

```yaml
#配置公共表
spring.shardingsphere.sharding.broadcast-tables= t_udict
#指定 t_udict 表中主键的生成策略  SNOWFLAKE:雪花算法
spring.shardingsphere.sharding.tables.t_udict.key-generator.column = dictid
spring.shardingsphere.sharding.tables.t_udict.key-generator.type =SNOWFLAKE
```

##### 4、编写测试代码

​	三个数据库公共表的数据同时增加 或者同时删除

(1) 创建新的实体类和mapper

(2) 编写添加和删除方法

```java
    @Autowired
    private DictMapper dictMapper;

    //==============================测试公共表====================================
    @Test
    public void addDict(){
        Dict dict = new Dict();
        dict.setUstatus("a");
        dict.setUvalue("已启用");
        dictMapper.insert(dict);
    }

    @Test
    public void deleteDict(){
        QueryWrapper<Dict> wrapper = new QueryWrapper<>();
        wrapper.eq("dictid",498292244155990017L);
        dictMapper.delete(wrapper);
    }
```



