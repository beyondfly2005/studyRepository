# TDengine

## 简介

### 简介

- TDengine 是一款开源、高性能、云原生的时序数据库，且针对物联网、车联网、工业互联网、金融、IT 运维等场景进行了优化。TDengine 的代码，包括集群功能，都在 GNU AGPL v3.0 下开源。除核心的时序数据库功能外，TDengine 还提供缓存、数据订阅、流式计算等其它功能以降低系统复杂度及研发和运维成本。

### 功能

- 写数据

	- SQL写入
	- 无模式写入

		- InfluxDB Line协议
		- OpenTSDB Telnet协议
		- OpenTSDB JSON协议

	- 第三方工具集成写入

		- Telegraf
		- Prometheus
		- StatSD
		- collectd
		- Icinga2
		- EMQX
		- HiveMQ

- 查询数据

	- 标准SQL 支持嵌套查询
	- 时序数据库特色函数
	- 时序数据特色查询

		- 采样
		- 差值
		- 累加和
		- 时间加强平均
		- 状态窗口
		- 会话窗口

	- 用户自定义函数 UDF

- 缓存
- 流式计算

	- 持事件驱动的流式计算，这样在处理时序数据时就无需 Flink 或 Spark 这样流式计算组件

- 数据订阅

	- 订阅一张表或一组表的数据，提供与 Kafka 相同的 API，而且可以指定过滤条件

- 可视化集成

	- 支持与 Grafana 的无缝集成
	- 支持与 Google Data Studio 的无缝集成

- 集群

	- 支持集群部署，可以通过增加节点进行水平扩展以提升处理能力
	- 支持通过 Kubernetes 部署 TDengine
	- 支持通过多副本提供高可用能力

- 管理

	- 监控TDengin实例
	- 多数据导入方式
	- 多数据导出方式

- 工具

	- 命令行交互程序CLI
	- 压力测试工具taoBenchmark

- 编程

	- 提供各种语言的连接器Connector
	- 支持REST接口

### 特点

- 高性能
- 极简时序数据平台
- 云原生
- 简单易用
- 分享能力
- 核心开源

### 技术生态

- 技术生态

- Kafka
- OPC-UA
- Telgraf

### 适用场景

- 典型适用场景

	- LoT物联网
	- 工业互联网
	- 车联网
	- 能源
	- 金融证券
	- 。。。

- 数据源特点

	- 总体数据量巨大
	- 数据输入速度偶尔或者持续巨大
	- 数据源数目巨大

- 系统架构要求
- 系统功能需求
- 系统性能要求
- 系统维护要求
- 与其他数据对比测试

	- vs InfluxDB
	- vs OpenTSDB
	- vs Cassandra

## 基本概念

### 采集量Metric

- 采集量是指传感器、设备或其他类型采集点采集的物理量，随时间变化的
- 数据类型可以是整型、浮点型、布尔型，也可是字符串
- 如电流、电压、温度、压力、GPS 位置等

### 标签Label/Tag

- 标签是指传感器、设备或其他类型采集点的静态属性，不是随时间变化的
- 虽然是静态的，但 TDengine 容许用户修改、删除或增加标签值。
- 如设备型号、颜色、设备的所在地等，数据类型可以是任何类型。

### 数据采集点
Data Collection Point

- 按照预设时间周期或受事件触发采集物理量的硬件或软件。一个数据采集点可以采集一个或多个采集量，但这些采集量都是同一时刻采集的，具有相同的时间戳
- 复杂的设备，往往有多个数据采集点，每个数据采集点采集的周期都可能不一样，而且完全独立，不同步。
- 如对于一台汽车，有数据采集点专门采集 GPS 位置，有数据采集点专门采集发动机状态，有数据采集点专门采集车内的环境，这样一台汽车就有三个数据采集点。

### 表Table

- TDengine 采用传统的关系型数据库模型管理数据。用户需要先创建库，然后创建表，之后才能插入或查询数据
- TDengine 采取一个数据采集点一张表的策略，要求对每个数据采集点单独建表
- 采用一个数据采集点一张表的方式，能最大程度的保证单个数据采集点的插入和查询的性能是最优的

### 超级表 SuperTable

- 超级表是指某一特定类型的数据采集点的集合。同一类型的数据采集点，其表的结构是完全一样的，但每个表（数据采集点）的静态属性（标签）是不一样的。
- 描述一个超级表（某一特定类型的数据采集点的集合），除需要定义采集量的表结构之外，还需要定义其标签的 Schema，标签的数据类型可以是整数、浮点数、字符串、JSON，标签可以有多个，可以事后增加、删除或修改。
- 如果整个系统有 N 个不同类型的数据采集点，就需要建立 N 个超级表。
- 由于一个数据采集点一张表，导致表的数量巨增，难以管理，而且应用经常需要做采集点之间的聚合操作，聚合的操作也变得复杂起来。为解决这个问题，TDengine 引入超级表的概念
- 表用来代表一个具体的数据采集点，超级表用来代表一组相同类型的数据采集点集合

### 子表 Subtable

- 通过超级表创建的表称之为子表
- 使用超级表的定义做模板，同时指定该具体采集点（表）的具体标签值来创建该表

### 库（Database）

- 库是指一组表的集合。

## 安装部署

### docker方式

- 拉取镜像

	- docker pull tdengine/tdengine:latest

- 运行镜像

	- docker run -d -p 6030:6030 -p 6041:6041 -p 6043-6049:6043-6049 -p 6043-6049:6043-6049/udp tdengine/tdengine

- 客户端

	- $ taos

### 安装包方式

- 安装

	- RPM方式

		- 方法

		  下载：
		  TDengine-server-3.0.3.0-Linux-x64.rpm
		  taosTools-2.4.9-Linux-x64-comp3.rpm
		  
		  安装：
		  sudo rpm -ivh TDengine-server-<version>-Linux-x64.rpm

	- tar包方式

		- 方法

		  安装包：
		  TDengine-server-3.0.3.1-Linux-x64.tar.gz
		  TDengine-server-3.0.3.1-Linux-arm64.tar.gz
		  TDengine-server-3.0.3.1-Linux-x64-Lite.tar.gz
		  taosTools-2.4.10-Linux-x64-comp3.tar.gz
		  taosTools-2.4.10-Linux-arm64-comp3.tar.gz
		  TDengine-client-3.0.3.1-Linux-x64.tar.gz
		  TDengine-client-3.0.3.1-Linux-arm64.tar.gz
		  TDengine-client-3.0.3.1-Linux-x64-Lite.tar.gz
		  
		  解压：
		  tar -zxvf TDengine-server-<version>-Linux-x64.tar.gz
		  
		  安装命令：
		  sudo ./install.sh

	- apt-get
	- windows

		- 方法

		  下载：
		  
		  TDengine-server-3.0.3.1-Windows-x64.exe
		  TDengine-client-3.0.3.1-Windows-x64.exe
		  
		  运行：
		  
		  cmd 窗口执行 sc start taosd
		  或在 C:\TDengine 目录下，运行 taosd.exe 来启动 TDengine 服务进程

	- macOS

### 使用云服务

>https://cloud.taosdata.com/

## 集群部署

### 手动部署

#### 准备工作
##### 第零步
确保集群所有物理节点，没有安装过其他版本的TDengin,或已将其三次，并情况所有数据
##### 第一步
规划集群所有物理节点的 FQDN，将规划好的 FQDN 分别添加到每个物理节点的 /etc/hosts；修改每个物理节点的 /etc/hosts，将所有集群物理节点的 IP 与 FQDN 的对应添加好。
##### 第二步
确保集群中所有主机在端口 6030-6042 上的 TCP/UDP 协议能够互通。

##### 第三步
在所有物理节点安装 TDengine，且版本必须是一致的，但不要启动 taosd。安装时，提示输入是否要加入一个已经存在的 TDengine 集群时，第一个物理节点直接回车创建新集群，后续物理节点则输入该集群任何一个在线的物理节点的 FQDN:端口号（默认 6030）

##### 第四步
查所有数据节点，以及应用程序所在物理节点的网络设置：

每个物理节点上执行命令 hostname -f，查看和确认所有节点的 hostname 是不相同的

每个物理节点上执行 ping host，其中 host 是其他物理节点的 hostname，看能否 ping 通其它物理节点；如果不能 ping 通，需要检查网络设置，或 /etc/hosts 文件

##### 第五步

修改 TDengine 的配置文件（所有节点的文件 /etc/taos/taos.cfg 都需要修改）。假设准备启动的第一个数据节点 End Point 为 h1.taosdata.com:6030，其与集群配置相关参数如下：

```conf
// firstEp 是每个数据节点首次启动后连接的第一个数据节点
firstEp               h1.taosdata.com:6030

// 必须配置为本数据节点的 FQDN，如果本机只有一个 hostname，可注释掉本项
fqdn                  h1.taosdata.com

// 配置本数据节点的端口号，缺省是 6030
serverPort            6030
```

一定要修改的参数是 firstEp 和 fqdn。在每个数据节点，firstEp 需全部配置成一样，fqdn 一定要配置成其所在数据节点的值。
以下涉及集群相关的参数必须完全相同，否则不能成功加入到集群中

