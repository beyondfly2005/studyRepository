# Flink

> Flink教程 尚硅谷
> https://www.bilibili.com/video/BV133411s7Sa/


## P1 Flink课程简介

#### Flink的特点
- 高吞吐量
低延迟
- 结果正确



#### 课程内容
- 基础篇
  - 

- 核心篇
  - DataStream API
  - 

- 高阶篇
  - 7 处理函数
  - 8 多流转换
- 扩展篇
  - 12 Flink ZEP


#### 课程特色
- 基于Flink 1.13 版本
- 基于Java语言
- 大量的代码实现
- 电商应用场景案例
- 教材：剑指大数据 摘要

资料获取方式
- B栈免费


课程基础要求
- 熟悉Java语言
- 熟悉Linux常用命令
- 熟悉Idea开发工具

## P2 Flink流式处理简介
> 初识Flink
> 
> 
对标Spark 处理框架

区别
- Spark 做批处理
- Flink做流处理

主要内容
- Flink是什么
- 为什么要用Flink
- 流处理的发展和演变
- Flink的主要特点
- Flink vs Spark Streaming


#### Flink是什么
Apache Flink is aframework and distributed processing engin for stateful computions over
Apache Flin世界框架和分布式处理引擎，用于对无解和有界数据流进行状态计算

#### Flink发展时间线
- 14年 第一个不能把0.6发布 ,核心开发者创建DataArtisans公司
- 14年12月 完成孵化
- 15年4月 发布里程碑式的版本0.9.0
- 19年1月 阿里巴巴收购Data Artisans公司
- 19年8月 阿里巴巴将内部Blink开源 合并如Flink1.9.0版本

扩展
- 小松鼠 快速并且灵活的大数据流处理 ，快速响应
- Flink德语 表示快速灵巧

官网地址
> http://flink.apache.org
> 数据量上的有状态计算

