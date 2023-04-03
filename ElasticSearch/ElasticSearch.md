# ElasticSearch


> Elasticsearch学习
> https://blog.51cto.com/elasticsearch/5768421
> 

> 用Elasticsearch构建电商搜索平台，一个极有代表性的基础技术架构和算法实践案例
> 
> https://blog.51cto.com/u_15486212/5239188


> Elasticsearch分片、路由、数据写入过程
> https://blog.csdn.net/weixin_38405646/article/details/121184254


## 1. ES 基础
#### 1.1 ES定义
ES是ElasticSearch简写， Elasticsearch是一个开源的高扩展的分布式全文检索引擎，它可以近乎实时的存储、检索数据；本身扩展性很好，可以扩展到上百台服务器，处理PB级别的数据。
Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

#### 1.2 Lucene与ES关系
1）Lucene只是一个库。想要使用它，你必须使用Java来作为开发语言并将其直接集成到你的应用中，更糟糕的是，Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。

2）Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

#### 1.3 ES主要解决问题：
1）检索相关数据；

2）返回统计结果；

3）速度要快。

#### 1.4 ES工作原理

当ElasticSearch的节点启动后，它会利用多播(multicast)(或者单播，如果用户更改了配置)寻找集群中的其它节点，并与之建立连接。这个过程如下图所示：



#### 1.5 ES核心概念
- 1）Cluster：集群。
ES可以作为一个独立的单个搜索服务器。不过，为了处理大型数据集，实现容错和高可用性，ES可以运行在许多互相合作的服务器上。这些服务器的集合称为集群。

- 2）Node：节点。
形成集群的每个服务器称为节点。

- 3）Shard：分片。
当有大量的文档时，由于内存的限制、磁盘处理能力不足、无法足够快的响应客户端的请求等，一个节点可能不够。这种情况下，数据可以分为较小的分片。每个分片放到不同的服务器上。
当你查询的索引分布在多个分片上时，ES会把查询发送给每个相关的分片，并将结果组合在一起，而应用程序并不知道分片的存在。即：这个过程对用户来说是透明的。

- 4）Replia：副本。
为提高查询吞吐量或实现高可用性，可以使用分片副本。
副本是一个分片的精确复制，每个分片可以有零个或多个副本。ES中可以有许多相同的分片，其中之一被选择更改索引操作，这种特殊的分片称为主分片。
当主分片丢失时，如：该分片所在的数据不可用时，集群将副本提升为新的主分片。

- 5）全文检索。
全文检索就是对一篇文章进行索引，可以根据关键字搜索，类似于mysql里的like语句。
全文索引就是把内容根据词的意义进行分词，然后分别创建索引，例如”你们的激情是因为什么事情来的” 可能会被分词成：“你们“，”激情“，“什么事情“，”来“ 等token，这样当你搜索“你们” 或者 “激情” 都会把这句搜出来。

#### 1.6 ES数据架构的主要概念（与关系数据库Mysql对比）

（1）关系型数据库中的数据库（DataBase），等价于ES中的索引（Index）

（2）一个数据库下面有N张表（Table），等价于1个索引Index下面有N多类型（Type），

（3）一个数据库表（Table）下的数据由多行（ROW）多列（column，属性）组成，等价于1个Type由多个文档（Document）和多Field组成。

（4）在一个关系型数据库里面，schema定义了表、每个表的字段，还有表和字段之间的关系。 与之对应的，在ES中：Mapping定义索引下的Type的字段处理规则，即索引如何建立、索引类型、是否保存原始索引JSON文档、是否压缩原始JSON文档、是否需要分词处理、如何进行分词处理等。

（5）在数据库中的增insert、删delete、改update、查search操作等价于ES中的增PUT/POST、删Delete、改_update、查GET.

#### 1.7 ELK是什么？
ELK=elasticsearch+Logstash+kibana
elasticsearch：后台分布式存储以及全文检索
logstash: 日志加工、“搬运工”
kibana：数据可视化展示。
ELK架构为数据分布式存储、可视化查询和日志解析创建了一个功能强大的管理链。 三者相互配合，取长补短，共同完成分布式大数据处理工作。

## 2. ES特点和优势
   1）分布式实时文件存储，可将每一个字段存入索引，使其可以被检索到。
   2）实时分析的分布式搜索引擎。
   分布式：索引分拆成多个分片，每个分片可有零个或多个副本。集群中的每个数据节点都可承载一个或多个分片，并且协调和处理各种操作；
   负载再平衡和路由在大多数情况下自动完成。
   3）可以扩展到上百台服务器，处理PB级别的结构化或非结构化数据。也可以运行在单台PC上（已测试）
   4）支持插件机制，分词插件、同步插件、Hadoop插件、可视化插件等。

##  3、ES性能
#### 3.1 性能结果展示
（1）硬件配置：
CPU 16核 AuthenticAMD
内存 总量：32GB
硬盘 总量：500GB 非SSD

