### 01 大数据启蒙

- 启蒙很重要





##### 分治思想

分而治之的思想

Redis集群

ESearch



O(4)





###### 单机处理大数据问题

如果有一个非常大的文本文件（1T），里面有很多行，只有两行一样

他们出现在未知的位置，需要查找他们

单机，而言可用的内存很少，也就几十兆





归并算法







集群分布式处理





##### 结论

- 分而治之
- 并行计算

- 计算向数据移动
- 数据本地化读取
- 



Hadoop







Hadoop



Hbase

Hive

Spark

Zookeeper



#### 大数据生态







批量计算-> 流计算

Spark

Flink



HDFS（Distributed File System）





#### 02 hadoop-hdfs





存储模型

架构设计

角色功能

元数据持久化

安全模式



#### 存储模型

- 文件线性按自己切割成块(block) 具有offset，id
- 文件与文件的block大小可以不一样
- 一个文件除最后一个block块，其他block大小一致
- block的大小依据硬件的IO特性调整 
- block被分数存放在集群的节点中，具有location
- Block具有副本(replication)，没有主从概念，副本补不能同时出现在同一节点
- 副本是满足可靠性和性能的关键
- 文件上传可以指定block大小和副本数，上传后只能修改副本数
- 异常写入多次读取，不支持修改
- 支持追加数据

#### 分布式文件系统那么多

为什么Hadoop还要开发HSFS系统





#### 架构设计

- HDFS是一个主从架构
- 由一个NameNode和一些DataNode组成