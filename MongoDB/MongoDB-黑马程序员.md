# MongoDB基础入门到高级进阶

> 课程地址：https://www.bilibili.com/video/BV1bJ411x7mq
>

MongoDB用起来 - 快速上手&集群和安全系列

# MongoDB快速上手



## 课程目标

- 理解MongoDB的业务场景，属性MongoDB的简介、特点和体系结构、数据类型
- 能够在Windows和Linux系按照和启动MongoDB，图形化管理工具Compass的安装和使用
- 掌握MongoDB基本常用命令实现数据库的CRUD
- 掌握MongoDB的索引类型、索引管理、执行计划
- 使用SpringData MongodDB完成文章评论业务的开发

## 1、MongoDB的相关概念

#### 1.1 业务应用场景

传递的关系型数据库（如MySQL）,在数据库操作的“三高”需求以及应对Web2.0的网站需求面前，显得力不从心。

解释：“三高”需求

- High performace - 对数据库高并发读写的需求。 
- Huge Storage - 对海量数据的高效率纯粹和访问的需求。
- High Scalability & High Availability - 对数据库的高扩展和高可用的需求

**MongoDB可用应对“三高”需求**



具体的应用场景如：

- 社交场景，使用MongoDB存储用户新，以及用户发表的朋友圈信息，通过地理位置索引 实现附近的人、地点等功能。

- 游戏场景，使用MongoDB存储游戏用户信息，用户的额装备，积分等直接以内嵌文档的形式存储，方便查询、高效存储和访问。

- 物流场景，使用MongoDB存储订单信息，订单状态在运送过程中会不断更新，以MongoDB内嵌数组的形式存储，一次查询就能将订单所有的变更读取出来。

- 视频直播，使用MonGoDB存储用户信息、点赞互动信息等。



在这些应用场景中，数据操作方面的公同特点是：

- 数据量大
- 写入操作频繁（读写都很频繁）
- 价值较低的数据、对事务性要求不高

对于这样的数据，我们更适合使用MongoDB来实现数据的存储。

##### 什么时候选择MongoDB？

在架构选型上，除了上述的三个特点外，如果你还犹豫是否要选择它？可以考虑一下的一些问题：

引用不需事务及负债的join支持

新应用，需求汇编，数据模型无法确定，响应快速迭代开发

应用需要2000-3000以上的QPS（更高也可以）

应需要TB甚至PB级别的数据存储

也能够有要存储的数据不丢失

一个月需要99.9999%高可用

也能够有需要大量的地理位置查询，文本查询

如果上述有1个符合，可用考虑MongoDB，2个以上的符合，选择MongoDB绝对不会后悔。

思考：如果用MySQL呢？

答：下不过对于MySQL，可以以更低的成本解决问题（包括学习、开发、运维等成本）



#### 1.2 MongDB简介

MongoDB是一个开源的、高性能、无模式的文档型数据库，是NoSQL数据库产品中最热门的一种。是最像关系型数据库（MySQL）的菲关系型数据库。

它支持的数据结构非常松散，是一种类似json的bson格式，所以他既可以存储比较复杂的数据类型，又相当的灵活。

MongoDB中的记录是一个文档，它是由一个由字段和键值对（field:value）组成的数据结构。MongoDB文档类似于JSON对象，即一个文档任务就是一个对象，字段的数据类型是字符型，它的值除了使用基本的而一些类型外，还可以包括其他文档，普通数组和文档数组。

#### 1.3 体系结构

MySQL和MongoDB对比

![img](https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180106141036237-258431155.png)

| **SQL 术语/概念** | **MongoDB 术语/概念** | **解释/说明** |
| ----------------- | --------------------- | ------------- |
| **database**      | database            |  数据库 |
| **table**         | collection          |数据库表/集合|
| **row**           | document or BSON document | 数据记录行/文档 |
| **column**        | field                    | 数据字段/域 |
| **index**         | index                    | 索引 |
| **table joins**   |  | 表连接，MongoDB不支持 |
||嵌入文档和链接      | MongoDB通过嵌入式文档来替代多表连接 |
| **primary key** (指定任何唯一的列或列组合作为主键) | **primary key** |在MongoDB中，主键自动设置为_id字段|
| **aggregation聚合 (e.g. group by)**                            | aggregation pipeline |聚合管道参见SQL到聚合映射图|

