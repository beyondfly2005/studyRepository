# Cassandra

> 黑马教程
> https://www.bilibili.com/video/BV1aQ4y1Z7Nj
> 
> 
## P2 NoSQL数据库分类

### 1.3.1 Key-Value 数据库

如Redise

### 列存储数据库
如Cassandra HBase

### 文档型数据库 
通常是用来以应对分布式存储的海量数据，键仍然存储，但是他妈的特点是执行了多个列，这些列是由列家族来安排的，
如MongoDB、CouchDB 

### 图形Graph数据库
图形结果欧的数据库同 其他行了及其关系型数据库不同， 它是使用灵活的图形模型，并且能够扩展到多个服务器上

## P2 Cassandra介绍和下载
2.1 Cassandra概述

2.1.1 百度介绍
开一分布式NoSQL数据库系统 由FaceBook开发

2.1.1 官网
http://cassandra.apache.org

特点
- 弹性可扩展
- 始终基于架构
- 快速性性能
- 
- 事务支持
- 快速写入

使用场景
- 数据写入操作密集
- 修改操作很少
- 通过主键查询
- 需要对数据进行分区存储  支持分布式 

场景举例
- 存储日志
- 类似物联网的海量数据
- 对数据信息跟踪

### 下载 安装 访问

课程使用 3.9版本
download  最新 3.11


## P4 Window下的安装和环境变量设置

CASSANDRA_HOME 指向解压目录
添加到PATH变量  $CASSANDRA_HOME/bin

测试 cmd窗口
echo %CASSANDRA_HOME%

- data 数据
- system 系统数据 存储相关元数据信息
- commitlog 还没有真正持久化的数据，保证宕机不丢失数据
- cache目录 缓存数据 在casandra.yaml中定义
- 持久化缓存数据的时间间隔
- 磁盘足够的话 将以上三个目录 存放到不同磁盘中 以提升读写性能


创建data目录
修改 config\cassandar.yaml中修改配置
```yaml
data_file_direotories:
  - D:\xxxx\data

```

创建commitlog目录
```yaml
commit_directory: D:\xxx\commitlog

```

```yaml
save_cache: D:\xxx\cache
```

## P6 Win下的启动
cmd窗口
```bash
cd D:\xxx\cassandra
cassandra.bat
```
> 注意  窗口不要关闭

Starting listening for CQL client on local 127.0.0.1

#### 3.2.5 连接 cassandra

```bash
cqsh.bat 
cqlsh.bat 127.0.0.1 9042
cqlsh.bat 本机IP 9042
```

自动连接配置，配置文件搜索：rpc   开启本机ip
```yaml
start_rpc: true
```

```bash
cqlsh.bat 本机IP 9042
```

## P7 CentOS下的安装

上传压缩包
/usr/local/

解压缩

创建文件夹

```bash
mkdir data
mkdir commitlog
mkdir save_caches
```

修改配置文件
vim cassandra.yaml

启动cassandra
```properties
cd bin
./cassandra
```

不推荐使用root用户使用cassandra 启动报错

解决方法
```bash
./cassandra -r

```

启动成功
Strting Message Service on / 192.168.1.1 8704


测试启动成功
```bash
ps -ef |grep cassandra
```


使用 cqlsh

```bash
./cqlsh 192.168.1.1 9042
```


cd ../config
vim 

rpc: true

adddress: 192.168.1.1


## P8 Cassandra端口和配置文件介绍

#### Cassandra的端口

端口
- 7199 JMX
- 7000 节点通信
- 77001 TLS节点通信（使用TLS节点时）
- 9168 Thrift客户端API
- 9042 CQL本地传输端口

#### 3.5 Cassandra.yaml内容
cluster_name: 'Test Cluster'
listen_address 家庭ip  默认是localhost
stop
ignore
rpc_address
sedds:
内存大小

### 四、Cassandra的基本概念

4.1 数据模型
4.1.1 列
名称 值  时间戳
不需要雨雪订阅列
需要再KeySpace里定义列族，

##### 4.1.2 列族
包含多行Row的容器 相当于关系数据库的表Table
列族理解为Map<>
RowKey 理解为

列族类型
- 静态
- 动态column family(dynamic column family)

列族属性
- keys_cached 
- rows_cached
- preload_row_cache

