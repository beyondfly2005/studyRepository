# DataX-Migration

> 参考文档： https://my.oschina.net/jawf/blog/2964783



### 什么是DataX-Migration 

DataX-Migration 是云学堂开源的基于阿里巴巴DataX 3.0的数据库迁移工具。支持对Mysql，Oracle，SqlServer, PostgreSql之间的相互迁移, 支持迁移时带where查询条件，并生成迁移数据报表。



### 什么是DataX

**DataX** 是阿里巴巴集团内被广泛使用的离线数据同步工具/平台，实现包括 MySQL、Oracle、SqlServer、Postgre、HDFS、Hive、ADS、HBase、OTS、ODPS 等各种异构数据源之间高效的数据同步功能。
**DataX** 在阿里巴巴集团内被广泛使用，承担了所有大数据的离线同步业务，并已持续稳定运行了6年之久。目前每天完成同步8w多道作业，每日传输数据量超过300TB。



### 为什么还需要DataX-Migration

**DataX**专注于对数据的同步，它使用脚本以及可配置的方式，以一个个独立的脚本任务，非常方便地对单表的数据进行同步操作。但我们需要更加智能或自动的方式同步整个数据库，所以我们对DataX进行了包装，以更方便地进行整个数据库的迁移工作。



### DataX-Migration的功能

**DataX-Migration** 能根据用户配置数据库表tables的查询条件，生成这些数据库表的单独的DataX json配置，然后启动DataX的脚本来开始这些表的数据迁移，并生成相应的cvs**报表**。当表的数量过多时，可以配置切分策略来划分出**多个线程**来同时做迁移已加快迁移数据。



### DataX-Migration安装

1、git下载源文件

```bash
git clone https://github.com/Jawf/datax-migration.git
```

2、mvn编译

```bash
maven clean package
```

3、编译后的文件复制到 datax主目录

```
复制target/datax-migration.jar 和 target/datax-migration_lib 
到 datax/ 主目录
```

```
5. 打开 datax-migration.jar, 编辑 config.properties, 
配置迁移数据库信息源 source/target url, dbname, user, password 等等
6. 打开 datax-migration.jar, 编辑 job/jobtemplate.json accordingly, 它默认从mysqlreader->mysqlwriter进行迁移
7. java -jar datax-migration.jar
```