| #	|配置参数	|含义 |
| ----- | ----- | ---- |
|1|	statusInterval|	dnode 向 mnode 报告状态时长|
|2|	timezone|	时区|
|3|	locale|	系统区位信息及编码格式|
|4|	charset|	字符集编码|

#### 启动集群

启动第一个数据节点，例如 h1.taosdata.com，
```bash
## 启动
systemctl start taosd

## 查看状态
systemctl status taosd
```
然后执行 taos，启动 TDengine CLI，在其中执行命令 “SHOW DNODES”
```bash
$ taos

taos> show dnodes;

id | endpoint | vnodes | support_vnodes | status | create_time | note |
============================================================================================================================================
1 | h1.taosdata.com:6030 | 0 | 1024 | ready | 2022-07-16 10:50:42.673 | |
Query OK, 1 rows affected (0.007984s)
```


#### 添加数据节点

在每个物理节点启动 taosd；（注意：每个物理节点都需要在 taos.cfg 文件中将 firstEp 参数配置为新集群首个节点的 End Point）

在第一个数据节点，使用 CLI 程序 taos，登录进 TDengine 系统，执行命令：
```bash
## 仅为示例 替换为新节点的EndPoint 
CREATE DNODE "h2.taos.com:6030";
```
将新数据节点的 End Point 添加进集群的 EP 列表。“fqdn:port”需要用双引号引起来，否则出错。

查看新节点是否被成功加入，然后执行命令
```
SHOW DNODES;
```

#### 查看数据节点
启动 TDengine CLI 程序 taos，然后执行：
```
SHOW DNODES;
```

#### 删除数据节点

启动 CLI 程序 taos，执行：
```bash
DROP DNODE "fqdn:port";

## 或者
DROP DNODE dnodeId;
```

### Kubernetes(K8S)

#### 前置条件
- Kubernetes v1.5 以上版本
- Kubernetes 已经安装部署并能正常访问使用或更新必要的容器仓库或其他服务
- 使用 minikube、kubectl 和 helm 等工具进行安装部署，需提前安装好相应软件

#### 配置 Service 服务
创建一个 Service 配置文件：taosd-service.yaml，服务名称 metadata.name ，添加 TDengine 所用到的端口：

```yaml

---
apiVersion: v1
kind: Service
metadata:
  name: "taosd"
  labels:
    app: "tdengine"
spec:
  ports:
    - name: tcp6030
      protocol: "TCP"
      port: 6030
    - name: tcp6041
      protocol: "TCP"
      port: 6041
  selector:
    app: "tdengine"

```


#### 有状态服务 StatefulSet

#### 使用 kubectl 命令部署 TDengine 集群



#### 使能端口转发
#### 使用 dashboard 进行图形化管理
#### 集群扩容

TDengine 集群支持自动扩容
将 TDengine 集群扩容到 4 个节点
```bash
kubectl scale statefulsets tdengine --replicas=4
```
先检查 POD 的状态
```bash
kubectl get pods -l app=tdengine
```

查看集群节点
```bash
kubectl exec -i -t tdengine-3 -- taos -s "show dnodes"
```


#### 集群缩容
##### 步骤
- 1使用 kubectl 命令进行缩容需要首先使用 "drop dnodes" 命令

```

$ kubectl exec -i -t tdengine-0 -- taos -s "drop dnode 4"
```

确认移除成功，使用如下命令：
```bash
kubectl exec -it tdengine-0 -- taos -s "show dnodes"
```

- 2 节点删除完成后再进行 Kubernetes 集群缩容。
```bash
kubectl scale statefulsets tdengine --replicas=3
```

POD删除后，需要手动删除PVC
```bash
$ kubectl delete pvc taosdata-tdengine-3
```

#### 清理 TDengine 集群

完整移除 TDengine 集群，需要分别清理 statefulset、svc、configmap、pvc。
```bash
kubectl delete statefulset -l app=tdengine
kubectl delete svc -l app=tdengine
kubectl delete pvc -l app=tdengine
kubectl delete configmap taoscfg
```
### Helm

Helm 是 Kubernetes 的包管理器

#### 安装 Helm
```bash
curl -fsSL -o get_helm.sh \
  https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod +x get_helm.sh
./get_helm.sh
```
#### 安装 TDengine Chart
```bash
wget https://github.com/taosdata/TDengine-Operator/raw/3.0/helm/tdengine-3.0.2.tgz

```

#### 配置 Values
#### 扩容
从部署中获取 StatefulSet 的名称
```bash
export STS_NAME=$(kubectl get statefulset \
  -l "app.kubernetes.io/name=tdengine" \
  -o jsonpath="{.items[0].metadata.name}")

```
扩容操作只增加 replica 即可
```bash
kubectl scale --replicas 3 statefulset/$STS_NAME

```
检查是否扩容成功
```bash
show dnodes 
  
show mnodes
```

#### 缩容

获取需要缩容的 dnode 列表，并手动 Drop
```bash
kubectl --namespace default exec $POD_NAME -- \
  cat /var/lib/taos/dnode/dnodeEps.json \
  | jq '.dnodeInfos[1:] |map(.dnodeFqdn + ":" + (.dnodePort|tostring)) | .[]' -r
kubectl --namespace default exec $POD_NAME -- taos -s "show dnodes"
kubectl --namespace default exec $POD_NAME -- taos -s 'drop dnode "<you dnode in list>"'

```

#### 清理集群

Helm 清理寄去操作也很简单：
```
helm uninstall tdengine
```

## 开发使用

### 建立连接
TDengine 提供了丰富的应用程序开发接口，为了便于用户快速开发自己的应用，TDengine 支持了多种编程语言的连接器
包括支持 C/C++、Java、Python、Go 等等
这些连接器支持使用原生接口（taosc）和 REST 接口连接 TDengine 集群。


#### 连接器建立连接的方式
TDengine 提供两种连接方式
- 使用taosAdapter 组件提供的 REST API 建立与 taosd 的连接，称为“REST 连接”
- 使用客户端驱动程序 taosc 直接与服务端程序 taosd 建立连接，称为“原生连接”

#### 安装客户端驱动 taosc
如果选择原生连接，而且应用程序不在 TDengine 同一台服务器上运行，你需要先安装客户端驱动


##### 安装步骤

1. 下载
- Windows
  TDengine-client-3.0.3.1-Windows-x64.exe
- Linux
  TDengine-client-3.0.3.1-Linux-x64.tar.gz

2. 解压
- Linux
```bash
tar -xzvf TDengine-client-VERSION.tar.gz

```

3. 安装
运行 install_client.sh 安装脚本，用于应用驱动程序

4. 配置 taos.cfg

编辑 taos.cfg 文件（默认路径/etc/taos/taos.cfg），将 firstEP 修改为 TDengine 服务器的 End Point


##### 安装验证
```bash
taos

taos> show databases;
```

#### 使用连接器

Java 引入历来
```pom
<dependency>
  <groupId>com.taosdata.jdbc</groupId>
  <artifactId>taos-jdbcdriver</artifactId>
  <version>3.0.0</version>
</dependency>
```

#### 建立连接

Java 创建连接

- 原生连接

```java
package com.taos.example;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

import com.taosdata.jdbc.TSDBDriver;

public class JNIConnectExample {
  public static void main(String[] args) throws SQLException {
    String jdbcUrl = "jdbc:TAOS://localhost:6030?user=root&password=taosdata";
    Properties connProps = new Properties();
    connProps.setProperty(TSDBDriver.PROPERTY_KEY_CHARSET, "UTF-8");
    connProps.setProperty(TSDBDriver.PROPERTY_KEY_LOCALE, "en_US.UTF-8");
    connProps.setProperty(TSDBDriver.PROPERTY_KEY_TIME_ZONE, "UTC-8");
    Connection conn = DriverManager.getConnection(jdbcUrl, connProps);
    System.out.println("Connected");
    conn.close();
  }
}

```

- REST连接

```java
    public static void main(String[] args) throws SQLException {
        String jdbcUrl = "jdbc:TAOS-RS://localhost:6041?user=root&password=taosdata";
        Connection conn = DriverManager.getConnection(jdbcUrl);
        System.out.println("Connected");
        conn.close();
    }

```

- REST批量拉取功能

```java
    public static void main(String[] args) throws SQLException {
        String jdbcUrl = "jdbc:TAOS-RS://localhost:6041?user=root&password=taosdata";
        Properties connProps = new Properties();
        connProps.setProperty(TSDBDriver.PROPERTY_KEY_BATCH_LOAD, "true");
        Connection conn = DriverManager.getConnection(jdbcUrl, connProps);
        System.out.println("Connected");
        conn.close();
    }
```

### 数据建模

#### 创建库

```sql
CREATE DATABASE power KEEP 365 DURATION 10 BUFFER 16 WAL_LEVEL 1;
```
- 创建一个名为 power 的库
- 这个库的数据将保留 365 天
- 每 10 天一个数据文件
- 每个 VNode 的写入内存池的大小为 16 MB
- WAL 级别，默认为 1
  - 1：写 WAL，但不执行 fsync。
  - 2：写 WAL，而且执行 fsync。

#### 创建超级表
```sql
CREATE STABLE meters (ts timestamp, current float, voltage int, phase float) TAGS (location binary(64), groupId int);
```

#### 创建表
```sql
CREATE TABLE d1001 USING meters TAGS ("California.SanFrancisco", 2);
```

