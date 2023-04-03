# ElasticSearch



> ElasticStack技术栈 https://www.bilibili.com/video/BV1qr4y1w7hJ



### 第一章 ElasticSearch入门

### 1、Elasticsearch简介

#### 1.1 Elasticsearch简介

##### 1.1.1 介绍

- Elasticsearch是一个机遇Lucene的搜索服务器
- 提供了一个分布式多用户能力的全文搜索引擎，机遇RESTful web接口
- Elasticsearch是java语言开发，并作为Apache许可条款下的开放源码发布，是一种流行的企业级搜索引擎。Elasticsearch用于云计算中，能够达到实时搜索，稳定 可靠 快速 安装使用方便，官方客户端在Java .net（C#） PHP Python Groovy Ruby和许多其他语言中都是可以使用的
- 根据DB-Engines的排名显示，Elasticsearch是最廋欢迎的企业搜索引擎，其次是Apache Solr 也是基于Lucene

##### 1.2.2 创始人

Shay Banon 谢巴农



#### 1.2 Elasticsearch可以做什么

##### 1.2.1 信息检索

- 搜索引擎 百度

- 商品检索 淘宝
- 职位搜索 51job
- 论坛内搜索 CSDN
- 电商/ 招聘/门户/论坛

##### 1.2.2 企业内部系统搜索

- 关系型数据库使用like进行模糊检索，会导致索引失效，效率低下
- 可以使用Elasticsearch来进行检索，效率很高
- OA /CRM / ERP

##### 1.2.3 数据分析引擎

Elasticsearch聚合可以对数十亿行日志进行聚合分析，探索数据的趋势和规律

#### 1.3 Elasticsearch的特点

##### 1.3.1 海量数据处理

- 大型分布式集群 数百台规模服务器
- 处理PB即数据
- 小公司也可以进行单机部署

##### 1.3.2 开箱即用

- 简单易用 ，操作非常简单
- 快速部署生产环境

##### 1.3.3 作为传统数据库的补充

- 传统关系型数据库不擅长全文检索 Mysql自带全文索引，与ES性能差距非常大
- 传统关系型数据库无法支撑搜索排名，海量数据存储，分析等功能
- Elasticsearch可以作为传统数据库的补充，提供RDBM无法提供的功能

#### 1.4 哪些公司在使用Elasticsearch

京东 携程 去哪儿 58同城 滴滴 今日头条 小米 哔哩哔哩 联想 思科

Airbus ebay 暴雪 德国大众 微软 Symantec FaceBook BBC 英伟达 Uber

IBM Github  Docker

#### 1.5 Elasticsearch 使用案例

- 2013年初GitHub抛弃了Solr，采取Elasticsearch来做PB级的搜索，包括13亿文件和1300亿行代码”。
- 维基百科：启动Elasticsearch为基础的核心搜索架构
- SoudCloud:
-  百度目前广泛使用ElasticSearch作为文本数据分析，采集百度所有服务器上的各类指标数据及用户自定义数据，通过对各种数据进行多维分析展示，辅助定位分析实例异常或业务层面异常。目前覆盖百度内部20多个业务线（包括casio、云分析、网盟、预测、文库、直达号、钱包、风控等），单集群最大100台机器，200个ES节点，每天导入30TB+数据。
- 新浪适应ES分析处理32亿条实时日志
- 阿里使用ES构建挖财自己的日志采集和分析题型

#### 1.6 ElasticSearche对比Solr

- Solr 利用Zookeeper进行分布式管理，而Elasticsearch自带分布式协调管理功能
- Solr 支持更多格式的数据，而Elasticsearch仅支持json文件格式
- Solr 官方提供的功能更多，而Elasticsearch本身更注重于核心功能，高级功能多由第三方插件提供
- Solr 在传统的搜索应用中表现好于Elasticsearch，但是处理实时搜索应用时效率明显低于Elasticsearch。



#### 1.7 发展历史

2004年，发布第一个版本为Compass的搜索引擎，创建搜索引擎的目的是为了搜索食谱

2010年，发布第二个版本更名为Elasticsearch，基于Apache Lucene开发并开源

2015年 Elasticsearch公司更名为Elastic,专门

ELK不再包括Elastic公司所有的开源项目，ELK开始更名为



ElasticStack（EKL）现在的组件越来越多





### 2、Lucene全文检索库

#### 2.1 什么是全文检索

2.1.1

##### 2.1.2 搜索结构化数据和非结构化数据

- 使用ES/

##### 2.1.3 全文检索

- 通过一个过程扫描文本中的每一个单词，针对单词建立索引，并保存该单词在文本中的位置，以及出现的次数；
- 用户查询是，通过之前建立好的索引来查询，将所有中的单词对应的文本位置，出现的次数返回给用户，因为有了具体文本的位置，索引可以将具体内容读取出来了
- 类似于通过字典中的检索字表

#### 2.2 Lucene简介

Lucene 是Apache的一个顶级开源项目，是一个全文检索工具包，但Luncene不是一个完整的全文检索引擎，它只是提供了一个基本的全文检索的架构，还提供了一些基本的文本分词库

- Lucene是一个简单易用的工具包，可以方便的

#### 2.3 美文检索数据

##### 2.3.1 需求

要实现以上需求，我们有以下两种办法：

1



