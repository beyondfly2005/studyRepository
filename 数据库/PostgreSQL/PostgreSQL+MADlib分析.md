# PostgreSQL + MADlib分析扩展


> Postgresql之Madlib安装
> https://blog.csdn.net/YangPF1910/article/details/89846557
> PostgreSQL机器学习插件MADlib实践 Breadth-First Search篇
> https://blog.geohey.com/postgresqlji-qi-xue-xi-cha-jian-madlibshi-jian-breadth-first-searchpian/amp/

### 什么是MADlib
MADlib 是伯克利大学的一个开源软件项目. 主要目的是扩展数据库的分析能力. 支持PostgreSQL和Greenplum数据库. 可以非常方便的加载到PostgreSQL或Greenplum, 扩展数据库的分析功能。

### 如何集成
进入apache官网的下载页面，http://madlib.apache.org/download.html
linux系统中使用wget命令下载，下载完格式为rpm
使用rpm命令安装
```
/usr/local/madlib/bin/madpack –p psotgres –c psotgres@127.0.0.1:5432/postgres install
```

### 使用方法


### MADLib + PostgreSQL 查询计划
> https://www.jb51.cc/faq/1334927.html
