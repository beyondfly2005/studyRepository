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
USE
ALTER KEYSPACE
DROP KEYSPACE
CREATE TABLE
DROP TABLE5.7 批量
TRUNCATE
CREATE INDEX

#### 4.4.2 数据操作命令
INSET
UPDATE
DELETE
BATCH

#### 4.4.3 查询指令
SELECT
WHERE
ORDERBY


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