#### 1.4 数据模型

MongoDB最小的存储单位就是文档(document)对象。文档(document)对象对应于关系型数据库的行。数据在MongoDB中以BSON（Binary-JSON）文档的格式存储在磁盘上。
BSON（Binary Serialized Document Format）是一种类json的一种二进制形式的存储格式，简称Binary JSON。BSON和JSON一样，支持
内嵌的文档对象和数组对象，但是BSON有JSON没有的一些数据类型，如Date和BinData类型。
BSON采用了类似于 C 语言结构体的名称、对表示方法，支持内嵌的文档对象和数组对象，具有轻量性、可遍历性、高效性的三个特点，可
以有效描述非结构化数据和结构化数据。这种格式的优点是灵活性高，但它的缺点是空间利用率不是很理想。
Bson中，除了基本的JSON类型：string,integer,boolean,double,null,array和object，mongo还使用了特殊的数据类型。这些类型包括date,object id,binary data,regular expression 和code。每一个驱动都以特定语言的方式实现了这些类型，查看你的驱动的文档来获取详
细信息。
BSON数据类型参考列表：

| 数据类型      | 描述                                                         | 举例                                                 |
| ------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| 字符串        | UTF-8字符串都可表示为字符串类型的数据                        | {“x” : “foobar”}                                     |
| 对象id        | 对象id是文档的12字节的唯一 ID                                | {“X” :ObjectId() }                                   |
| 布尔值        | 真或者假：true或者false                                      | {“x”:true}+                                          |
| 数组          | 值的集合或者列表可以表示成数组                               | {“x” ： [“a”, “b”, “c”]}                             |
| 32位整数      | 类型不可用。JavaScript仅支持64位浮点数，所以32位整数会被自动转换 | shell是不支持该类型的，shell中默认会转换成64         |
| 64位整数      | 不支持这个类型。shell会使用一个特殊的内嵌文档来显示64位整数  | shell是不支持该类型的，shell中默认会转换成64位浮点数 |
| 64位浮点数    | shell中的数字就是这一种类型                                  | {“x”：3.14159，“y”：3}                               |
| null          | 表示空值或者未定义的对象                                     | {“x”:null}                                           |
| undefined     | 文档中也可以使用未定义类型                                   | {“x”:undefined}                                      |
| 符号          | shell不支持，shell会将数据库中的符号类型的数据自动转换成字符串 |                                                      |
| 正则表达式    | 文档中可以包含正则表达式，采用JavaScript的正则表达式语法     | {“x” ： /foobar/i}                                   |
| 代码          | 文档中还可以包含JavaScript代码                               | {“x” ： function() { /* …… */ }}                     |
| 二进制数据    | 二进制数据可以由任意字节的串组成，不过shell中无法使用        |                                                      |
| 最大值/最小值 | BSON包括一个特殊类型，表示可能的最大值。shell中没有这个类型。 |                                                      |

**提示：**
shell默认使用64位浮点型数值。{“x”：3.14}或{“x”：3}。对于整型值，可以使用NumberInt（4字节符号整数）或NumberLong（8字节符号整数），{“x”:NumberInt(“3”)}{“x”:NumberLong(“3”)}

#### 1.5 MongoDB的特点

MongoDB主要有如下特点：

1. **高性能：**
   MongoDB提供高性能的数据持久性。特别是,
   对嵌入式数据模型的支持减少了数据库系统上的I/O活动。
   索引支持更快的查询，并且可以包含来自嵌入式文档和数组的键。（文本索引解决搜索的需求、TTL索引解决历史数据自动过期的需求、地理位置索引可用于构建各种 O2O 应用）
   mmapv1、wiredtiger、mongorocks（rocksdb）、in-memory 等多引擎支持满足各种场景需求。
   Gridfs解决文件存储的需求。