##### 自动建表
```sql
INSERT INTO d1001 USING meters TAGS ("California.SanFrancisco", 2) VALUES (NOW, 10.2, 219, 0.32);
```

#### 多列模型 vs 单列模型
TDengine 支持多列模型，只要物理量是一个数据采集点同时采集的（时间戳一致），这些量就可以作为不同列放在一张超级表里。但还有一种极限的设计，单列模型，每个采集的物理量都单独建表，因此每种类型的物理量都单独建立一超级表。比如电流、电压、相位，就建三张超级表。

TDengine 建议尽可能采用多列模型，因为插入效率以及存储效率更高

### 写入数据

#### 一次写入一条
```sql
INSERT INTO d1001 VALUES (ts1, 10.3, 219, 0.31);
```

#### 一次写入多条
```sql
INSERT INTO d1001 VALUES (ts1, 10.2, 220, 0.23) (ts2, 10.3, 218, 0.25);
```

#### 一次写入多表
```sql
INSERT INTO d1001 VALUES (ts1, 10.3, 219, 0.31) (ts2, 12.6, 218, 0.33) d1002 VALUES (ts3, 12.3, 221, 0.31);
```

### 查询数据

#### 主要查询功能

```sql
select * from d1001 where voltage > 215 order by ts desc limit 2;
```

#### 多表聚合查询

```sql
## 查找加利福尼亚州所有智能电表采集的电压平均值  并按照 location 分组
SELECT AVG(voltage), location FROM meters GROUP BY location;

## 查找 groupId 为 2 的所有智能电表的记录条数  电流的最大值
SELECT count(*), max(current) FROM meters where groupId = 2;

```

#### 降采样查询、插值

物联网场景里，经常需要通过降采样（down sampling）将采集的数据按时间段进行聚合。

```sql
SELECT _wstart, sum(current) FROM d1001 INTERVAL(10s);

SELECT _wstart, SUM(current) FROM meters where location like "California%" INTERVAL(1s);

SELECT _wstart, SUM(current) FROM meters INTERVAL(1s, 500a);


```

### 流式计算

#### 流式计算的创建
```sql
CREATE STREAM [IF NOT EXISTS] stream_name [stream_options] INTO stb_name AS subquery
stream_options: {
 TRIGGER    [AT_ONCE | WINDOW_CLOSE | MAX_DELAY time]
 WATERMARK   time
 IGNORE EXPIRED [0 | 1]
}
```


### 数据订阅
TDengine 提供了类似消息队列产品的数据订阅、消费接口。这样在很多场景下，采用 TDengine 的时序数据处理系统不再需要集成消息队列产品，比如 kafka, 从而简化系统设计的复杂度，降低运营维护成本。



#### 主要API
```java
void subscribe(Collection<String> topics) throws SQLException;

void unsubscribe() throws SQLException;

Set<String> subscription() throws SQLException;

ConsumerRecords<V> poll(Duration timeout) throws SQLException;

void commitAsync();

void commitAsync(OffsetCommitCallback callback);

void commitSync() throws SQLException;

void close() throws SQLException;
```
#### 写入数据
```java
DROP DATABASE IF EXISTS tmqdb;
CREATE DATABASE tmqdb;
CREATE TABLE tmqdb.stb (ts TIMESTAMP, c1 INT, c2 FLOAT, c3 VARCHAR(16)) TAGS(t1 INT, t3 VARCHAR(16));
CREATE TABLE tmqdb.ctb0 USING tmqdb.stb TAGS(0, "subtable0");
CREATE TABLE tmqdb.ctb1 USING tmqdb.stb TAGS(1, "subtable1");       
INSERT INTO tmqdb.ctb0 VALUES(now, 0, 0, 'a0')(now+1s, 0, 0, 'a00');
INSERT INTO tmqdb.ctb1 VALUES(now, 1, 1, 'a1')(now+1s, 11, 11, 'a11');
```

#### 创建订阅主题
```sql
CREATE TOPIC [IF NOT EXISTS] topic_name AS subquery;
```

##### 列订阅
```sql
CREATE TOPIC topic_name as subquery;
```


##### 超级表订阅
```sql
CREATE TOPIC topic_name AS STABLE stb_name

```

##### 数据库订阅
```sql
CREATE TOPIC topic_name AS DATABASE db_name;

```

#### 创建消费者 consumer
##### 订阅 topics
```java
List<String> topics = new ArrayList<>();
topics.add("tmq_topic");
consumer.subscribe(topics);
```

#### 消费
```java
while(running){
  ConsumerRecords<Meters> meters = consumer.poll(Duration.ofMillis(100));
    for (Meters meter : meters) {
      processMsg(meter);
    }    
}
```

#### 结束消费
```java
/* 取消订阅 */
consumer.unsubscribe();

/* 关闭消费 */
consumer.close();
```


#### 删除订阅主题
```sql
DROP TOPIC [IF EXISTS] topic_name;
```
#### 查看订阅主题
```sql
SHOW TOPICS
```

#### 创建消费组

消费组的创建只能通过 TDengine 客户端驱动或者连接器所提供的 API 创建。


#### 删除消费组
```sql
DROP CONSUMER GROUP [IF EXISTS] cgroup_name ON topic_name;
```
#### 查看消费组
```sql
SHOW CONSUMERS;
```


#### 状态查看
1、topics：查询已经创建的 topic
```sql
SHOW TOPICS;
```

2、consumers：查询 consumer 的状态及其订阅的 topic
```sql
SHOW CONSUMERS;
```

3、subscriptions：查询 consumer 与 vgroup 之间的分配关系
```sql
SHOW SUBSCRIPTIONS;
```


### 缓存

#### 写缓存
```sql
create database db0 vgroups 100 buffer 16MB
```
#### 读缓存
在创建数据库时可以选择是否缓存该数据库中每个子表的最新数据。
- none: 不缓存
last_row: 缓存子表最近一行数据，这将显著改善 last_row 函数的性能
last_value: 缓存子表每一列最近的非 NULL 值，这将显著改善无特殊影响（比如 WHERE, ORDER BY, GROUP BY, INTERVAL）时的 last 函数的性能
both: 同时缓存最近的行和列，即等同于上述 cachemodel 值为 last_row 和 last_value 的行为同时生效

#### 元数据缓存
```sql
create database db0 pages 128 pagesize 16kb
```

#### 文件系统缓存

TDengine 利用 WAL 技术来提供基本的数据可靠性。写入 WAL 本质上是以顺序追加的方式写入磁盘文件。此时文件系统缓存在写入性能中也会扮演关键角色。在创建数据库时可以利用 wal 参数来选择性能优先或者可靠性优先。

- 1: 写 WAL 但不执行 fsync ，新写入 WAL 的数据保存在文件系统缓存中但并未写入磁盘，这种方式性能优先
- 2: 写 WAL 且执行 fsync，新写入 WAL 的数据被立即同步到磁盘上，可靠性更高

#### 客户端缓存

TDengine 的所有客户端都要调用的核心库 libtaos.so （也称为 taosc ）中也充分利用了缓存技术。在 taosc 中会缓存所访问过的各个数据库、超级表以及子表的元数据，集群的拓扑结构等关键元数据。
其它客户端所缓存的元数据不同步或失效的情况，此时需要在客户端执行 "reset query cache" 以让整个缓存失效从而强制重新拉取最新的元数据重新建立缓存。

### 自定义函数

## 连接器

### Java连接TDengin
#### 安装步骤
```pom
<dependency>
  <groupId>com.taosdata.jdbc</groupId>
  <artifactId>taos-jdbcdriver</artifactId>
  <version>3.0.0</version>
</dependency>
```

#### 建立连接

##### 建立连接
##### 原生连接
```java
Class.forName("com.taosdata.jdbc.TSDBDriver");
String jdbcUrl = "jdbc:TAOS://taosdemo.com:6030/test?user=root&password=taosdata";
Connection conn = DriverManager.getConnection(jdbcUrl);
```
##### REST连接
```java
Class.forName("com.taosdata.jdbc.TSDBDriver");
String jdbcUrl = "jdbc:TAOS://taosdemo.com:6030/test?user=root&password=taosdata";
Connection conn = DriverManager.getConnection(jdbcUrl);
```

##### 原生连接连接 TDengine 集群
需要在配置文件中指定集群的 firstEp、secondEp 等参数
```java
public Connection getConn() throws Exception{
  Class.forName("com.taosdata.jdbc.TSDBDriver");
  String jdbcUrl = "jdbc:TAOS://:/test?user=root&password=taosdata";
  Properties connProps = new Properties();
  connProps.setProperty(TSDBDriver.PROPERTY_KEY_CHARSET, "UTF-8");
  connProps.setProperty(TSDBDriver.PROPERTY_KEY_LOCALE, "en_US.UTF-8");
  connProps.setProperty(TSDBDriver.PROPERTY_KEY_TIME_ZONE, "UTC-8");
  Connection conn = DriverManager.getConnection(jdbcUrl, connProps);
  return conn;
}
```

###### 配置文件
```conf
# first fully qualified domain name (FQDN) for TDengine system
firstEp cluster_node1:6030

# second fully qualified domain name (FQDN) for TDengine system, for cluster only
secondEp cluster_node2:6030

# default system charset
# charset UTF-8

# system locale
# locale en_US.UTF-8
```

