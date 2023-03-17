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



