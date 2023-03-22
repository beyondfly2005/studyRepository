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
	- 进入证券
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

## 集群部署

### 手动部署

### K8S

### Helm

## 开发使用

### 建立连接

### 数据建模

### 吸入数据

- 想法

### 查询数据

### 流式计算

### 数据订阅

### 缓存

### 自定义函数

## 连接器

### 子主题 1

## SQL基础

### 数据类型

- 时间戳
- 数据类型

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

#### 创建超级表
#### 查看超级表
#### 删除超级表
#### 修改超级表


### 数据写入

#### 语法
#### 插入一条记录

#### 插入多条记录

#### 自定列插入

#### 插入记录时自动建表

#### 插入来自文件的数据记录

#### 插入来自文件的记录并自定建表



### 数据查询

#### 查询语法

#### 列表

#### 查询对象

#### GROUP BY
#### LIMIT

#### SLMIT

#### 特色工程

#### 正则

#### CASE

#### JOIN 字句

#### 嵌套查询

#### UNION ALL字句

#### SQL示例

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
CAST
TO_ISO8601
TO_JSON
TO_UNIXTIMESTAMP
##### 时间和日期函数
NOW
TIMEDIFF
TIMETRUNCATE
TIMEZONE
TODAY
##### 聚合函数
APERCENTILE
AVG
COUNT
ELAPSED
LEASTSQUARES
SPREAD
STDDEV
SUM
HYPERLOGLOG
HISTOGRAM
PERCENTILE
##### 选择函数
BOTTOM
FIRST
INTERP
LAST
LAST_ROW
MAX
MIN
MODE
SAMPLE
TAIL
TOP
UNIQUE
##### 时序数据特有函数
CSUM
DERIVATIVE
DIFF
IRATE
MAVG
STATECOUNT
STATEDURATION
TWA
##### 系统信息函数
DATABASE
CLIENT_VERSION
SERVER_VERSION
SERVER_STATUS

### 特色查询

#### 数据切分查询
#### 窗口切分查询
- 窗口子句的规则
- FILL 子句
- 时间窗口
- 状态窗口
- 会话窗口
- 事件窗口
- 时间戳伪列

### 数据订阅

创建订阅主题
删除订阅主题
查看订阅主题
SHOW TOPICS
创建消费组
删除消费组
查看消费组

### 流式计算
创建流式计算
流式计算的 partition
流式计算读取历史数据
删除流式计算
展示流式计算
流式计算的触发模式
流式计算的窗口关闭
流式计算的过期数据处理策略

### 运算符
算术运算符
位运算符
JSON 运算符
集合运算符
比较运算符
逻辑运算符
### JSON类型
语法说明
支持的操作
其他约束条件

### 转义字符

转义字符表
转义字符使用规则

### 命名与边界

命名规则
密码合法字符集
一般限制

### 保留关键字
保留关键字
### 集群管理

创建数据节点
查看数据节点
删除数据节点
修改数据节点配置
添加管理节点
查看管理节点
删除管理节点
创建查询节点
查看查询节点
删除查询节点
查询集群状态
修改客户端配置
查看客户端配置

### 元数据
#### 元数据
TDengine 内置了一个名为 INFORMATION_SCHEMA 的数据库，提供对数据库元数据、数据库系统信息和状态的访问，例如数据库或表的名称，当前执行的 SQL 语句等。该数据库存储有关 TDengine 维护的所有其他数据库的信息。它包含多个只读表。实际上，这些表都是视图，而不是基表，因此没有与它们关联的文件。所以对这些表只能查询，不能进行 INSERT 等写入操作。
#### 内置元数据
INS_DNODES
INS_MNODES
INS_QNODES
INS_CLUSTER
INS_DATABASES
INS_FUNCTIONS
INS_INDEXES
INS_STABLES
INS_TABLES
INS_TAGS
INS_COLUMNS
INS_USERS
INS_GRANTS
INS_VGROUPS
INS_CONFIGS
INS_DNODE_VARIABLES
INS_TOPICS
INS_SUBSCRIPTIONS
INS_STREAMS

### 统计数据

PERF_APP
PERF_CONNECTIONS
PERF_QUERIES
PERF_CONSUMERS
PERF_TRANS
PERF_SMAS

### SHOW命令

SHOW APPS
SHOW CLUSTER
SHOW CONNECTIONS
SHOW CONSUMERS
SHOW CREATE DATABASE
SHOW CREATE STABLE
SHOW CREATE TABLE
SHOW DATABASES
SHOW DNODES
SHOW FUNCTIONS
SHOW LICENCES
SHOW INDEXES
SHOW LOCAL VARIABLES
SHOW MNODES
SHOW QNODES
SHOW SCORES
SHOW STABLES
SHOW STREAMS
SHOW SUBSCRIPTIONS
SHOW TABLES
SHOW TABLE DISTRIBUTED
SHOW TAGS
SHOW TOPICS
SHOW TRANSACTIONS
SHOW USERS
SHOW CLUSTER VARIABLES(3.0.1.6 之前为 SHOW VARIABLES)
SHOW VGROUPS
SHOW VNODES

### 权限管理

### 自定义函数

### 异常恢复

终止连接
终止查询
终止事务
重置客户端缓存

### 语义变更

## 运维

### 安装卸载

### 容量规划

### 容错灾备

### 数据导入

### 数据导出

### 系统监控

## 第三方工具

### Kafka

### EMQX

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



## 常用操作

### 数据库命令
```sql
## 创建数据库
create database if not exists test;
    
show databases;
    
    
```

*XMind: ZEN - Trial Version*