##### 使用Properties 获取连接
除了通过指定的 URL 获取连接，还可以使用 Properties 指定建立连接时的参数。
```java
public Connection getConn() throws Exception{
  Class.forName("com.taosdata.jdbc.TSDBDriver");
  String jdbcUrl = "jdbc:TAOS://taosdemo.com:6030/test?user=root&password=taosdata";
  Properties connProps = new Properties();
  connProps.setProperty(TSDBDriver.PROPERTY_KEY_CHARSET, "UTF-8");
  connProps.setProperty(TSDBDriver.PROPERTY_KEY_LOCALE, "en_US.UTF-8");
  connProps.setProperty(TSDBDriver.PROPERTY_KEY_TIME_ZONE, "UTC-8");
  connProps.setProperty("debugFlag", "135");
  connProps.setProperty("maxSQLLength", "1048576");
  Connection conn = DriverManager.getConnection(jdbcUrl, connProps);
  return conn;
}

public Connection getRestConn() throws Exception{
  Class.forName("com.taosdata.jdbc.rs.RestfulDriver");
  String jdbcUrl = "jdbc:TAOS-RS://taosdemo.com:6041/test?user=root&password=taosdata";
  Properties connProps = new Properties();
  connProps.setProperty(TSDBDriver.PROPERTY_KEY_BATCH_LOAD, "true");
  Connection conn = DriverManager.getConnection(jdbcUrl, connProps);
  return conn;
}

```

#### 使用示例

> 具体Java使用代码在后面有详述，这里略过
- 创建数据库和表
- 插入数据
- 创建数据库和表
- 插入数据
- 查询数据
- 处理异常
- 通过参数绑定写入数据
- 无模式写入
- 数据订阅
  - 创建 Topic
  - 创建 Consumer
  - 订阅消费数据
  - 关闭订阅
  - 完整示例
- 与连接池使用
  - HikariCP
  - Druid
  - 更多示例程序

## SQL手册

### 数据类型

#### 时间戳

使用 TDengine，最重要的是时间戳，缺省的时间戳精度是毫秒，但通过在 CREATE DATABASE 时传递的 PRECISION 参数也可以支持微秒和纳秒。

```java
CREATE DATABASE db_name PRECISION 'ns';
```

#### 数据类型

| #| 	类型                                      |	Bytes|	说明 |
| ---- |------------------------------------------| ---- | ---- |
|1	| TIMESTAMP	| 8	|时间戳。缺省精度毫秒，可支持微秒和纳秒，详细说明见上节。 |
|2	| INT	| 4	|整型，范围 [-2^31, 2^31-1]|
|3	| INT UNSIGNED|	4| 	无符号整数，[0, 2^32-1]|
|4	| BIGINT	| 8	|长整型，范围 [-2^63, 2^63-1]|
|5	| BIGINT  UNSIGNED|	8|	长整型，范围 [0, 2^64-1]|
|6	| FLOAT	| 4	|浮点型，有效位数 6-7，范围 [-3.4E38, 3.4E38]|
|7	| DOUBLE	| 8	|双精度浮点型，有效位数 15-16，范围 [-1.7E308, 1.7E308]|
|8	| BINARY	| 自定义	|记录单字节字符串，建议只用于处理 ASCII 可见字符，中文等多字节字符需使用 NCHAR
|9	| SMALLINT	| 2	|短整型， 范围 [-32768, 32767]
|10	| SMALLINT | UNSIGNED|	2	无符号短整型，范围 [0, 65535]
|11	| TINYINT	| 1|	单字节整型，范围 [-128, 127]
|12	| TINYINT | UNSIGNED|	1	无符号单字节整型，范围 [0, 255]
|13	| BOOL	| 1	|布尔型，{true, false}
|14	| NCHAR	| 自定义	|记录包含多字节字符在内的字符串，如中文字符。每个 NCHAR 字符占用 4 字节的存储空间。字符串两端使用单引号引用，字符串内的单引号需用转义字符 \'。NCHAR 使用时须指定字符串大小，类型为 NCHAR(10) 的列表示此列的字符串最多存储 10 个 NCHAR 字符。如果用户字符串长度超出声明长度，将会报错。
|15	| JSON	| 	JSON |数据类型， 只有 Tag 可以是 JSON 格式
|16	| VARCHAR| 	自定义 |	BINARY 类型的别名



### 数据库

- 创建

	- 方法
    ```sql
	  CREATE DATABASE [IF NOT EXISTS] db_name [database_options]
    ```
- 使用

  USE db_name;    

- 删除

	- 方法

    DROP DATABASE [IF EXISTS] db_name	  


- 修改参数
  - 方法
    ALTER DATABASE db_name [alter_database_options]

- 修改cachesize
  - 查看cachesize
    select * from information_schema.ins_databases;
  - 查看cacheload
    show <db_name>.vgroups;
- 查看数据库
  - 方法
    SHOW DATABASES;
  - 查看数据库创建语句
    SHOW CREATE DATABASE db_name \G;
  - 查看数据库参数
    SELECT * FROM INFORMATION_SCHEMA.INS_DATABASES WHERE NAME='db_name' \G;
- 删除过期数据库
  TRIM DATABASE db_name;
- 落盘内存数据
  FLUSH DATABASE db_name;
- 自动调整VGROUP中VNODE的分布
  BALANCE VGROUP
- 查看数据库工作状态
  SHOW db_name.ALIVE;

### 表
- 创建表
  - 方法
    CREATE TABLE [IF NOT EXISTS] [db_name.]tb_name (create_definition [, create_definition] ...) [table_options]
- 创建字表
  CREATE TABLE [IF NOT EXISTS] tb_name USING stb_name TAGS (tag_value1, ...);
- 创建字表并指定标签的值
  CREATE TABLE [IF NOT EXISTS] tb_name USING stb_name (tag_name1, ...) TAGS (tag_value1, ...);
- 批量创建字表
  CREATE TABLE [IF NOT EXISTS] tb_name1 USING stb_name TAGS (tag_value1, ...) [IF NOT EXISTS] tb_name2 USING stb_name TAGS (tag_value2, ...) ...;

- 修改普通表
  - 方法
    ALTER TABLE [db_name.]tb_name alter_table_clause
- 修改子表
  - 原则
    对子表的列和标签的修改，除了更改标签值以外，都要通过超级表才能进行。
  - 方法
    ALTER TABLE [db_name.]tb_name alter_table_clause
- 删除表
  - 方法
    DROP TABLE [IF EXISTS] [db_name.]tb_name [, [IF EXISTS] [db_name.]tb_name] ...
- 查看表
  -  显示所有表
    show tables  [LIKE tb_name_wildchar];
  - 显示建表语句
    SHOW CREATE TABLE tb_name;
  - 获取表结构信息
    DESCRIBE [db_name.]tb_name;

### 超级表
```sql
CREATE STABLE [IF NOT EXISTS] stb_name (create_definition [, create_definition] ...) TAGS (create_definition [, create_definition] ...) [table_options]
 
create_definition:
    col_name column_definition
 
column_definition:
    type_name [COMMENT 'string_value']
```
#### 创建超级表
```sql


```

#### 查看超级表
```sql
SHOW STABLES [LIKE tb_name_wildcard];
```

#### 删除超级表
```sql
DROP STABLE [IF EXISTS] [db_name.]stb_name
```

#### 修改超级表
```sql
ALTER STABLE [db_name.]tb_name alter_table_clause
 
alter_table_clause: {
    alter_table_options
  | ADD COLUMN col_name column_type
  | DROP COLUMN col_name
  | MODIFY COLUMN col_name column_type
  | ADD TAG tag_name tag_type
  | DROP TAG tag_name
  | MODIFY TAG tag_name tag_type
  | RENAME TAG old_tag_name new_tag_name
}
 
alter_table_options:
    alter_table_option ...
 
alter_table_option: {
    COMMENT 'string_value'
}

```

### 数据写入

#### 语法
```sql
INSERT INTO
    tb_name
        [USING stb_name [(tag1_name, ...)] TAGS (tag1_value, ...)]
        [(field1_name, ...)]
        VALUES (field1_value, ...) [(field1_value2, ...) ...] | FILE csv_file_path
    [tb2_name
        [USING stb_name [(tag1_name, ...)] TAGS (tag1_value, ...)]
        [(field1_name, ...)]
        VALUES (field1_value, ...) [(field1_value2, ...) ...] | FILE csv_file_path
    ...];

INSERT INTO tb_name [(field1_name, ...)] subquery
```
#### 插入一条记录
```sql
INSERT INTO d1001 VALUES (NOW, 10.2, 219, 0.32);
```

#### 插入多条记录
```sql
INSERT INTO d1001 VALUES ('2021-07-13 14:06:32.272', 10.2, 219, 0.32) (1626164208000, 10.15, 217, 0.33);
```
#### 向多个表插入数据
```sql
INSERT INTO d1001 VALUES ('2021-07-13 14:06:34.630', 10.2, 219, 0.32) ('2021-07-13 14:06:35.779', 10.15, 217, 0.33)
            d1002 (ts, current, phase) VALUES ('2021-07-13 14:06:34.255', 10.27, 0.31）;
```
#### 自定列插入
```sql
INSERT INTO d1001 (ts, current, phase) VALUES ('2021-07-13 14:06:33.196', 10.27, 0.31);
```