#### Flink框架处理流程
![Flink框架处理流程](https://flink.apache.org/img/flink-home-graphic.png)
- 事件驱动型的应用
- 流处理的刘淑霞
- 流&批数据分析


#### Flink的应用场景
- 电商和市场营销
  - 实时报表
  - 广告投放
  - 实施推荐
- 物联网
  - 实时数据采集
  - 实时报警
- 物流配送及服务
  - 钉钉状态跟踪
  - 信息推送
- 银行和金融业
  - 实时结算
  - 风险检测
  - 交易行为风险检测

#### 为什么要用Flink
- 批处理和流处理
- 流数据更真实地反应了我们的生活方式
- 我们的目标
  - 低延迟
  - 高吞吐
  - 结果的准确性和良好的容错性


传统数据处理架构
- 事务处理 OLTP

- 分析处理 OLAP

Storm 第一代流式处理计算框架


#### 流处理的发展和演变
- lambda架构
  - 用两套系统，同时保证低延迟和结果准确
  - 缺点：同时维护批处理和流处理两套架构
##### 第三代 流处理器Flink
  
#### Flink的主要特点
Flink核心特点
- 高吞吐
- 结果的准确性
- 精确一次exactly-once的状态一致性保证

#### Flink的应用模型 应用场景
- 事件驱动型应用
  - 连接Kafka
- 数据分析型应用

- 数据管道型应用
  - ETL（提取-转换-加载） 
  - 数据管道与ETL作业向上，都可以转换数据 并将其存储系统移动到另一个
  - 区别：数据管道是以持续流模式运行，而非周期性触发，支持从一个不断生成数据的源头触发，并将其以低延迟移动到终点
  - 例如：监控新的日志文件 写入事件日志；将事件流物化到数据库或增量构建和优化查询索引。


#### 分层API
- SQL 最高层语言
- Table API声明式领域专用语言
- DataStream/DataSet API 核心APIs
- 有状态流处理 底层APIs

Flink分层特点：
- 越顶层越抽象，表达含有越简明，使用越方便
- 越底层越具体，表达能力越丰富，使用越灵活



#### Flink vs Spark Streaming
Spark主要提出了内存计算
Spark Streaming 流式计算
Spark 

数据处理架构：
Spark是基于批处理
Spark streaming也是基于批处理的
将数据流进行截取，批次执行，将流切分为 足够小的批
Flink 

有界数据  无界数据

#### 数据模型
- Spark采用RDD模型，spark streaming的DStream 实际上也就是一组组小批数据RDD的集合
- Flink基本数据模型师数据流，以及事件Event序列

#### 运行时架构
- Spark




## 第2章 Flink快速上手
同时支持Java和Scala API
使用Idea作为开发工具
使用Maven作为包管理工具
示例：分词 词频统计


### 2.1 环境准备
- windows10
- Java8
- IDEA
- Maven
- Git
- Flink版本 1.13.0

### 2.2 创建项目

项目名称: FlinkTutorial


##### 相关依赖
```shell
flink 1.13.0
日志管理
log4j 
sel4j  1.7.30
```

指定版本


scala.版本 2.12

flink-java
flink-streaming-java
flink-clients_${version}


```pom
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.beyond</groupId>
    <artifactId>flink-tutorial</artifactId>
    <version>1.0.0</version>
    <description>Flink-Demo 分词 </description>

    <properties>
        <java.version>1.8</java.version>
        <flink.version>1.13.0</flink.version>
        <scala.binary.version>2.12</scala.binary.version>
        <sl4j.version>1.7.30</sl4j.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-java</artifactId>
            <version>${flink.version}</version>
        </dependency>

        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-streaming-java_${scala.binary.version}</artifactId>
            <version>${flink.version}</version>
        </dependency>

        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-clients_${scala.binary.version}</artifactId>
            <version>${flink.version}</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${sl4j.version}</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${sl4j.version}</version>
        </dependency>

        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-to-slf4j</artifactId>
            <version>2.14.0</version>
        </dependency>
    </dependencies>
</project>
```

DataSet API 弃用

使用DataStream API 做批处理任务，需要在提交任务时，  运行时的提交模式改为 BATCH，


### 2.3.2 流处理
Data Stream API批流统一，

两种不同的流数据
- 批处理 有界流
- 流处理 无界流

并行度 默认是当前电脑的CPU的核心数 16核 1-16



## P14 Flink快速上手（三） 流处理（二） 无界流处理WordCount
使用netcat //linux 执行 nc命令 在7777 端口进行输出 
```bash
nc -lk 7777
```

##### 修改硬编码的主机名和端口号 为外部参数传入

```java
public static void main(String[]args){
  ParameterTool parameterTool = ParameterTool.fromArgs(args);
  String hostname = parameterTool.get("host");
  Integer port parameterTool.get("port");
  }
```
Idea 运行时, 配置Program arguments
```java
--host hadoop02 --port 7777
```


P15


## 第3章 Flink部署

开发环境中 由引入的jar包 模拟了一个集群环境

关键组件
- 客户端Client
- 作业管理器 JobManager
- 任务管理器 TaskManager


我们的代码由客户端进行获取和转换，


### 3.1 快速启动一个Flink集群
#### 3.1.1 环境配置
三台Linux机器
- CentOS7.5
- Java8
- 安装Hadoop集群 建议2.7.5  为了使用Yarn的调度 扩充TaskManager节点
- 节点1 192.168.10.102 名为hadoop102
- 节点2 192.168.10.103 名为hadoop103
- 节点3 192.168.10.104 名为hadoop104


### 3.1.2 本地启动
最简单的方式 不搭建集群  本地启动
flink-1.13.2-scale_2.12.tgz




flink-conf.yaml
```yaml
jobmanager.rpc.address: hadoop102
jobmanager.rpc.port: 6123

jobmanager.

taskmanager.numberOftaskSlots: 1 #任务槽 默认1
parallelism.default: 1 # 并行度1

## 高可用配置

## Fault 容错配置检查点

##
```
###### masters 
cat masters



###### workers
cat conf/workers

启动
```shell
cd flink-1.13.0
bin/start-cluster.sh

jps 

```
start-cluster
stop-cluster

### 集群部署
集群节点的角色
JobManager hadoop102
TaskManager hadoop103
TaskManager hadoop104


修改集群配置
- 修改conf/flink-conf.yaml文件 修改 jobmanager.rpc.address 为hadoop102
- 修改works文件 将另外两天节点服务器添加为当前Flink集群的TaskManager节点


web-ui
> http://hadoop102:8081/


##### TaskManagers
##### JobManager
##### Submit New Job 作业信息


#### 3、向集群提交作业
Add New  上传jar包

打包工具 maven-assembly-plugin
```pom
<build>
  <plugins>
    <plugin>
    <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-assembly-plugin</artifactId>
      <configuration>
          <!--   这个是assembly 所在位置；${basedir}是指项目的的根路径  -->
          <descriptors>
              descriptor>${basedir}/src/main/assembly/assembly.xml</descriptor>
          </descriptors>
          <!--打包解压后的目录名；${project.artifactId}是指：项目的artifactId-->
          <finalName>${project.artifactId}</finalName>
          <!-- 打包压缩包位置-->
          <outputDirectory>${project.build.directory}/release</outputDirectory>
          <!-- 打包编码 -->
          <encoding>UTF-8</encoding>
      </configuration>
      <executions>
        <execution><!-- 配置执行器 -->
            <id>make-assembly</id>
            <phase>package</phase><!-- 绑定到package生命周期阶段上 -->
            <goals>
                <goal>single</goal><!-- 只运行一次 --> 
            </goals>
        </execution>
    </executions>

    </plugin>
  </plugins>
</build
```

上传jar包

指定运行参数
- 指定入口类
- Parallelism: 并行度
- Program Arguments 运行参数
- Savepoint Path 保存点
- Show Plan 显示执行计划
  - 并行度 部分操作的并行度 是无法调高的 ，例如读取流的操作
- 提交任务

- 任务失败 查看log
  - 连接失败， nc 并未启动
  - 没有输出 nc中 需要输入 文本 
- 执行结果
  - JobManager Stdout 控制台
  - TaskManager Stdout 控制台 输出到 TaskManager 是Worker 真正执行的服务
- 停止任务
  - Cancel Job 按钮

#### 命令行 方式 进行任务提交

先上传jar包 ftp上传到hadoop102
```shell

# 在本机执行
./bin/flink run -c com.beyond.wordcount.StreamWordCount -p 2 ../FlinkTutorial-1.0-SNAPSHOT.jar
# 在其他服务器执行
./bin/flink run -m hadoop102:8081 -c com.beyond.wordcount.StreamWordCount -p 2 ../FlinkTutorial-1.0-SNAPSHOT.jar
```

取消任务 需要通过web页面 Cancel Job
如果再次提交 会提示 没有可用的资源，只有一个 

命令行 取消操作
```shell
./bin.flink cancel JOB-ID
```


### 3.2 部署模式
三种不同的模式
- 会话模式 Session Mod
- 单作业

#### 3.2.1 会话模式Session MOd
需要先启动一个集群，保持一个会话，在这个会话中通过客户端提交作业，

- 回话模式 比较适合于 打个规模比较小，执行时间段的大量作业

#### 3.2.2 单作业模式
会话因为资源共享会导致很多问题，索引为了更好的隔离资源，我们可以考虑为每个提交的作业启动一个集群

单作业模式



#### 3.2.3 应用模式Application Mode
前面两种模式下，应用代码都是在客户端上执行

跟单作业模式

作业与集群一对一
应用jar包 与集群一对一


### 3.3 独立模式 Standalone
独立模式 是部署Flink最基本也是最简单的方式，所需要的所有FLink组件 都只是操作系统上运行的JVM

#### 3.3.1 毁坏模式部署

#### 3.3.2 单作业模式部署
没有

#### 3.3.3 应用模式部署
 很少使用

````bash
./binstandalon-job.sh start --job-classname com.beyond.wordcount.StreamWordCount

./bin taskmanager.sh start

./bin task
````

### 3.4 YARN模式
 最常用的资源管理平台是YARN
YARN上部署的过是：客户段把Flink应用提交给Yarn的ResourceManager，Yarn的ResourceManage会项目NodeManager申请容器，在这些容器上,Flink
会部署JobManager 和TskManager


1.8之前，Flink部署在YARN上，需要Hadoop
1.8之后，Fink 需要自行下载

Flink安装目录的 lib目录下


1.11 之后 ，只需要配置环境变量


（3） 启动Hadoop集群

（4） 
```shell
./bin/yarn-session.
```

#### 3.4.1 相关准备和配置
#### 3.4.2 会话模式部署

从Flink 1.11.0版本之后 -n - 两个参数

./bin/flink run -c com.beyond.wc.Stream



#### 3.4.3 单作业模式的部署
在YARN环境中，由于有了外部平台做资源调度，所以我们可以使用直接诶想YARN 一个单独的作业，从而启动一个Flink
集群
（1）执行命令提交作业
樱花园模式同样


#### 3.4.4 


#### 3.5 K8S模式

容器化不是事如今业界


## Flink运行时架构
- 系统架构
![img.png](./images/system-architecture.png)
- 作业提交流程
- 


作业管理器JobManage
控制一个应用程序的主进程，是Flink集群中任务管理和调度的核心
- Jobmaster
  - JobMaster是JobManager最核心的组件，负责处理单独的作业Job
  - 在作业提交时，JobMaster会现接受到要执行应用，一般是由客户端提价来的，包括：Jar包，数据流图dataflow graph 和作用和图JobGraph
  - JobMaster 回吧JobGraph转换为换一个物理层面的数据流图，这个图呗叫做执行图
- 资源管理器ResourceManager
  - ResourceManager主要负责资源的分配和关联，在Flink集群中只有一个，所谓 资源 主要是指TaskManager的任务槽 task slots 。任务槽就是Flink集群中的资源调配单元，包含了机器用来执行计算的一组CPU和内存资源。每一个任务Task都要分配到一个slot上执行。
- 分发器Dispatcher


任务管理Taskmanaager
- Flink中的工作进程，通常自带Flink中会有多个TaskManger运行，每一个TaskManager都包含了一定数量的插槽
- 启动了


##### 作业提交流程
![img.png](./images/task-submint-flow.png)
概括的提交流程 
比较抽象的提交流程 ，不考虑提交模式的情况下

Standalone 模式作业提交流程
会话模式的提交流程

![img.png](./images/task-submint-flow-standalone.png)

YARN会话模式作业提交流程

![img.png](./images/submint-flow-yarn-session.png)

## 第5章 DataStream API（基础篇）

## 第6章 Flink中的事时间和窗口

## 第7章 处理函数

## 第8章 多流转换

## 第9章 状态编程

## 第10章 容错机制

## 第11章 TableAPI和 SQL

## 第12章 Flink CEP