#### 4.1.3 键空间 Keyspace
相当于数据库，我们创建一个键空间就是创建了一个数据库

副本因子

决定了数据有几份副本
配置为3以上 为避免单点故障

节点没有主从之分，每个节点
副本因子 不要超过 集群节点梳理


###### 副本存放策略
SimpleStrategy 简单策略
NetworkTopologyStrategy

4.1.4 副本
> 副本就是把数据存储到多个节点，来提示高容错性
> 

4.1.5 节点 Node


##### 4.1.6 数据中心

##### 4.1.7 集群

##### 4.1.8 超级列


#### 4.2 数据类型

> CQL提供一种
> 
##### 4.2.1 数智类型
|数据类型|含有| 描述 |
| ---- | ---- |----|
||||


#### 4.2.4 标识符类型


#### 4.2.5 集合类型

|数据类型|含有| 描述 |
| --- | --- |----|
|set|||
|list|||
|map|||


#### 4.2.6 其他基本类型

|数据类型| 含义 |描述|
| --- |---| --- |
|boolean | 布尔类型 ||
|blob|||
|inet| IPv或IPv6 ||
|counter| 计数器类型 ||

自定义数据类型

#### 4.3 CQL SHell客户端

##### 4.3.1 启动CQL
##### 4.3.2 

4.3.2 CQL基本命令

|     |     |     |
|-----|-----|-----|

##### Help 帮助
```shell
help

DESCRIBE cluster;


DESCRIBE KEYSPACE;

DESCRIBE tables; 


clear

SHOW 

SHOW HOST

SHOW SESSION

SHOW VERSION


```


### 4.4 CQL-Cassandra查询预研
CQL：Cassandra Query Language和关系数据库SQL很类似，一些关键词相似，可以使用CQL进行交互，实现订阅数据结构，插入数据，执行查询
> CQL和SQL是相互独立，没有任何关系的，CQL切实SQL的一些关键孤男寡女 如JOIN等，

| 指令              | 描述                    |
|-----------------|-----------------------|
| CREATE KEYSPACE | 在Cassandra中创建KeySpace |
| USE             | 连接到已创建的KeySpace       |
| ALTER KEYSPACE  | 更改KeySpace的属性         |
| DROP TABLLE     | 删除表                   |
| TRUNCATE        ||
| CREATE INDEX   | 在表的单个列上定义新索引     |
| DROP INDEX      |                       |

#### 4.4.1 数据定义名
|     |      |
|-----|------|
USE
ALTER KEYSPACE
DROP KEYSPACE
CREATE TABLE
DROP TABLE5.7 批量
TRUNCATE
CREATE INDEX

#### 4.4.2 数据操作命令
| |      |
| --- |------|
INSET
UPDATE
DELETE
BATCH

#### 4.4.3 查询指令
| |      |
| --- |------|
|SELECT| |
| WHERE | |
|ORDERBY| |


## 五 基本操作
### 5.1 操作键空间

```sql
create keyspace school with replication = {'class':'SimpleStrategy', 'replication_factor': 3};
```
验证
```bash
DESCRIBE KEYSPACES;
```
### 5.2 操作表 索引

### 5.3 查询数据

### 5.4 添加数据

### 5.5 更新列数据


### 5.6 删除行

### 5.7 批量操作


## P24

## 六、Java操作Cassandra

### 6.1 Java客户端介绍

- Netflix astyanax 
- datastx 的java-driver
- hector-client
- spring-data-cassandra

6.2 datastax的java-driver

```pom
<dependency>
com.datasta
</dependenvy>
```