#### 插入记录时自动建表
```sql
INSERT INTO d21001 USING meters TAGS ('California.SanFrancisco', 2) VALUES ('2021-07-13 14:06:32.272', 10.2, 219, 0.32);
```

#### 插入来自文件的数据记录
```sql
INSERT INTO d1001 FILE '/tmp/csvfile.csv';
```
#### 插入来自文件的记录并自定建表
```sql
INSERT INTO d21001 USING meters TAGS ('California.SanFrancisco', 2) FILE '/tmp/csvfile.csv';
```


### 数据查询

#### 查询语法
```sql
SELECT {DATABASE() | CLIENT_VERSION() | SERVER_VERSION() | SERVER_STATUS() | NOW() | TODAY() | TIMEZONE()}

```


#### 查询列表
```sql
SELECT * FROM d1001;
SELECT d1001.* FROM d1001;
```
#### 查询对象
FROM 关键字后面可以是若干个表（超级表）列表，也可以是子查询的结果。 如果没有指定用户的当前数据库，可以在表名称之前使用数据库的名称来指定表所属的数据库。

#### GROUP BY
ORDER BY 子句对结果集排序。
#### LIMIT
LIMIT 控制输出条数，OFFSET 指定从第几条之后开始输出。LIMIT/OFFSET 对结果集的执行顺序在 ORDER BY 之后。
#### SLMIT
SLIMIT 和 PARTITION BY/GROUP BY 子句一起使用，用来控制输出的分片的数量。
#### 特色工程

#### 正则

#### CASE
```sql
CASE value WHEN compare_value THEN result [WHEN compare_value THEN result ...] [ELSE result] END
CASE WHEN condition THEN result [WHEN condition THEN result ...] [ELSE result] END
```

#### JOIN 字句
TDengine 支持基于时间戳主键的内连接，即 JOIN 条件必须包含时间戳主键。
#### 嵌套查询
“嵌套查询”又称为“子查询”，也即在一条 SQL 语句中，“内层查询”的计算结果可以作为“外层查询”的计算对象来使用。
TDengine 的查询引擎开始支持在 FROM 子句中使用非关联子查询（“非关联”的意思是，子查询不会用到父查询中的参数）。

#### UNION ALL字句
```sql
SELECT ...
UNION ALL SELECT ...
[UNION ALL SELECT ...]
```
#### SQL示例
```sql
SELECT (col1 + col2) AS 'complex' FROM tb1 WHERE ts > '2018-06-01 08:00:00.000' AND col2 > 1.2 LIMIT 10 OFFSET 5;
```

### 标签检索

#### 语法

