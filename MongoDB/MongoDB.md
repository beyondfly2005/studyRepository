## MongoDB简介

- MongoDB是为快速开发互联网Web应用而设计的数据库系统
- MongoDB的设计目标是极简、灵活、作为Web应用栈的一部分
- MongoDB的数据模型是面向文档的，所谓文档就是一种类似JSON的结构，简单理解MongoDB这个数据库中存的是各种各样的JSON。（BSON）Binary Json

## MongoDB三个概念

- 数据库database

  数据库是一个仓库，在仓库中可以存放集合

- 集合 collection

  集合类似数组，在集合中可以存放文档。

- 文档 document

  文档是数据库最小单位



## MongoDB下载安装

- 下载地址

  https://www.mogodb.org/dl/win32/

- MongoDB的版本偶数版为稳定版本，奇数版为开发版

- 配置环境变量 将MongoDB的安装路径\bin 配置到path中

- 测试安装 cmd 输入 mongo

- C盘根目录创建文件夹data\db

- cmd 输入mongod启动mongodb服务器

- 客户端：再打开一个cmd 输入 mongo ；连接mongoDB 出现 >  表明服务器启动成功

- 32位系统，第一次启动服务器需要输入: mongod --storageEngine=mmapv1

  以后启动 mongod

- 指定其他路径 mongod --dbpath d:\mongodb\data

- 默认端口21017 指定端口  mpmdod --port10056

- 手动创建服务

  - 创建目录 mkdir c:\data\db  mkdir c:\data\log

  - 创建配置文件 c:\progrogramFile\MongdDB\Server3.2\

    bin同目录下mongod.cfg文件

  - 写入

  ```config
  systemLog:
      destination: file
      path: c:\data\log\mongod.log
  storage:
      dbpath: c:\data\db
  ```

  - 以管理员身份运行  执行如下命名

  sc.exe create MongoDB binPath="\"c:\Program Files\MongoDB\Server\3.2\bin\mongod.exe\" --service --config="\c:\program Files\MongoDB\Server\3.2\mongod.cfg\"" DisplayName="MongoDB" start="aotu"

  - 启动Mongodb服务
  - 如果启动失败，证明上面的操作有误，在控制台输入 sc delete MongoDB 删除之前的配置服务，然后从第一步再来一次

Mongod Shell 可以编写js代码

show dbs			显示当前所有数据库

show databases  显示当前所有数据库

use db1 				进入数据库

数据库和集合 不需要显示创建，在第一次向数据库中添加文档时，自动创建

db  显示当前所在的数据库

show collections 显示数据库中有多少个集合

数据库的CRUD

 - 插入文档

   db.<collection>.insert(doc)  

   向test数据库students集合中插入一个学生对象 test.students.insert({name:'孙悟空'}，age:18,gender:'男')

   insertone