```java
public class TestKeySpace{
    
    private Session session;
    
    @Before
    public void init(){
        //服务器地址
        String host ="192.168.137..131";
        int port =9042;
        //连接服务端 获取会话
        Cluster cluster = Cluster.builder()
          .addContactPoint(host)
          .withPort(port)
          .build();
        session = cluster.connect();
    }
    
    
    @Test
    public void findKeySpace(){
        List<KeySpace> kespaces = session.getCluster().getMetadata().getKeySpaces();
        for(KeySpace keySpace: keyspaces){
          System.out.println(keySpace.getName());
        }
    }
    
    
    public void createKeySpace(){
        //使用cql来创建
        session.execute("create keyspace school with replication = {'class':'SimpleStrategy', 'replication_factor': 3};");
        //面向对象的方式
      Map<String,Object> replication = new HashMap<>;
        KeySpaceOption  options=SchemaBuilder.createKeySpace("school")
          .ifNotExists()
          .with()
          .replication(replication);
        session.execute(options);
    }
    
    @Test
    public void deleteKeySpace(){
        //使用cql
        //面向兑现方式
      DropKeuySpace dropKeySpace = SchemaBuilder.dropKeyspace("school").ifExists();
      session.execute(dropKeSpace);
    }

    @Test
    public void alterKeySpace(){
        Map<String,Object> options = new HashMap<>;
    SchemaBuilder.alerKeyspace("school").alerKeyspace("school")
      .with()
      .replication(options);
    }
}

```

TestTable.java

```java

public class TestTable{
    
    private Session session;
    @Before
    public void init(){
        String address="192.168.1.1";
        int port;
        Cluster cluster = Cluster.builder()
        .addDContactPoint(address).withPort(port)
        .build();
        session = cluster.connect();
    }
}

```

### 6.3 SpringData Cassandra



### Spring Data Cassandra

环境要求
- Cassandra 2.0以上
- JDK 1.8及以上
- Spring 5.2.7.RELEASE 及以上


#### 6.3.2 创建Maven工程

##### 引入依赖
```pom

<dependency>
   <> spring-data-cassandra
</dependency>

```

配置文件
```properties
cassandra.contactpoints=192.168.1.1
cassandra.port=9042
cassandra.keyspace=school
```

springContext.xml

```xml
<beans>
<!-- 引入properties配置文件 -->
<context:property-placeholder location="class:cassandra.properties">
<!--配置IP -->
  <context:cluster contact-points="${cassandra.keyspace}" port="${cassandra.port}" />
  <!--配置映射 -->
<cassandra:mapping />
  <!--配置转换器 -->
<cassandra:converter />
  <!--模板-->
  <cassandra:repositories base-package="com.beyond.springcass.repository" />
    <!--扫描-->
    <context:component-scan base-package="com.beyond"/>
</beans>
```

pojo

```java

@Data
@AllargConstructor
@NorgConstructor
@Table
public class Student{
   @PrimKey
  private Long id;
}

```

Repository

```Java

//提供简单的CRUD方法

@Repository
public interface StudentRepository extends CassandraRepository<Studennt,Long>{
    
}
```
测试

```Java
public class TestCass {
  
    
    private StudentServie studentService; 
    
    @Before
  public void init() {
      //读取配置文件  初始化spring
      //ClasssPathXmlApplicationContext classsPathXmlApplicationContext = new ClasssPathXmlApplicationContext("springContext.xml");
      ConfigurableApplicationContext ctx = new ClasssPathXmlApplicationContext("springContext.xml");
    StuddentService studdentService = (StudentService)ctx.getBean("studentService");
      
  }
  
  //查询方法
  @Test
    public void findAll(){
        List<Student> studentList = studentService.queryAll();
        for (Student student: studentList) {
          System.out.println(student.getName());
          System.out.println("-------------------------");
        }
  }
}


```
service
```java

@Service
public class StudentService{
    
}
```


#### 6.3.2 SpringBoot集成Cassandra
pom
```pom
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-cassandra</artifactId>
</dependency>
```

yml配置

```yaml

spring:
  data:
    cassandra:
      keyspace-name: spacenametest
      #entity-base-packages:
      contact-points: 集群地址用逗号分隔
      port: 9042
      cluster-name: Test Cluster
      username: cass用户名
      password: cass密码
      consistency-level: ONE
      serial-consistency-level: ONE
```

配置类CassandraConfig.java