##### 创建索引
CREATE INDEX index_name ON tbl_name (tagColName）

##### 删除索引
DROP INDEX index_name

##### 查看索引
SELECT * FROM information_schema.INS_INDEXES

##### 使用说明
- 支持的过滤算子有 =, >, >=, <, <=。
- 针对一个 tag 列只能建立一个索引，如果重复创建索引则会报错。
- 每次只能针对一个 tag 列建立一个索引，不能同时对多个 tag 建立索引。

- 整个系统中不管是哪种类型的索引，其名称必须唯一。

- 对索引个数没有限制，但每增加一个索引都会导致系统中的元数据增加，过多的索引会降低元数据存取的效率从而降低整个系统的性能。所以请尽量避免添加不必要的索引。

- 不支持对普通和子表建立索引。

- 如果某个 tag 列的唯一值较少时，不建议对其建立索引，这种情况下收效甚微。



### 函数
#### 单行函数
##### 数学函数
- ABS
- ACOS
- ASIN
- ATAN
- CEIL
- COS
- FLOOR
- LOG
- POW
- ROUND
- SIN
- SQRT
- TAN
#####  字符串函数
- CHAR_LENGTH
- CONCAT
- CONCAT_WS
- LENGTH
- LOWER
- LTRIM
- RTRIM
- SUBSTR
- UPPER
#####  转换函数
- CAST
- TO_ISO8601
- TO_JSON
- TO_UNIXTIMESTAMP
##### 时间和日期函数
- NOW
- TIMEDIFF
- TIMETRUNCATE
- TIMEZONE
- TODAY
##### 聚合函数
- APERCENTILE
- AVG
- COUNT
- ELAPSED
- LEASTSQUARES
- SPREAD
- STDDEV
- SUM
- HYPERLOGLOG
- HISTOGRAM
- PERCENTILE
##### 选择函数
- BOTTOM
- FIRST
- INTERP
- LAST
- LAST_ROW
- MAX
- MIN
- MODE
- SAMPLE
- TAIL
- TOP
- UNIQUE
##### 时序数据特有函数
- CSUM
- DERIVATIVE
- DIFF
- IRATE
- MAVG
- STATECOUNT
- STATEDURATION
- TWA
##### 系统信息函数
- DATABASE
- CLIENT_VERSION
- SERVER_VERSION
- SERVER_STATUS

### 特色查询
提供了一系列满足时序业务场景需求的特色查询语法，这些语法能够为时序场景的应用的开发带来极大的便利。
#### 数据切分查询
当需要按一定的维度对数据进行切分然后在切分出的数据空间内再进行一系列的计算时使用数据切分子句
```sql
PARTITION BY part_list
```
#### 窗口切分查询
TDengine 支持按时间窗口切分方式进行聚合结果查询，比如温度传感器每秒采集一次数据，但需查询每隔 10 分钟的温度平均值。这种场景下可以使用窗口子句来获得需要的查询结果。窗口子句用于针对查询的数据集合按照窗口切分成为查询子集并进行聚合，窗口包含时间窗口（time window）、状态窗口（status window）、会话窗口（session window）、条件窗口（event window）四种窗口。其中时间窗口又可划分为滑动时间窗口和翻转时间窗口。
``` 
window_clause: {
    SESSION(ts_col, tol_val)
  | STATE_WINDOW(col)
  | INTERVAL(interval_val [, interval_offset]) [SLIDING (sliding_val)] [FILL(fill_mod_and_val)]
  | EVENT_WINDOW START WITH start_trigger_condition END WITH end_trigger_condition
}
```
- 窗口子句的规则

- FILL 子句
  FILL 语句指定某一窗口区间数据缺失的情况下的填充模式。

- 时间窗口
  时间窗口又可分为滑动时间窗口和翻转时间窗口。

- 状态窗口
  使用整数（布尔值）或字符串来标识产生记录时候设备的状态量。产生的记录如果具有相同的状态量数值则归属于同一个状态窗口，数值改变后该窗口关闭。
- 会话窗口
  会话窗口根据记录的时间戳主键的值来确定是否属于同一个会话。
- 事件窗口
  事件窗口根据开始条件和结束条件来划定窗口，当start_trigger_condition满足时则窗口开始，直到end_trigger_condition满足时窗口关闭。
- 时间戳伪列
  窗口聚合查询结果中，如果 SQL 语句中没有指定输出查询结果中的时间戳列，那么最终结果中不会自动包含窗口的时间列信息。如果需要在结果中输出聚合结果所对应的时间窗口信息，需要在 SELECT 子句中使用时间戳相关的伪列: 时间窗口起始时间 (_WSTART), 时间窗口结束时间 (_WEND), 时间窗口持续时间 (_WDURATION), 以及查询整体窗口相关的伪列: 查询窗口起始时间(_QSTART) 和查询窗口结束时间(_QEND)。需要注意的是时间窗口起始时间和结束时间均是闭区间，时间窗口持续时间是数据当前时间分辨率下的数值。
```sql
CREATE TABLE meters (ts TIMESTAMP, current FLOAT, voltage INT, phase FLOAT) TAGS (location BINARY(64), groupId INT);
```

### 数据订阅

- 创建订阅主题
```sql
CREATE TOPIC [IF NOT EXISTS] topic_name AS subquery;
```
- 删除订阅主题
```sql
DROP TOPIC [IF EXISTS] topic_name;
```
- 查看订阅主题
```sql
SHOW TOPICS;
```
- 创建消费组
  消费组的创建只能通过 TDengine 客户端驱动或者连接器所提供的 API 创建。

- 删除消费组
```sql
DROP CONSUMER GROUP [IF EXISTS] cgroup_name ON topic_name;
```

- 查看消费组
```sql
SHOW CONSUMERS;
```

### 流式计算
#### 创建流式计算
```sql
CREATE STREAM [IF NOT EXISTS] stream_name [stream_options] INTO stb_name SUBTABLE(expression) AS subquery
stream_options: {
 TRIGGER    [AT_ONCE | WINDOW_CLOSE | MAX_DELAY time]
 WATERMARK   time
}
```

#### 流式计算的 partition
  可以使用 PARTITION BY TBNAME，tag，普通列或者表达式，对一个流进行多分区的计算，每个分区的时间线与时间窗口是独立的，会各自聚合，并写入到目的表中的不同子表。

####  流式计算读取历史数据


#### 删除流式计算
```sql
DROP STREAM [IF EXISTS] stream_name;
```
#### 展示流式计算
```sql
SHOW STREAMS;
```

#### 流式计算的触发模式
在创建流时，可以通过 TRIGGER 指令指定流式计算的触发模式。

对于非窗口计算，流式计算的触发是实时的；对于窗口计算，目前提供 3 种触发模式，默认为 AT_ONCE：

AT_ONCE：写入立即触发

WINDOW_CLOSE：窗口关闭时触发（窗口关闭由事件时间决定，可配合 watermark 使用）

MAX_DELAY time：若窗口关闭，则触发计算。若窗口未关闭，且未关闭时长超过 max delay 指定的时间，则触发计算。

#### 流式计算的窗口关闭


#### 流式计算的过期数据处理策略

### 运算符
#### 算术运算符

|#|	运算符|	支持的类型|	说明|
| ----|----| ----| ----|
|1|	+, -	|数值类型|	表达正数和负数，一元运算符|
|2|	+, -	|数值类型|	表示加法和减法，二元运算符|
|3|	*, /	|数值类型|	表示乘法和除法，二元运算符|
|4|	%	|数值类型|	表示取余运算，二元运算符|

#### 位运算符
|#	| 运算符	          |支持的类型|	说明|
| ----|---------------| ----| ----|
|1	| &	            |数值类型|	按位与，二元运算符|
|2	| 	&#124;     	 |数值类型|	按位或，二元运算符|

#### JSON 运算符
-> 运算符可以对 JSON 类型的列按键取值。-> 左侧是列标识符，右侧是键的字符串常量，如 col->'name'，返回键 'name' 的值。

#### 集合运算符
支持 UNION ALL 和 UNION 操作符。

#### 比较运算符

|#|	运算符|	支持的类型|	说明|
| ----|---------------| ----| ----|
|1|	=	|除 BLOB、MEDIUMBLOB 和 JSON 外的所有类型	相等|
|2|	<>, !=	|除 BLOB、MEDIUMBLOB 和 JSON 外的所有类型，且不可以为表的时间戳主键列	不相等|
|3|	>, <	|除 BLOB、MEDIUMBLOB 和 JSON 外的所有类型	大于，小于|
|4|	>=, <=	|除 BLOB、MEDIUMBLOB 和 JSON 外的所有类型	大于等于，小于等于|
|5|	IS [NOT] NULL	|所有类型	是否为空值|
|6|	[NOT] BETWEEN AND	|除 BOOL、BLOB、MEDIUMBLOB 和 JSON 外的所有类型	闭区间比较|
|7|	IN	|除 BLOB、MEDIUMBLOB 和 JSON 外的所有类型，且不可以为表的时间戳主键列	与列表内的任意值相等|
|8|	LIKE	|BINARY、NCHAR 和 VARCHAR	通配符匹配|
|9|	MATCH, NMATCH	|BINARY、NCHAR 和 VARCHAR	正则表达式匹配|
|10|	CONTAINS|	JSON	JSON 中是否存在某键|


#### 逻辑运算符
|#|	运算符|	支持的类型|	说明|
| ----|---------------| ----| ----|
|1|	AND|	BOOL	|逻辑与，如果两个条件均为 TRUE， 则返回 TRUE。如果任一为 FALSE，则返回 FALSE|
|2|	OR	|BOOL	|逻辑或，如果任一条件为 TRUE， 则返回 TRUE。如果两者都是 FALSE，则返回 FALSE|

### JSON类型
#### 语法说明
1、创建 json 类型 tag
```sql
create stable s1 (ts timestamp, v1 int) tags (info json)

create table s1_1 using s1 tags ('{"k1": "v1"}')
```
2、json 取值操作符 ->
```sql
select * from s1 where info->'k1' = 'v1'

select info->'k1' from s1
```
3、json key 是否存在操作符 contains
```sql
select * from s1 where info contains 'k2'

select * from s1 where info contains 'k1'
```

#### 支持的操作
1、在 where 条件中时，支持函数 match/nmatch/between and/like/and/or/is null/is no null，不支持 in
```sql
select * from s1 where info->'k1' match 'v*';

select * from s1 where info->'k1' like 'v%' and info contains 'k2';

select * from s1 where info is null;

select * from s1 where info->'k1' is not null
```
2、支持 json tag 放在 group by、order by、join 子句、union all 以及子查询中，比如 group by json->'key'
3、支持 distinct 操作
```sql
select distinct info->'k1' from s1
```
4、标签操作
支持修改 json 标签值（全量覆盖）

支持修改 json 标签名

不支持添加 json 标签、删除 json 标签、修改 json 标签列宽


#### 其他约束条件
1、只有标签列可以使用 json 类型，如果用 json 标签，标签列只能有一个。
2、长度限制：json 中 key 的长度不能超过 256，并且 key 必须为可打印 ascii 字符；json 字符串总长度不超过 4096 个字节。
3、json 格式限制
4、当查询 json 中不存在的 key 时，返回 NULL
5、当 json tag 作为子查询结果时，不再支持上层查询继续对子查询中的 json 串做解析查询。
比如暂不支持
```sql
select jtag->'key' from (select jtag from stable)
```
不支持
```sql
select jtag->'key' from (select jtag from stable) where jtag->'key'>0
```


### 转义字符

#### 转义字符表

|字符序列|	代表的字符|
|----|----|
|\'	|单引号'|
|\"	|双引号"|
|\n	|换行符|
|\r	|回车符|
|\t	|tab 符|
|\\	|斜杠\|
|\%	|% 规则见下|
|\_	|_ 规则见下|


#### 转义字符使用规则
1、标识符里有转义字符（数据库名、表名、列名）
- 普通标识符： 直接提示错误的标识符，因为标识符规定必须是数字、字母和下划线，并且不能以数字开头。
- 反引号``标识符： 保持原样，不转义
2、数据里有转义字符
- 遇到上面定义的转义字符会转义（%和_见下面说明），如果没有匹配的转义字符会忽略掉转义符\。
- 对于%和_，因为在 like 里这两个字符是通配符，所以在模式匹配 like 里用\%%和\_表示字符里本身的%和_，如果在 like 模式匹配上下文之外使用\%或\_，则它们的计算结果为字符串\%和\_，而不是%和_。

### 命名与边界
#### 名称命名规则
- 合法字符：英文字符、数字和下划线

- 允许英文字符或下划线开头，不允许以数字开头

- 不区分大小写

- 转义后表（列）名规则

  为了支持更多形式的表（列）名，TDengine支持转义符 "`"。可用让表名与关键词不冲突，同时不受限于上述表名称合法性约束检查 转义后的表（列）名同样受到长度限制要求，且长度计算的时候不计算转义符。使用转义字符以后，不再对转义字符中的内容进行大小写统一

#### 密码合法字符集

[a-zA-Z0-9!?$%^&*()_–+={[}]:;@~#|<,>.?/]

去掉了 ‘“`\ (单双引号、撇号、反斜杠、空格)

#### 一般限制
- 数据库名最大长度为 64 字节
- 表名最大长度为 192 字节，不包括数据库名前缀和分隔符
- 每行数据最大长度 48KB （注意：数据行内每个 BINARY/NCHAR 类型的列还会额外占用 2 个字节的存储位置）
- 列名最大长度为 64 字节
- 最多允许 4096 列，最少需要 2 列，第一列必须是时间戳。
- 标签名最大长度为 64 字节
- 最多允许 128 个，至少要有 1 个标签，一个表中标签值的总长度不超过 16KB
- SQL 语句最大长度 1048576 个字符
- SELECT 语句的查询结果，最多允许返回 4096 列（语句中的函数调用可能也会占用一些列空间），超限时需要显式指定较少的返回数据列，以避免语句执行报错
- 库的数目，超级表的数目、表的数目，系统不做限制，仅受系统资源限制
- 数据库的副本数只能设置为 1 或 3
- 用户名的最大长度是 23 字节
- 用户密码的最大长度是 128 字节
- 总数据行数取决于可用资源
- 单个数据库的虚拟结点数上限为 1024

#### 表(列)名合法性说明
##### TDengine 中的表（列）名命名规则
只能由字母、数字、下划线构成，数字不能在首位，长度不能超过 192 字节，不区分大小写。这里表名称不包括数据库名的前缀和分隔符。

##### 转义后表（列）名规则
为了兼容支持更多形式的表（列）名，TDengine 引入新的转义符 "`"，可以避免表名与关键词的冲突，同时不受限于上述表名合法性约束检查，转义符不计入表名的长度。 转义后的表（列）名同样受到长度限制要求，且长度计算的时候不计算转义符。使用转义字符以后，不再对转义字符中的内容进行大小写统一。

### 保留关键字
#### 保留关键字
TDengine 有 200 多个内部保留关键字，这些关键字如果需要用作库名、表名、超级表名、子表名、数据列名及标签列名等，无论大小写，需要使用符号 ` 将关键字括起来使用，例如 `ADD`。

#### 关键字列表

### 集群管理

#### 创建数据节点
```sql
CREATE DNODE {dnode_endpoint | dnode_host_name PORT port_val}
```

#### 查看数据节点
```sql
SHOW DNODES;
```

#### 删除数据节点
```sql
DROP DNODE {dnode_id | dnode_endpoint}
```

#### 修改数据节点配置
```sql
ALTER DNODE dnode_id dnode_option

ALTER ALL DNODES dnode_option

dnode_option: {
    'resetLog'
  | 'balance' 'value'
  | 'monitor' 'value'
  | 'debugFlag' 'value'
  | 'monDebugFlag' 'value'
  | 'vDebugFlag' 'value'
  | 'mDebugFlag' 'value'
  | 'cDebugFlag' 'value'
  | 'httpDebugFlag' 'value'
  | 'qDebugflag' 'value'
  | 'sdbDebugFlag' 'value'
  | 'uDebugFlag' 'value'
  | 'tsdbDebugFlag' 'value'
  | 'sDebugflag' 'value'
  | 'rpcDebugFlag' 'value'
  | 'dDebugFlag' 'value'
  | 'mqttDebugFlag' 'value'
  | 'wDebugFlag' 'value'
  | 'tmrDebugFlag' 'value'
  | 'cqDebugFlag' 'value'
}
```

#### 添加管理节点
```sql
CREATE MNODE ON DNODE dnode_id
```

#### 查看管理节点
```sql
SHOW MNODES;
```

#### 删除管理节点
```sql
DROP MNODE ON DNODE dnode_id;
```

#### 创建查询节点
```sql
CREATE QNODE ON DNODE dnode_id;
```

#### 查看查询节点
```sql
SHOW QNODES;
```

#### 删除查询节点
```sql
DROP QNODE ON DNODE dnode_id;
```

#### 查询集群状态
```sql
SHOW CLUSTER ALIVE;
```

#### 修改客户端配置
````sql
ALTER LOCAL local_option

local_option: {
    'resetLog'
  | 'rpcDebugFlag' 'value'
  | 'tmrDebugFlag' 'value'
  | 'cDebugFlag' 'value'
  | 'uDebugFlag' 'value'
  | 'debugFlag' 'value'
}
````

#### 查看客户端配置
```sql
SHOW LOCAL VARIABLES;
```

### 元数据
#### 元数据
TDengine 内置了一个名为 INFORMATION_SCHEMA 的数据库，提供对数据库元数据、数据库系统信息和状态的访问，例如数据库或表的名称，当前执行的 SQL 语句等。该数据库存储有关 TDengine 维护的所有其他数据库的信息。它包含多个只读表。实际上，这些表都是视图，而不是基表，因此没有与它们关联的文件。所以对这些表只能查询，不能进行 INSERT 等写入操作。
#### 内置元数据
- INS_DNODES
- INS_MNODES
- INS_QNODES
- INS_CLUSTER
- INS_DATABASES
- INS_FUNCTIONS
- INS_INDEXES
- INS_STABLES
- INS_TABLES
- INS_TAGS
- INS_COLUMNS
- INS_USERS
- INS_GRANTS
- INS_VGROUPS
- INS_CONFIGS
- INS_DNODE_VARIABLES
- INS_TOPICS
- INS_SUBSCRIPTIONS
- INS_STREAMS

### 统计数据

- PERF_APP
- PERF_CONNECTIONS
- PERF_QUERIES
- PERF_CONSUMERS
- PERF_TRANS
- PERF_SMAS

### SHOW命令

- SHOW APPS
- SHOW CLUSTER
- SHOW CONNECTIONS
- SHOW CONSUMERS
- SHOW CREATE DATABASE
- SHOW CREATE STABLE
- SHOW CREATE TABLE
- SHOW DATABASES
- SHOW DNODES
- SHOW FUNCTIONS
- SHOW LICENCES
- SHOW INDEXES
- SHOW LOCAL VARIABLES
- SHOW MNODES
- SHOW QNODES
- SHOW SCORES
- SHOW STABLES
- SHOW STREAMS
- SHOW SUBSCRIPTIONS
- SHOW TABLES
- SHOW TABLE DISTRIBUTED
- SHOW TAGS
- SHOW TOPICS
- SHOW TRANSACTIONS
- SHOW USERS
- SHOW CLUSTER VARIABLES(3.0.1.6 之前为 SHOW VARIABLES)
- SHOW VGROUPS
- SHOW VNODES

### 权限管理

#### 创建用户
```sql
CREATE USER use_name PASS 'password' [SYSINFO {1|0}];
```

#### 查看用户
```sql
SHOW USERS;
```


#### 删除用户
```sql
DROP USER user_name;
```

#### 修改用户信息
```sql
ALTER USER user_name alter_user_clause
 
alter_user_clause: {
    PASS 'literal'
  | ENABLE value
  | SYSINFO value
}
```

#### 授权
```sql
GRANT privileges ON priv_level TO user_name
 
privileges : {
    ALL
  | priv_type [, priv_type] ...
}
 
priv_type : {
    READ
  | WRITE
}
 
priv_level : {
    dbname.*
  | *.*
}
```

#### 撤销授权
```sql
REVOKE privileges ON priv_level FROM user_name
 
privileges : {
    ALL
  | priv_type [, priv_type] ...
}
 
priv_type : {
    READ
  | WRITE
}
 
priv_level : {
    dbname.*
  | *.*
}

```

### 自定义函数
#### 创建 UDF
- 创建标量函数
```sql
CREATE FUNCTION function_name AS library_path OUTPUTTYPE output_type;
```
- 创建聚合函数
```sql
CREATE AGGREGATE FUNCTION function_name AS library_path OUTPUTTYPE output_type [ BUFSIZE buffer_size ];
```
#### 管理 UDF

- 删除自定义函数
```sql
DROP FUNCTION function_name;
```
- 删除的函数的名字
```sql
DROP FUNCTION bit_and;
```

- 显示系统中当前可用的所有 UDF
```sql
SHOW FUNCTIONS;
```


#### 调用 UDF
```sql
SELECT bit_and(c1,c2) FROM table;
```

### 异常恢复

#### 终止连接
```sql
KILL CONNECTION conn_id;
```
#### 终止查询
```sql
KILL QUERY kill_id;
```

#### 终止事务
```sql
RESET QUERY CACHE;
```

#### 重置客户端缓存
```sql
RESET QUERY CACHE;
```


## 运维

### 安装卸载

### 容量规划

### 容错灾备

### 数据导入

### 数据导出
#### 按表导出 CSV 文件
```sql
select * from <tb_name> >> data.csv;
```

#### 用 taosdump 导出数据
利用 taosdump，用户可以根据需要选择导出所有数据库、一个数据库或者数据库中的一张表，所有数据或一时间段的数据，甚至仅仅表的定义。

### 系统监控

## 第三方工具

### Kafka

TDengine Kafka Connector 包含两个插件: TDengine Source Connector 和 TDengine Sink Connector。用户只需提供简单的配置文件，就可以将 Kafka 中指定 topic 的数据（批量或实时）同步到 TDengine， 或将 TDengine 中指定数据库的数据（批量或实时）同步到 Kafka。

### EMQX
MQTT 是流行的物联网数据传输协议，EMQX是一开源的 MQTT Broker 软件，无需任何代码，只需要在 EMQX Dashboard 里使用“规则”做简单配置，即可将 MQTT 的数据直接写入 TDengine。EMQX 支持通过 发送到 Web 服务的方式保存数据到 TDengine，也在企业版上提供原生的 TDengine 驱动实现直接保存。


### Grafana
#### 概念
Grafana 是一款开源的数据可视化工具，使用 Grafana 可以非常轻松的将数据转成图表(如下图)的展现形式来做到数据监控以及数据统计。


### Google Data Studio

#### 概念
Google Data Studio 是一个强大的报表可视化工具，它提供了丰富的数据图表和数据连接，可以非常方便地按照既定模板生成报表。因其简便易用和生态丰富而在数据分析领域
广受青睐

#### 使用
TDengine 连接器已经发布到 Google Data Studio 应用商店，你可以在 “Connect to Data” 页面下直接搜索 TDengine，将其选作数据源。

### icinga2
icinga2 是一款开源主机、网络监控软件，最初由 Nagios 网络监控应用发展而来。目前，icinga2 遵从 GNU GPL v2 许可协议发行

## 应用实践

### Java连接TDEngine
#### JDBC方式

- 连接数据库

```java

    public static Connection getConn() throws Exception {
        final String url = "jdbc:TAOS://localhost:6030/?user=root&password=taosdata";
        try {
            Properties properties = new Properties();
            properties.setProperty("charset", "UTF-8");
            properties.setProperty("locale", "en_US.UTF-8");
            properties.setProperty("timezone", "UTC-8");
            System.out.println("get connection starting...");
            Connection connection = DriverManager.getConnection(url, properties);
            if (connection != null) {
                System.out.println("[ OK ] Connection established.");
            }
            return connection;
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }
```


- 创建数据库、创建表、插入数据等操作

```java

    private void createDatabase() {
        String sql = "create database if not exists " + dbName;
        exuete(sql);
    }

    private void useDatabase() {
        String sql = "use " + dbName;
        exuete(sql);
    }

    private void dropTable() {
        final String sql = "drop table if exists " + dbName + "." + tbName + "";
        exuete(sql);
    }

    private void createTable() {
        final String sql = "create table if not exists " + dbName + "." + tbName + " (ts timestamp, temperature float, humidity int)";
        exuete(sql);
    }

    private void insert() {
        final String sql = "insert into " + dbName + "." + tbName + " (ts, temperature, humidity) values(now, 20.5, 34)";
        exuete(sql);
    }

    private void select() {
        final String sql = "select * from " + dbName + "." + tbName;
        executeQuery(sql);
    }

    private void close() {
        try {
            if (connection != null) {
                this.connection.close();
                System.out.println("connection closed.");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }


    private void executeQuery(String sql) {
      long start = System.currentTimeMillis();
      try (Statement statement = connection.createStatement()) {
        ResultSet resultSet = statement.executeQuery(sql);
        long end = System.currentTimeMillis();
        printSql(sql, true, (end - start));
        printResult(resultSet);
      } catch (SQLException e) {
        long end = System.currentTimeMillis();
        printSql(sql, false, (end - start));
        e.printStackTrace();
      }
    }

    private void printResult(ResultSet resultSet) throws SQLException {
      ResultSetMetaData metaData = resultSet.getMetaData();
      while (resultSet.next()) {
        for (int i = 1; i <= metaData.getColumnCount(); i++) {
        String columnLabel = metaData.getColumnLabel(i);
        String value = resultSet.getString(i);
        System.out.printf("%s: %s\t", columnLabel, value);
        }
        System.out.println();
      }
    }

    private void printSql(String sql, boolean succeed, long cost) {
      System.out.println("[ " + (succeed ? "OK" : "ERROR!") + " ] time cost: " + cost + " ms, execute statement ====> " + sql);
    }

    private void exuete(String sql) {
      long start = System.currentTimeMillis();
      try (Statement statement = connection.createStatement()) {
        boolean execute = statement.execute(sql);
        long end = System.currentTimeMillis();
        printSql(sql, true, (end - start));
      } catch (SQLException e) {
        long end = System.currentTimeMillis();
        printSql(sql, false, (end - start));
        e.printStackTrace();
      }
    }

    private static void printHelp() {
      System.out.println("Usage: java -jar JDBCDemo.jar -host <hostname>");
      System.exit(0);
    }

```

#### 整合SpringBoot

##### 添加依赖
```pom

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>com.taosdata.jdbc</groupId>
            <artifactId>taos-jdbcdriver</artifactId>
            <version>3.0.0</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.17</version>
        </dependency>
    </dependencies>
```

##### 配置文件

```properties
# datasource config - JDBC-RESTful
spring.datasource.driver-class-name=com.taosdata.jdbc.rs.RestfulDriver
spring.datasource.url=jdbc:TAOS-RS://localhost:6041/test?timezone=UTC-8&charset=UTF-8&locale=en_US.UTF-8
spring.datasource.username=root
spring.datasource.password=taosdata
spring.datasource.druid.initial-size=5
spring.datasource.druid.min-idle=5
spring.datasource.druid.max-active=5
spring.datasource.druid.max-wait=30000
spring.datasource.druid.validation-query=select server_status();
spring.aop.auto=true
spring.aop.proxy-target-class=true
# mybatis
mybatis.mapper-locations=classpath:mapper/*.xml
logging.level.com.taosdata.jdbc.springbootdemo.dao=debug

```

##### 实体类
```java
@Data
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class Weather {

    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss.SSS", timezone = "GMT+8")
    private Timestamp ts;
    private Float temperature;
    private Float humidity;
    private String location;
    private String note;
    private byte[] bytes;
    private int groupId;
}
```

##### 启动类
```java
@MapperScan(basePackages = {"com.taosdata.example.springbootdemo"})
@SpringBootApplication
public class SpringbootdemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootdemoApplication.class, args);
    }
}

```

##### Dao接口
```java
public interface WeatherMapper {

    Map<String, Object> lastOne();

    void dropDB();

    void createDB();

    void createSuperTable();

    void createTable(Weather weather);

    List<Weather> select(@Param("limit") Long limit, @Param("offset") Long offset);

    int insert(Weather weather);

    int count();

    List<String> getSubTables();

    List<Weather> avg();

}

```

##### Mapper
````xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.taosdata.example.springbootdemo.dao.WeatherMapper">

    <resultMap id="BaseResultMap" type="com.taosdata.example.springbootdemo.domain.Weather">
        <id column="ts" jdbcType="TIMESTAMP" property="ts"/>
        <result column="temperature" jdbcType="FLOAT" property="temperature"/>
        <result column="humidity" jdbcType="FLOAT" property="humidity"/>
        <result column="bytes" jdbcType="BINARY" property="bytes" />
    </resultMap>

    <select id="lastOne" resultType="java.util.Map">
        select last_row(ts) as ts, last_row(temperature) as temperature, last_row(humidity) as humidity, last_row(note) as note,last_row(location) as location , last_row(groupid) as groupid from test.weather;
    </select>

    <update id="dropDB">
        drop
        database if exists test
    </update>

    <update id="createDB">
        create
        database if not exists test
    </update>

    <update id="createSuperTable">
        create table if not exists test.weather
        (
            ts
            timestamp,
            temperature
            float,
            humidity
            float,
            note
            binary
        (
            64
        ),
            bytes
            binary
        (
            64
        )) tags
        (
            location nchar
        (
            64
        ), groupId int)
    </update>

    <update id="createTable" parameterType="com.taosdata.example.springbootdemo.domain.Weather">
        create table if not exists test.t#{groupId} using test.weather tags
        (
            #{location},
            #{groupId}
        )
    </update>

    <select id="select" resultMap="BaseResultMap">
        select * from test.weather order by ts desc
        <if test="limit != null">
            limit #{limit,jdbcType=BIGINT}
        </if>
        <if test="offset != null">
            offset #{offset,jdbcType=BIGINT}
        </if>
    </select>

    <insert id="insert" parameterType="com.taosdata.example.springbootdemo.domain.Weather">
        insert into test.t#{groupId} (ts, temperature, humidity, note, bytes)
        values (#{ts}, ${temperature}, ${humidity}, #{note}, #{bytes})
    </insert>

    <select id="getSubTables" resultType="String">
        select tbname
        from test.weather
    </select>

    <select id="count" resultType="int">
        select count(*)
        from test.weather
    </select>

    <resultMap id="avgResultSet" type="com.taosdata.example.springbootdemo.domain.Weather">
        <id column="ts" jdbcType="TIMESTAMP" property="ts"/>
        <result column="avg(temperature)" jdbcType="FLOAT" property="temperature"/>
        <result column="avg(humidity)" jdbcType="FLOAT" property="humidity"/>
    </resultMap>

    <select id="avg" resultMap="avgResultSet">
        select avg(temperature), avg(humidity)
        from test.weather interval(1m)
    </select>
</mapper>
````

##### Service
```java

@Service
public class WeatherService {

    @Autowired
    private WeatherMapper weatherMapper;
    private Random random = new Random(System.currentTimeMillis());
    private String[] locations = {"北京", "上海", "广州", "深圳", "天津"};

    public int init() {
        weatherMapper.dropDB();
        weatherMapper.createDB();
        weatherMapper.createSuperTable();
        long ts = System.currentTimeMillis();
        long thirtySec = 1000 * 30;
        int count = 0;
        for (int i = 0; i < 20; i++) {
            Weather weather = new Weather(new Timestamp(ts + (thirtySec * i)), 30 * random.nextFloat(), random.nextInt(100));
            weather.setLocation(locations[random.nextInt(locations.length)]);
            weather.setGroupId(i % locations.length);
            weather.setNote("note-" + i);
            weather.setBytes(locations[random.nextInt(locations.length)].getBytes(StandardCharsets.UTF_8));
            weatherMapper.createTable(weather);
            count += weatherMapper.insert(weather);
        }
        return count;
    }

    public List<Weather> query(Long limit, Long offset) {
        return weatherMapper.select(limit, offset);
    }

    public int save(float temperature, float humidity) {
        Weather weather = new Weather();
        weather.setTemperature(temperature);
        weather.setHumidity(humidity);

        return weatherMapper.insert(weather);
    }

    public int count() {
        return weatherMapper.count();
    }

    public List<String> getSubTables() {
        return weatherMapper.getSubTables();
    }

    public List<Weather> avg() {
        return weatherMapper.avg();
    }

    public Weather lastOne() {
        Map<String, Object> result = weatherMapper.lastOne();

        long ts = (long) result.get("ts");
        float temperature = (float) result.get("temperature");
        float humidity = (float) result.get("humidity");
        String note = (String) result.get("note");
        int groupId = (int) result.get("groupid");
        String location = (String) result.get("location");

        Weather weather = new Weather(new Timestamp(ts), temperature, humidity);
        weather.setNote(note);
        weather.setGroupId(groupId);
        weather.setLocation(location);
        return weather;
    }
}

```

##### Controller
```java

@RequestMapping("/weather")
@RestController
public class WeatherController {

    @Autowired
    private WeatherService weatherService;

    @GetMapping("/lastOne")
    public Weather lastOne() {
        return weatherService.lastOne();
    }

    @GetMapping("/init")
    public int init() {
        return weatherService.init();
    }

    @GetMapping("/{limit}/{offset}")
    public List<Weather> queryWeather(@PathVariable Long limit, @PathVariable Long offset) {
        return weatherService.query(limit, offset);
    }

    @PostMapping("/{temperature}/{humidity}")
    public int saveWeather(@PathVariable float temperature, @PathVariable float humidity) {
        return weatherService.save(temperature, humidity);
    }

    @GetMapping("/count")
    public int count() {
        return weatherService.count();
    }

    @GetMapping("/subTables")
    public List<String> getSubTables() {
        return weatherService.getSubTables();
    }

    @GetMapping("/avg")
    public List<Weather> avg() {
        return weatherService.avg();
    }

}
```

## 常用操作

### 数据库命令
```sql
## 创建数据库
create database if not exists test;
    
show databases;
    
    
```
