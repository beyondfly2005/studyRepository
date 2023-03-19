# Flink 1.14.4最新版轻松入门熟练掌握

> https://www.bilibili.com/video/BV1K44y1g7wA
> 
>

# 1快速认识 fink
## 1.1.离线批计算与买时设式计算
批计算与流式计算，本质上是对有界流和无界流的计算

- 批计算
针对有界流：由于在产出计算结果前可以看到整个(完整)数据集合
- 流计算
针对无界流，由于用于无法看到输入数据的整体（）

## 1.2.Fink 基本概念

## 1.3.Flink 的运行时架构
## 1.4.Flink 的持性

#  2.FInk 编程基础api
## 2.1.震要引入的基本依赖
## 2.2.fink 的DataStream 抽象
## 2.3,fnk 编程模极
## 2.4.Mnk 程序的并行概念
## 2.5.fnk 编程入口

## 2.6 慧本source算子
### 2.6.1基于集合的 Source (测试用)
### 2.6.2基于 Socket 的 Source (测试用)
### 2.6.3.基于文件的Source
### 2.6.4.Kafka Source (生产常用)
### 2.6.5.白定义 Source

## 2.7.慧础 transformation 算子
### 2.7.1.脱射算子
### 2.7.2.过滤算子
### 2.7.3.分组算子
### 2.7.4.消动服合链子

## 2.8慧本sink 算子
### 2.8.1.打0的出 print
### 2.8.2.确出到文件
### 2.8.3.StreamFileSink
### 2.8.4.JdbcSink
### 2.8.5.KafkaSink
### 2.8.6.RediSink


## 2.9.task 与算子链
## 2.10.分区 partition算子v 

# 3.fink 多流操作 api
## 3.1.split 分被操作(巴 deprecated
## 3.2.分资操作[使用侧晚输出]
## 3.3.connect 连接摄作
## 3.4.broadcast *播
## 3.5.union 合并操作
## 3.6.join 关联操作
## 3.7.coGroup 协网分组
## 3.8.sideOutput 倒流输出v

# 4.FInk 编程 process function
## 4.1.process function 概述
## 4.2.ProcessFunction
## 4.3.Keyed ProcessFunction
## 4.4.ProcessWindowfunction
## 4.5.ProcessAITWindowFunction
## 4.6.CoProcessFuntion
## 4.7.ProcessJoinFunction
## 4.8.BroadcastProcessFunction
## 4.9.Kcyed BroadcastProcessFunction

# 5.Fnk时间语交
## 5.1.处理时间 (processing time) 语义
## 5.2.事件时间 (event ime) 语义
## 5.3财的还业的沿雪