```java

package com.config;
 
 
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.cassandra.config.AbstractCassandraConfiguration;
import org.springframework.data.cassandra.config.CqlSessionFactoryBean;
 
@Configuration
public class CassandraConfig extends AbstractCassandraConfiguration {
 
    //空间名称
    @Value("${spring.data.cassandra.keyspace-name}")
    private String keyspaceName;
 
    //节点IP（连接的集群节点IP）
    @Value("${spring.data.cassandra.contact-points}")
    private String contactPoints;
 
    @Value("${spring.data.cassandra.username}")
    private String username;
 
    @Value("${spring.data.cassandra.password}")
    private String password;
 
//    @Value("${spring.data.cassandra.session-name}")
//    private String sessionName;
 
    public String getKeyspaceName() {
        return keyspaceName;
    }
 
    public String getContactPoints() {
        return contactPoints;
    }
 
//    @Override
//    public String getSessionName() {
//        return sessionName;
//    }
 
//    @Override
//    public String getLocalDataCenter() {
//        return "datacenter1";
//    }
 
    @Override
    public CqlSessionFactoryBean cassandraSession() {
        CqlSessionFactoryBean cqlSessionFactoryBean = super.cassandraSession();
        cqlSessionFactoryBean.setPassword(password);
        cqlSessionFactoryBean.setUsername(username);
        return cqlSessionFactoryBean;
    }
 
}
```

Student映射Pojo类
```java
package com.cas4.entity;
 
import lombok.Getter;
import lombok.Setter;
import org.springframework.data.cassandra.core.cql.PrimaryKeyType;
import org.springframework.data.cassandra.core.mapping.Column;
import org.springframework.data.cassandra.core.mapping.PrimaryKeyColumn;
import org.springframework.data.cassandra.core.mapping.Table;
 
@Setter
@Getter
@Table(value = "student")
public class Student {
 
//@PrimaryKeyColumn(value = "id", type = PrimaryKeyType.CLUSTERED)
    @PrimaryKeyColumn(value = "id", type = PrimaryKeyType.PARTITIONED)
    private int id;
    @Column("name")
    private String name;
    @Column("age")
    private int age;
 
}

```

Service类调用
```java
package com.cas4.service;
 
import com.cas4.entity.Student;
import org.springframework.data.cassandra.core.CassandraTemplate;
import org.springframework.stereotype.Service;
 
import javax.annotation.Resource;
import java.util.List;
 
@Service
public class ServiceImpl {
 
    @Resource
    private CassandraTemplate cassandraTemplate;
 
    //自定义sql语句
    public Student findByTenantIdAndSequenceId(String tenantId, String sequenceId) {
        String cql = String.format("select * from master_order where tenant_id = '%s' and sequence_id='%s'", tenantId, sequenceId);
        Student student = cassandraTemplate.selectOne(cql, Student.class);
        return student;
    }
 
    public void insert(Student masterOrderDO) {
        //插入
        cassandraTemplate.insert(masterOrderDO);
        //删除
//        cassandraTemplate.delete(masterOrderDO);
    }
 
    public void find(Student student) {
        //插入
        List<Student> list  = cassandraTemplate.select("select * from spacenametest.student where v='"+ student.getV()+"'", Student.class);
        System.out.println("总条数:"+list.size());
    }
 
}

```
cassandra 建表语句

```bash
bin]# ./cqlsh x.x.x.x 9042 -u cass用户名 -p cass密码

use spacenametest;
```

建表语句
```sql
CREATE TABLE student(
  id int,
  name text,
  age int,
  PRIMARY KEY (id,name)  --多个主键
)
```
或者
```sql
CREATE TABLE student(
  id int PRIMARY KEY,  --一个主键
  name text,
  age int,
  PRIMARY KEY 
)

```



## P37 集群搭建

三台CentOS7 系统 
设置静态IP 重启虚拟机 不会变化IP
seed 种子节点

种子节点的作用：
> 一个新阶段加入集群时，需通过种子节点来发现集群中其他节点，需要至少一个活跃的种子节点可以连接
> 一旦节点加入这个集群，知道了集群中的其他节点，这个几点在下次启动的时候就不需要种子节点了
> 对应种子节点没有特殊要求，可以设置任何一个节点为种子。
> 
> 
```shell
cd conf

vim cassandra.yaml
```

```yaml
## 修改集群名称
cluster_name: 'Test Cluster'

seed_provitor:
  paramters:
    - seeds: 192.168.137.31


rpc_address: 192.168.

```

## 八 Cassandra的数据存储

> 分为三种
> CommitLog：
> Memtable：
> SSTable：

### 8.1 CommitLog数据格式
数据将会被持久化到磁盘中，

### 8.2 Memtable内存中数据结构

### 8.3 SSTable
