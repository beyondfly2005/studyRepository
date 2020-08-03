> 参考视频
>
> https://www.bilibili.com/video/BV1Ab41127Yq?from=search&seid=12198849306641399699



### 一  MySQL数据库 - SQL优化

#### 1 结构图

MySQL DBMS-MySQL Database Management System 数据库管理系统

![](https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1759924753,1123411956&fm=15&gp=0.jpg)



#### 2 MySQL数据库引擎简介

##### 2.11 ISAM（Indexed Sequential Access Method）

​	ISAM是一个定义明确且经理世界考研的数据表格管理方法，

ISAM的两个主要不足之处在于：不支持事务处理，也不能够容错，如果你的硬盘崩溃了，那么数据文件就无法恢复了，如果你正在

##### 2.2 MyISAM

​	MyISAM是Mysql的ISAM扩展格式（MySQL5.5之前的版本缺省数据库引擎）数据库引擎，

处理提供ISAM例所没有的索引和自动管理的大力功能，MyISAM还使用一种表格锁定的机制，来优化多个并发的读写操作，其代价是你需要经常运行OPIMIZE TABLE命令

MyISAM格式的一个重要缺陷是补鞥呢在表损坏后恢复数据。

**不支持事务**； 数据越多写操作效率越低，因为要维护数据和索引信息 索引越多

如果使用该数据库引擎，会生成三个文件

.frm 表结构信息

.myd 数据文件

.myi 表的索引信息  索引是key-value

##### 2.3 InnoDB

​	InnoDB数据库引擎都是造成MySQL灵活性的技术的直接产品，

ISAM和MyISAM引擎不支持事务，也不支持外键。尽管要比ISAM和MyISAM引擎慢很多，但是InnoDB包含了对事务处理和外键的支持，这两点都是前两个索引所没有的，是现在的Mysql(5.5以上的版本)常用版本的默认引擎。

​	MySQL官方对InnoDB是这样解释的：InnoDB给MySQL提供了具有提交



​	InnoDB是为处理巨大数据量时的最大性能设计，它的CPU效率可能是任何其他基于磁盘的关系型数据库引擎所不能匹敌的。

​	InnoDB存储引擎被完全与MySQL服务器整合，InnoDB存储引擎为在主内存中缓存数据和索引而危害它自己的缓冲池。InnoDB存储它的表和索引在一个表空间中，

###### 2.3.1InnoDB 

###### 2.3.2 如何选择

- 是否要支持事务
- 

##### 2.4 Memory存储引擎

​	Memory存储引擎，通过名字就很容易让人知道，他是一个将数据存储在内存中的存储

##### 2.5  NDBCluster存储环境

​	NDB存储引擎也叫NDBCLuster存储引擎，主要是从MySQLClu

2.6

2.7

2.8 ARCHIVE存储引擎

2.9 BLACKHOME存储引擎

2.10 CSV存储引擎

#### 3 存储引擎管理

##### 3.1 查看数据库支持的存储引擎

```sql
show engines
```

##### 3.2 查看数据库当前使用的存储引擎
```sql
show variables like '%storage_engine%'
```
##### 3.3 查看数据库表所用的存储引擎
查看默认的存储引擎
my.ini
default-storage-engine=INNODB
```sql
show create table table_name 
```
#### 3.4 创建表指定存储引擎
```sql
create table table_name(coumne_name column_type) engine =engine_name
```
#### 3.5 修改表的存储引擎
```sql
alter table table_name engine =engine_name
```

#### 3.6修改默认的存储引擎
```sql
安装目录/my.ini(5.7版本在数据目录中 programData/mysql Server 5.7/)
default-storage-engine=INNODB
linux系统 /etc/my.cnf
修改完后 重启数据库
```

### 4 MySQL中的索引

#### 4.1 索引的优点

为什么要创建索引？这是因为 藏家索引可以达到提高系统的查询性能

第一 通过创建唯一索引，可以保证数据库表中的没以后数据的唯一性  如id 和用户表的登录名

第二 可以当加快数据的检索速度，这也是创建索引的最主要的原因

第三 可以加速表和表之间的连接 特别是咋实现数据的参考完整性方面特别有意义

第四 在使用分组和排序字句

#### 4.2 索引的缺点

每一列都加索引，是非常不明智的，这是因为，增加索引也有许多不利的方面

第一 创建索引和维护索引要耗费时间 这种时间随着数据量的增加而增加

第二 索引需要占用物理空间，除了数据表占用空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引 

#### 4.3 什么样的自动审核创建索引

第一 经常需要进行搜索的列上

第二 在作为主键的列上 强制该列的唯一性和组织表中数据的排列结构

第三 在经常用在连接的列上，这些列主要是一些外键，可以加快连接的速度

第三 在经常需要根据范围进行搜索的列上创建索引，因为索引已经排序，其指定的范围是连续的

第四 在经常需要排序的列上藏家索引，因为索引以及排序，这样查询可以利用

#### 4.4 什么样的索引不适合创建索引

第一 对于在查询中很少使用或者参考的列不应该创建索引。这是因为 既然这些列很少使用

第二 对于很少数据值的列也不应该增加索引，比如性别 

第三 对于那些定义为text image blob clob bit 大文本字段 不应该增加索引

第三 当修改性能远远大于检索性能是，不应该创建索引，一直增删改 不做查询

### 5  MySQL中的索引类型

#### 5.1 B-Tree索引 

B-Tree索引 平衡树索引 顾名思义，就是所有的索引节点都按照 blance  tree的数据结构来存储。B-tree结构可以显著减少定位记录时所经历的中间过程 从而加快存取速度。

B-tree中每个节点包含

1 本节点所含关键字的个数

2 指向父节点的指针

3 关键字

4 指向子节点的指针

#### 5.2 Full-text

### 6 MySQL中的索引管理

索引的查看、删除都是一样的

#### 6.1 普通索引

6.1.1 创建索引
```sql
create index index_name 
```

6.2.2 查看索引
```sql

```
6.2.3 删除索引
```sql

```
#### 6.2 唯一索引
与普通索引类似，不同的就是：索引
6.2.1创建索引
```sql
create unique index index_name on table_name (column(lenght))
```
#### 6.3 全文索引
仅适用于MyISAM表
CHAR VARCHAR 或TEXT
对于较大的，对于大容量的数据表，生成全文索引是一个非常消耗时间非常消耗硬盘空间的做法
6.3.1创建索引
```sql
create fulltext index 
```
#### 6.4 组合索引

6.4.1创建索引
```sql

```
### 二 MySQL+ MyCat分库分表

### 1  MyCat简介  

### 2  



