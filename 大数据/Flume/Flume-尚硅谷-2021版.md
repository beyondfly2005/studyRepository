# Flume-尚硅谷 2021版

> https://www.bilibili.com/video/BV1wf4y1G7EQ

## 课程介绍
课程内容 
- 版本升级 1.7升级到1.9
- 组件及架构介绍
- 企业常用组件及案例演示
- 监控工具的使用
- 源码阅读及修改源码

适用人员
- 从事数据采集工作
- 数据工程师从0到1

前置知识
- Java基础
- Hadoop基础

## 学习任务
大数据：海量数据的采集、存储、计算、工具
工具类框架
- Askaban 类似Cron 定时任务
- Consible不能做逻辑调度 B任务依赖于A任务的调度结果
计算
- Hive
- Flink
- Spark
采集
- Flume
- DataX
存储
- HDFS

内容
- 第1章 Flume概述
- 第2章 Flume入门
- 第3章 Flume进阶
- 第4章 Flume面试题

## 第1章 Flume概述

### 1.1 Flume定义

> 官网： flume.apache.org

 定义：Flume是Cloudera提供的一个高可用，高可靠的， 分布式的海量日志采集，聚合和传输的系统，Flume基于流式架构，灵活简单
 
- 日志采集 ，采取 PDF文件 音视频数据 会报错
- 流式计算 动态采集 ，日志有变化 ，Flume动态采集

Python爬虫数据
Java后台日志数据   服务器本地磁盘文件夹  采集到HDFS 或者Kafka
如果没有实时业务 ，可以不使用Flume 直接使用文件put上传


### 1.2 Flume基础架构
Flume组成架构
![Flume组成架构图](https://flume.apache.org/_images/DevGuide_image00.png "Flume架构图")
![Flume组成架构图](https://img2.baidu.com/it/u=109148212,1315003415&fm=253&fmt=auto&app=138&f=PNG?w=1149&h=500 "Flume架构图")
#### 1.2.1 Agent
Agent是一个JVM进程，他以实践的形式将数据从源头送至目的地
Agent主要有三个部分组成Source Channel Sink

#### 1.2.2 Source
Source是负责接收数据到Flume Agent的组件，Source组件可以处理各种类型、各种格式的日志数据，包括avro \thrift、exec、

#### 1.2.3 Sink
Sink不断地轮询Channel中的事件且批量移除他们，并将这些事件

#### 1.2.3 Channel1
Channel是位于Source和Sink之间的缓冲区，
Flume自带两种Channel：Memory Channel 和File Channel，还可以使用Kafka

#### 1.2.3 Event
传输单元，数据传输的基本单元，以Event的形式将数据从源头送至目的地
Event有Header和Body两部分组成，Header用来存放event的一些属性 k-v结构

## 第2章 Flume入门
### 2.1 Flume安装部署
#### 2.1.1 安装地址
 Flume官网地址  http://flume.apache.org
 Flume文档地址  http://flume.apache.org.FlumeUserGuide.html
 下载地址  http://archive.apache.org/dist/flume/

### 2.2 安装部署
```bash

```