（2）在上述硬件指标的基础上测试性能如下：
1）平均索引吞吐量： 12307docs/s（每个文档大小：40B/docs）
2）平均CPU使用率： 887.7%（16核，平均每核：55.48%）
3）构建索引大小： 3.30111 GB
4）总写入量： 20.2123 GB
5）测试总耗时： 28m 54s.

#### 3.2 性能esrally工具（推荐）
使用参考

## 4、为什么要用ES？
#### 4.1 ES国内外使用优秀案例
1） 2013年初，GitHub抛弃了Solr，采取ElasticSearch 来做PB级的搜索。 “GitHub使用ElasticSearch搜索20TB的数据，包括13亿文件和1300亿行代码”。
2）维基百科：启动以elasticsearch为基础的核心搜索架构。
3）SoundCloud：“SoundCloud使用ElasticSearch为1.8亿用户提供即时而精准的音乐搜索服务”。
4）百度：百度目前广泛使用ElasticSearch作为文本数据分析，采集百度所有服务器上的各类指标数据及用户自定义数据，通过对各种数据进行多维分析展示，辅助定位分析实例异常或业务层面异常。目前覆盖百度内部20多个业务线（包括casio、云分析、网盟、预测、文库、直达号、钱包、风控等），单集群最大100台机器，200个ES节点，每天导入30TB+数据。

#### 4.2 我们也需要
实际项目开发实战中，几乎每个系统都会有一个搜索的功能，当搜索做到一定程度时，维护和扩展起来难度就会慢慢变大，所以很多公司都会把搜索单独独立出一个模块，用ElasticSearch等来实现。

近年ElasticSearch发展迅猛，已经超越了其最初的纯搜索引擎的角色，现在已经增加了数据聚合分析（aggregation）和可视化的特性，如果你有数百万的文档需要通过关键词进行定位时，ElasticSearch肯定是最佳选择。当然，如果你的文档是JSON的，你也可以把ElasticSearch当作一种“NoSQL数据库”， 应用ElasticSearch数据聚合分析（aggregation）的特性，针对数据进行多维度的分析。

【知乎：热酷架构师潘飞】ES在某些场景下替代传统DB

个人以为Elasticsearch作为内部存储来说还是不错的，效率也基本能够满足，在某些方面替代传统DB也是可以的，前提是你的业务不对操作的事性务有特殊要求；而权限管理也不用那么细，因为ES的权限这块还不完善。
由于我们对ES的应用场景仅仅是在于对某段时间内的数据聚合操作，没有大量的单文档请求（比如通过userid来找到一个用户的文档，类似于NoSQL的应用场景），所以能否替代NoSQL还需要各位自己的测试。
如果让我选择的话，我会尝试使用ES来替代传统的NoSQL，因为它的横向扩展机制太方便了。

## 5. ES的应用场景是怎样的？
### 5.1 通常我们面临问题有两个：
   1）新系统开发尝试使用ES作为存储和检索服务器；
   2）现有系统升级需要支持全文检索服务，需要使用ES。
   以上两种架构的使用，以下链接进行详细阐述。​

### 5.2 一线公司ES使用场景：
1）新浪ES 如何分析处理32亿条实时日志 http://dockone.io/article/505
2）阿里ES 构建挖财自己的日志采集和分析体系 http://afoo.me/columns/tec/logging-platform-spec.html
3）有赞ES 业务日志处理 http://tech.youzan.com/you-zan-tong-ri-zhi-ping-tai-chu-tan/
4）ES实现站内搜索 http://www.wtoutiao.com/p/13bkqiZ.html

## 6. 如何部署ES？
### 6.1 ES部署（无需安装）
   1）零配置，开箱即用
   2）没有繁琐的安装配置
   3）java版本要求：最低1.7
   我使用的1.8
   [root@laoyang config_lhy]# echo $JAVA_HOME
   /opt/jdk1.8.0_91
   4）下载地址：
   ​​ ​ https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/zip/elasticsearch/2.3.5/elasticsearch-2.3.5.zip​​​ 5）启动
   cd /usr/local/elasticsearch-2.3.5
   ./bin/elasticsearch
   bin/elasticsearch -d(后台运行)

### 6.2 ES必要的插件
必要的Head、kibana、IK（中文分词）、graph等插件的详细安装和使用。​

### 6.3 ES windows下一键安装
自写bat脚本实现windows下一键安装。
1）一键安装ES及必要插件（head、kibana、IK、logstash等）
2）安装后以服务形式运行ES。
3）比自己摸索安装节省至少2小时时间，效率非常高。
脚本说明

## 7. ES对外接口（开发人员关注）
  1）JAVA API接口
   ​ ​http://www.ibm.com/developerworks/library/j-use-elasticsearch-java-apps/index.html​​

  2）RESTful API接口
常见的增、删、改、查操作实现

## 8.ES遇到问题怎么办？
1）国外：https://discuss.elastic.co/
2）国内：http://elasticsearch.cn/






# ES面试题
> http://qcloud.xihaba.cn/?article/1082081
> 
> 
> 

> https://blog.51cto.com/elasticsearch/5768421