2. **高可用性：**
   MongoDB的复制工具称为副本集（replica set），它可提供自动故障转移和数据冗余。
3. **高扩展性：**
   MongoDB提供了水平可扩展性作为其核心功能的一部分。
   分片将数据分布在一组集群的机器上。（海量数据存储，服务能力水平扩展）
   从3.4开始，MongoDB支持基于片键创建数据区域。在一个平衡的集群中，MongoDB将一个区域所覆盖的读写只定向到该区域内的那些片。
4. **丰富的查询支持：**
   MongoDB支持丰富的查询语言，支持读和写操作(CRUD)，比如数据聚合、文本搜索和地理空间查询等。
5. **其他特点**：如无模式（动态模式）、灵活的文档模型、

## 2、单机部署

### 2.1 Windows系统中的安装启动

第一步：下载安装包

MongoDB提供了可用于32位和64位系统的预编译二进制包，你可以从MongoDB官网下载安装，官网下载地址如下：https://www.mongodb.com/download-center/community。选择版本、系统环境后，直接点击 Download 下载即可。

![img](https://img-blog.csdnimg.cn/20191011150443215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x6YjM0ODExMDE3NQ==,size_16,color_FFFFFF,t_70)

根据上图所示下载zip包

提示：版本选择：

MongoDB的版本命名规范：x.y.z

y为奇数时表示当前版本为开发版，如：1.5.2、4.1.13

y为偶数是表示当前版本为稳定版，如：1.6.3、4.0.10

z为修正版号，数字越大越好

详情：http://docs.mongodb.org/manual/release-notes/#release-version-numbers



第二步：解压安装启动

将压缩吧解压到一个目录中

在解压目录中，手动建立一个目录用于存放数据文件，如data/db



方式一：命令行参数方式启动服务

```
mongod --dbpath=..\data\db
```

方式2：配置文件方式启动服务

```
storage:
  dbPath: D:\DBServer\mongodb-wein32-x86_64\data
```



### 2.2 Shell连接（MongoDB命令）

### 2.3 Compass-图形化界面客户端

### 2.4 Linux系统中的安装启动和连接

步骤如下：

1、先到官网下载压缩包mongod-linux-x

```bash
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-4.0.12.tgz
```

2、上传压缩包到linux服务器，解压到当前目录

```bash
tar -zxvf mongodb-linux-x86_64-4.0.12.tgz
```

3、移动解压后的文件夹到指定的目录中

```
mv mongodb-linux-x86_64-4.0.1 /usr/local/mongodb
```

4、新建几个目录，分表用来存储数据和日志

```
#数据存储目录
mkdir -p 
mkdir -p 
```

5、新建并修改配置文件

```bash
vim mongod.conf
```

配置文件内容如下

```yaml
systemLog:
   #MongoDB发送所有日志输出的目标指定为文件
   # #The path of the log file to which mongod or mongos should send all diagnostic logging information
   destination: file
   #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
   path: "/mongodb/single/log/mongod.log"
   #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。 
   logAppend: true
storage:
   #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
   ##The directory where the mongod instance stores its data.Default Value is "/data/db".
   dbPath: "/mongodb/single/data/db"
   journal:
      #启用或禁用持久性日志以确保数据文件保持有效和可恢复。
      enabled: true
processManagement:
   #启用在后台运行mongos或mongod进程的守护进程模式。
   fork: true
net:
   #服务实例绑定的IP，默认是localhost
   bindIp: localhost,192.168.0.2 #想让其他机器访问，需要绑定局域网ip
   #bindIp
   #绑定的端口，默认是27017
   port: 27017

```

6、启动MongoDB服务

```
/usr/local/mongodb/bin/mongod -f /mongodb/mongod.conf
```

通过进程查看服务是否启动

```bash
ps -ef |grep mongod
```



主要的操作步骤参考如下：

```bash
#客户端登录服务，注意 这里通过localhost登录，如需远程登录，必须先登录认证才行。
mongo --port 27017
#切换到amin库
use admin
#关闭服务
db.shutdownServer()
```



## 3、基本常用命令

### 3.1 案例需求

存放文章评论的数据存放到MongoDB中，数据结构参考如下：

![img](https://img-blog.csdnimg.cn/20200908194015126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI0NzU3NjM1,size_16,color_FFFFFF,t_70)

### 3.2 数据库操作

1）选择和创建数据库：use 数据库名称

如果数据库不存在则自动创建

```bash
use articledb
```

 （2）查看有权限查看的所有的数据库命令

```bash
show dbs 或 show databases
```

（3）查看当前正在使用的数据库命令

```bash
db
```

MongoDB 中默认的数据库为 test，如果你没有选择数据库，集合将存放在 test 数据库中

有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库。

1）admin： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。

2）local: 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合

3）config: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

（4）数据库的删除

```bash
db.dropDatabase()
```

### 3.3 集合操作

集合，类似关系型数据库中的表。 可以显示的创建，也可以隐式的创建。

注意: 集合只有在内容插入后才会创建，就是说，创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建。 

（1）集合的显式创建：db.createCollection(name)

```bash
db.createCollection(articles)
```

（2）查看当前库中的表：show tables

```bash
show collections 或 show tables
```

（3）集合的隐式创建

当向一个集合中插入一个文档的时候，如果集合不存在，则会自动创建集合。

```bash
db.collection.insert()
```

（4）集合的删除：db.collection.drop()

```bash
db.collection.drop()
```

### 3.4 文档基本CRUD

文档（document）的数据结构和 JSON 基本一样，所有存储在集合中的数据都是 BSON 格式。

#### （1）单个文档插入

使用insert() 或 save() 方法向集合中插入文档

```bash
db.collection.insert(



    <document or array of documents>,



    {



        writeConcern: <document>,



        ordered: <boolean>



    }



)
```

![img](https://img-blog.csdnimg.cn/20200909164947648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI0NzU3NjM1,size_16,color_FFFFFF,t_70)

```bash
db.articles.insert({"articleid":"100000","content":"今天天气真好，阳光明



媚","userid":"1001","nickname":"Rose","createdatetime":new Date(),"likenum":NumberInt(10),"state":null})
```

提示：

1）comment集合如果不存在，则会隐式创建

2）mongo中的数字，默认是double类型，如果要存整型，必须使用函数NumberInt(整型数字)，否则取出来就有问题了。

3）插入的数据没有指定 _id ，会自动生成主键值，其类型是ObjectID类型

4）如果某字段没值，可以赋值为null，或不写该字段。 

#### （2）批量插入

```bash
db.collection.insertMany(



    [ <document 1> , <document 2>, ... ],



    {



        writeConcern: <document>,



        ordered: <boolean>



    }



)
db.articles.insertMany([



{"_id":"1","articleid":"100001","content":"我们不应该把清晨浪费在手机上，健康很重要，一杯温水幸福你我



他。","userid":"1002","nickname":"相忘于江湖","createdatetime":new Date("2019-08-



05T22:08:15.522Z"),"likenum":NumberInt(1000),"state":"1"},



{"_id":"2","articleid":"100001","content":"我夏天空腹喝凉开水，冬天喝温开水","userid":"1005","nickname":"伊人憔



悴","createdatetime":new Date("2019-08-05T23:58:51.485Z"),"likenum":NumberInt(888),"state":"1"},



{"_id":"3","articleid":"100001","content":"我一直喝凉开水，冬天夏天都喝。","userid":"1004","nickname":"杰克船



长","createdatetime":new Date("2019-08-06T01:05:06.321Z"),"likenum":NumberInt(666),"state":"1"},



{"_id":"4","articleid":"100001","content":"专家说不能空腹吃饭，影响健康。","userid":"1003","nickname":"凯



撒","createdatetime":new Date("2019-08-06T08:18:35.288Z"),"likenum":NumberInt(2000),"state":"1"},



{"_id":"5","articleid":"100001","content":"研究表明，刚烧开的水千万不能喝，因为烫



嘴。","userid":"1003","nickname":"凯撒","createdatetime":new Date("2019-08-



06T11:01:02.521Z"),"likenum":NumberInt(3000),"state":"1"}



]);
```

如果某条数据插入失败，将会终止插入，但已经插入成功的数据不会回滚掉。 因为批量插入由于数据较多容易出现失败，因此，可以使用try catch进行异常捕捉处理，测试的时候可以不处理。

```bash
try {



db.articles.insertMany([



{"_id":"1","articleid":"100001","content":"我们不应该把清晨浪费在手机上，健康很重要，一杯温水幸福你我



他。","userid":"1002","nickname":"相忘于江湖","createdatetime":new Date("2019-08-



05T22:08:15.522Z"),"likenum":NumberInt(1000),"state":"1"},



{"_id":"2","articleid":"100001","content":"我夏天空腹喝凉开水，冬天喝温开水","userid":"1005","nickname":"伊人憔



悴","createdatetime":new Date("2019-08-05T23:58:51.485Z"),"likenum":NumberInt(888),"state":"1"},



{"_id":"3","articleid":"100001","content":"我一直喝凉开水，冬天夏天都喝。","userid":"1004","nickname":"杰克船



长","createdatetime":new Date("2019-08-06T01:05:06.321Z"),"likenum":NumberInt(666),"state":"1"},



{"_id":"4","articleid":"100001","content":"专家说不能空腹吃饭，影响健康。","userid":"1003","nickname":"凯



撒","createdatetime":new Date("2019-08-06T08:18:35.288Z"),"likenum":NumberInt(2000),"state":"1"},



{"_id":"5","articleid":"100001","content":"研究表明，刚烧开的水千万不能喝，因为烫



嘴。","userid":"1003","nickname":"凯撒","createdatetime":new Date("2019-08-



06T11:01:02.521Z"),"likenum":NumberInt(3000),"state":"1"}



]);



} catch (e) {



print (e);



}
```

#### （3）文档的基本查询

```bash
db.collection.find(<query>, [projection])
```

![img](https://img-blog.csdnimg.cn/20200909111308301.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI0NzU3NjM1,size_16,color_FFFFFF,t_70)

1）查询所有

```bash
db.comment.find() 或 db.comment.find({})
```

按一定条件来查询，比如查询userid为1003的记录

```bash
db.articles.find({userid:'1003'})
```

2）查询单条数据

查询用户编号是1003的记录，但只最多返回符合条件的第一条记录

```bash
db.articles.findOne({userid:'1003'})
```

3）投影查询（Projection Query）：

要查询结果返回部分字段，需要使用投影查询只显示指定的字段。

```bash
db.articles.find({userid:"1003"},{userid:1,nickname:1})



{ "_id" : "4", "userid" : "1003", "nickname" : "凯撒" }



{ "_id" : "5", "userid" : "1003", "nickname" : "凯撒" }
```

#### （4）文档的更新

```bash
db.collection.update(query, update, options)



//或



db.collection.update(



    <query>,



    <update>,



    {



        upsert: <boolean>,



        multi: <boolean>,



        writeConcern: <document>,



        collation: <document>,



        arrayFilters: [ <filterdocument1>, ... ],



        hint: <document|string> // Available starting in MongoDB 4.2



    }



)
```

主要关注前四个参数即可 

![img](https://img-blog.csdnimg.cn/20200909114257953.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI0NzU3NjM1,size_16,color_FFFFFF,t_70) 1）覆盖的修改

修改_id为1的记录，点赞量为1001；执行后，会发现这条文档除了likenum字段其它字段都不见了

```bash
db.articles.update({_id:"1"},{likenum:NumberInt(1001)})
```

2）局部修改

使用修改器$set来实现修改_id为2的记录，浏览量为889；执行后只修改likenum，其他字段还在

```bash
db.articles.update({_id:"2"},{$set:{likenum:NumberInt(889)}})
```

3）批量的修改，multi设为true

更新所有用户为 1003 的用户的昵称为玉皇大帝 。

```bash
//默认只修改第一条数据



db.articles.update({userid:"1003"},{$set:{nickname:"玉皇大帝"}})



//修改所有符合条件的数据



db.articles.update({userid:"1003"},{$set:{nickname:"王母娘娘"}},{multi:true})
```

4）列值增长的修改，$inc增加

对3号数据的点赞数，每次递增1

```bash
db.articles.update({_id:"3"},{$inc:{likenum:NumberInt(1)}})
```

#### （4）删除文档

db.集合名称.remove(条件)

据全部删除，慎用

```bash
db.articles.remove({})
```

删除_id=1的记录

```bash
db.articles.remove({_id:"1"})
```

### 3.5 文档的分页查询

#### 3.5.1 统计查询

统计查询，用count()方法

```bash
db.collection.count(query, options)
```

![img](https://img-blog.csdnimg.cn/20200909144643733.png)

统计comment集合的所有的记录数

```bash
db.articles.count()
```

 按条件统计记录数：统计userid为1003的记录条数

```bash
db.articles.count({userid:"1003"})
```

#### 3.5.2 分页列表查询

limit() 读取指定数量的数据，默认值20

获取10条数据

```bash
db.articles.find().limit(10)
```

skip() 跳过指定数量的数据，默认值是0

跳过前面三条数据开始取数据

```
db.articles.find().skip(3)
```

分页查询：每页2个

```bash
//第一页
db.articles.find().skip(0).limit(2)

//第二页
db.articles.find().skip(2).limit(2)

//第三页
db.articles.find().skip(4).limit(2)
```

#### 3.5.3 排序查询

sort() 对数据进行排序，通过参数指定排序的字段，并使用 1 （升序）和 -1 （降序）来指定排序的方式

```bash
db.collection.find().sort({KEY:1})
```

对userid降序排列，并对访问量进行升序排列

```bash
db.articles.find().sort({userid:-1,likenum:1})
```

skip(), limilt(), sort()三个放在一起执行的时候，执行的顺序 是sort(), skip()，limit()，和命令编写顺序无关。

### 3.6 文档的更多查询

#### 3.6.1 正则的复杂条件查询

MongoDB的模糊查询是通过正则表达式的方式实现的，正则表达式是js的语法，直接量的写法。

```bash
db.collection.find({field:/正则表达式/})
或
db.集合.find({字段:/正则表达式/})
```

要查询评论的内容中以“专家”开头的 

```bash
db.articles.find({content:/^专家/})
```

#### 3.6.2 比较查询

```bash
db.集合名称.find({ "field" : { $gt: value }}) // 大于: field > value
db.集合名称.find({ "field" : { $lt: value }}) // 小于: field < value
db.集合名称.find({ "field" : { $gte: value }}) // 大于等于: field >= value
db.集合名称.find({ "field" : { $lte: value }}) // 小于等于: field <= value
db.集合名称.find({ "field" : { $ne: value }}) // 不等于: field != value
```

查询评论点赞数量大于700的记录

```bash
db.articles.find({likenum:{$gt:NumberInt(700)}})
```

#### 3.6.3 包含查询

包含使用$in操作符。查询评论的集合中userid字段包含1003或1004的文档

```bash
db.articles.find({userid:{$in:["1003","1004"]}})
```

不包含使用$nin操作符。查询评论集合中userid字段不包含1003和1004的文档

```bash
db.articles.find({userid:{$nin:["1003","1004"]}})
```

4）条件连接查询

查询同时满足两个以上条件，使用$and操作符将条件进行关联。（相 当于SQL的and） 格式为：

```bash
$and:[ { },{ },{ } ]
```

查询评论集合中likenum大于等于700 并且小于2000的文档：

```bash
db.articles.find({$and:[{likenum:{$gte:NumberInt(700)}},{likenum:{$lt:NumberInt(2000)}}]})
```

两个以上条件之间是或者的关系

```bash
$or:[ { },{ },{ } ]
```

查询评论集合中userid为1003，或者点赞数小于1000的文档记录

```bash
db.articles.find({$or:[ {userid:"1003"} ,{likenum:{$lt:1000} }]})
```

### 3.7 常用命令小结

```bash
选择切换数据库：use articledb
插入数据：db.comment.insert({bson数据})
查询所有数据：db.comment.find();
条件查询数据：db.comment.find({条件})
查询符合条件的第一条记录：db.comment.findOne({条件})
查询符合条件的前几条记录：db.comment.find({条件}).limit(条数)
查询符合条件的跳过的记录：db.comment.find({条件}).skip(条数)
修改数据：db.comment.update({条件},{修改后的数据}) 或db.comment.update({条件},{$set:{要修改部分的字段:数据})
修改数据并自增某字段值：db.comment.update({条件},{$inc:{自增的字段:步进值}})
删除数据：db.comment.remove({条件})
统计查询：db.comment.count({条件})
模糊查询：db.comment.find({字段名:/正则表达式/})
条件比较运算：db.comment.find({字段名:{$gt:值}})
包含查询：db.comment.find({字段名:{$in:[值1，值2]}})或db.comment.find({字段名:{$nin:[值1，值2]}})
条件连接查询：db.comment.find({$and:[{条件1},{条件2}]})或db.comment.find({$or:[{条件1},{条件2}]})
```

## 4、索引-Index 

### 4.1 概述

索引支持在MongoDB中高效地执行查询。如果没有索引，MongoDB必须执行全集合扫描，即扫描集合中的每个文档。这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟。 如果查询存在适当的索引，MongoDB可以使用该索引限制必须检查的文档数。

索引是特殊的数据结构，它以易于遍历的形式存储集合数据集的一小部分。索引存储特定字段或一组字段的值，按字段值排序。索引项的排序支持有效的相等匹配和基于范围的查询操作。此外，MongoDB还可以使用索引中的排序返回排序结果。

官网文档：http://docs.mongodb.com/manual/indexes/

了解：

MongoDB索引使用B树数据结构（确切的说是B-Tree，MySQL是B+Tree）

### 4.2 索引的类型

#### 4.2.1 单字段索引

MongoDB支持在文档的单个字段上创建用户定义的升序/降序索引，称为单字段索引（Single Field Index）。

对于单个字段索引和排序操作，索引键的排序顺序（即升序或降序）并不重要，因为MongoDB可以在任何方向上遍历索引。

#### 4.2.2 复合索引

MongoDB还支持多个字段的用户定义索引，即复合索引（Compound Index）。

复合索引中列出的字段顺序具有重要意义。例如，如果复合索引由 { userid: 1, score: -1 } 组成，则索引首先按userid正序排序，然后在每个userid的值内，再在按score倒序排序。

#### 4.2.3 其他索引

地理空间索引（Geospatial Index）、文本索引（Text Indexes）、哈希索引（Hashed Indexes）。

地理空间索引（Geospatial Index） 为了支持对地理空间坐标数据的有效查询，MongoDB提供了两种特殊的索引：返回结果时使用平面几何的二维索引和返回结果时使用球面 几何的二维球面索引。

文本索引（Text Indexes） MongoDB提供了一种文本索引类型，支持在集合中搜索字符串内容。这些文本索引不存储特定于语言的停止词（例如“the”、“a”、“or”）， 而将集合中的词作为词干，只存储根词。

哈希索引（Hashed Indexes） 为了支持基于散列的分片，MongoDB提供了散列索引类型，它对字段值的散列进行索引。这些索引在其范围内的值分布更加随机，但只支 持相等匹配，不支持基于范围的查询。

### 4.3 索引的管理操作

#### 4.3.1索引的查看

```bash
# 返回一个集合中的所有索引的数组。
db.collection.getIndexes()
```

默认_id索引：

MongoDB在创建集合的过程中，在 _id 字段上创建一个唯一的索引，默认名字为 _id_，该索引可防止客户端插入两个具有相同值的文档，不能在_id字段上删除此索引。

注意：该索引是唯一索引，因此值不能重复，即 _id 值不能重复的。在分片集群中，通常使用 _id 作为片键。

#### 4.3.2 索引的创建

```bash
# 在集合上创建索引
db.collection.createIndex(keys, options)
```

![img](https://img-blog.csdnimg.cn/20200914150533747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI0NzU3NjM1,size_16,color_FFFFFF,t_70)

1）单字段索引示例

```bash
db.articles.createIndex({userid:1})
```

查看索引

```bash
db.articles.getIndexes()
[
    {
        "v" : 2,
        "key" : {
            "_id" : 1
        },
        "name" : "_id_",
        "ns" : "articledb.articles"
    },
    {
        "v" : 2,
        "key" : {
        "userid" : 1
        },
        "name" : "userid_1",
        "ns" : "articledb.articles"
    }
]
```

 

2）复合索引：

对 userid 和 nickname 同时建立复合（Compound）索引：

```bash
 db.articles.createIndex({userid:1,nickname:-1})
```

#### 4.3.3 索引的移除

1）指定索引的移除

db.collection.dropIndex(index)

![img](https://img-blog.csdnimg.cn/20200914152737238.png)

删除 articles集合中 userid 字段上的升序索引：

```bash
db.articles.dropIndex({userid:1})

#或者通过索引名称删除
db.articles.dropIndex("userid_1")
```

2）所有索引的移除

```bash
db.articles.dropIndexes()
```

注意： _id 的字段的索引是无法删除的，只能删除非 _id 字段的索引。

## 4、索引的使用

### （1）执行计划

分析查询性能（Analyze Query Performance）通常使用执行计划（解释计划、Explain Plan）来查看查询的情况，如查询耗费的时间、是否基于索引查询等。 如果想知道建立的索引是否有效，效果如何，都需要通过执行计划查看。

命令：db.collection.find(query,options).explain(options)

```bash
> db.articles.find({userid:"1003"}).explain()



 



{



    "queryPlanner" : {



        "plannerVersion" : 1,



        "namespace" : "articledb.articles",



        "indexFilterSet" : false,



        "parsedQuery" : {



            "userid" : {



                "$eq" : "1003"



            }



        },



        "winningPlan" : {



            "stage" : "COLLSCAN",



            "filter" : {



                "userid" : {



                    "$eq" : "1003"



                }



            },



            "direction" : "forward"



        },



        "rejectedPlans" : [ ]



    },



    "serverInfo" : {



        "host" : "9ef3740277ad",



        "port" : 27017,



        "version" : "4.0.10",



        "gitVersion" : "c389e7f69f637f7a1ac3cc9fae843b635f20b766"



    },



    "ok" : 1



}
```

关键点看： "stage" : "COLLSCAN", 表示全集合扫描；"stage" : "IXSCAN" ,基于索引的扫描

### （2）涵盖的查询

当查询条件和查询的投影仅包含索引字段时，MongoDB直接从索引返回结果，而不扫描任何文档或将文档带入内存。 这些覆盖的查询可以非常有效。



# MongooDb集群和安全

## 课程目标

- MongoDB的副本集：操作、主要概念、故障转移、选举规划
- MongoDB的分片集群：概念、优点、操作、分片策略、故障转移
- MongoDB的安全认证

## 1. 副本集-Replica Sets

### 1.1 简介

MongoDB中的



### 1.2 副本集的三个角色

副本集有两种类型三种角色

两种类型：

- 主节点（Primary）类型
- 次要节点