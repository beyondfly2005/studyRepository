# TDengine 时序数据库

### TDEngin开源地址
> https://github.com/taosdata/TDengine

### 数据库连接工具 DBeaver
> https://blog.csdn.net/kangweijian/article/details/126815963
#### 官网下载地址
> https://dbeaver.io/
> https://dbeaver.io/files/dbeaver-ce-latest-x86_64-setup.exe
> https://dbeaver.io/files/dbeaver-ce-latest-win32.win32.x86_64.zip
#####
>  驱动下载


### TDengineGUI 图形管理工具
> https://github.com/arielyang/TDengineGUI/blob/main/README_.md
> TDengineGUI是一个基于electron构建的，针对时序数据库TDengine的图形化管理工具。具有跨平台、易于使用、版本适应性强等特点。
> 



### TDengine Client
> 
> 
> 
> 

##### 创建数据库 
```
#创建库 2.x
CREATE DATABASE test KEEP 365 10 BLOCKS 6 UPDATE 1;

#创建数据库3.x
create database if not exists test vgroups 10 buffer 10

CREATE DATABASE `test` BUFFER 96 CACHEMODEL 'none' COMP 2 DURATION 14400m WAL_FSYNC_PERIOD 3000 MAXROWS 4096 MINROWS 100 KEEP 5256000m,5256000m,5256000m PAGES 256 PAGESIZE 4 PRECISION 'ms' REPLICA 3 STRICT 'off' WAL_LEVEL 1 VGROUPS 2 SINGLE_STABLE 0


```

参数
参数	含义	示例
- keep	该数据库的数据保留多长天数，缺省是 3650 天(10 年)
- UPDATE	0=不允许更新；1=行全列更新，未赋值列被设置为NUll；2=行部分列更新，未赋值列不变
- cache	内存块的大小	默认16M
- replica	副本个数	默认1个
- precision	创建数据库时使用的时间精度	默认ms




### docker-compose 方式启动TDengine
> https://blog.csdn.net/weixin_43915643/article/details/127512586


### TDengine在线文档
> https://docs.taosdata.com/get-started/

### TDengine快速上手
> https://www.bilibili.com/video/BV12a411c7yf

### TDengine + EMQ X +Grafana  打造物联网平台
> https://www.bilibili.com/video/BV1Dy4y1B7cW
> https://www.bilibili.com/video/BV1Dy4y1B7cW
> https://www.taosdata.com/engineering/2007.html


### SpringBOot集成TDengine
> https://betheme.net/xiaochengxu/7749.html


### TDengine基本概念和建模思路
> https://blog.csdn.net/weixin_42599091/article/details/128674166
> 
> 
> 

### 集群搭建
https://blog.csdn.net/firewater23/article/details/125793627


### 集成kafka裂变存储
> https://blog.csdn.net/zwrlj527/article/details/129097044
> 
> 


### TDengin 3.0
> https://blog.51cto.com/u_15641375/5967969
> 
> 
> 
> 
### 创建超级表 并通过 超级表插入数据
```sql

## 1 创建超级表 meters

CREATE STABLE meters(ts TIMESTAMP, current FLOAT, voltage INT, phase FLOAT) TAGS(location BINARY(30), groupId INT);

## 2 以超级表为模板直接往子表插入数据

INSERT INTO d21001 USING meters (groupId) TAGS (2) VALUES ('2021-07-13 14:06:33.196', 10.15, 217, 0.33)

```
