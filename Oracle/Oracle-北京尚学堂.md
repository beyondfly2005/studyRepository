> https://www.bilibili.com/video/BV1Xb411q7h6?p=74







#### Oracle 物化视图


自动刷新 ON commit
手动刷新 on demand

create materialized view mv_name [选型N]
as select * form table_name
选型
build immediate deferred
是否在创建视图是 生成数据
选项：refresh fast complete  force never

选项 on demand ，commit
选项 start with
next

删除物化视图
drop materialized view mv_name

dba权限才能建立物化视图、

收回权限
revoke create materialized from scott;

赋予权限
grant  create materialized  view to scott;

create materialized  view v_ab
refresh force on commit
as select * froma.id,a.name,b.clisid,b.name as clsname from a,b where a.classid=b.clsid
--全表刷新


create materialized  view v_ab
refresh force on demand
start with sysdate next sysdate+1
as
select * form xxx

从今天开始 ，下次刷新时间 是明天



create materialized  view Mv_ab
refresh force on demand
start with sysdate next sysdate+1/1440
as
select * form xxx

从现在开始 ，下次刷新时间 是下一分钟

3.5.5 快速刷新的建立

增量刷新

create materialized view log on a with rowid;
create materialized view log on b with rowid;
create materialized view mv_ab
refresh fast on demand
start with sysdate
next sysdate+1/1440
as
select  a.rowid as arowid，b.rowid as browid,
  xxxxxxx  from xxx where a,b where a.xx=b.xx

查询是 必须要使用 log a 的id   和logb 的id


3.6.1 同义词
Oracle的同义词（synonyms）
从字面上理解就是别名的意思，和视图的功能类似，就是一种映射关系

共有同意词 
私有同义词


共有同意词 
create public synonym  to scott;

select * from employees t;

--当前用户为hr 表名hr.employees;
create public synonym aaa for hr.employees;

--相当于别名
select * from aaa;

--将aaa的查询权限 赋予 scott用户
grant select on aaa to scott；

不同实例的数据库  也可以做同义词
rootep.

3.7.1 DBLINK
不同数据库实例 或者远程进行连接




--使用dblink进行查询

select * from tt from AAA@db1

DBlink不能查 BLOB  COLB的

4.x  PL/SQL编程

5.1.1 表空间的使用
创建临时表空间
create temporary tablespac zcdb_temp
tempfile 'D:\oracle\datafile\zcdb_temp.dbf' size 1024m 
autoextend on
next 50m maxsize 2048m
extent management local;



##### 5.2.2 OLTP和OLAP

联系事务处理OLTP 连接分析处理OLAP



单表一千亿以下的数据 oracle 理论上也可以处理



两者的区别

| XX       | OLTP                 | OLAP             |
| -------- | -------------------- | ---------------- |
| 系统功能 | 日常交易处理         | 统计、分析、报表 |
| DB设计   | 面向实时交易类的应用 |                  |
| 数据处理 |                      |                  |
| 实时性   |                      |                  |
|          |                      |                  |



##### 5.2.4 关系型数据库和NoSQL数据的特点

|      | 关系型数据库 | NoSQL |
| ---- | ------------ | ----- |
|      |              |       |

##### 5.2.5 何为数据切分

垂直切分 水平切分

垂直切分 oracle 是采用不同的scheme实现



##### 5.2.6 垂直拆分



##### 5.2.7 水平切分



##### 5.2.8 数据拆分优点和缺点

垂直查房的优点：

业务逻辑清晰

可扩展性强

维护简单方便

水平拆分的优点：

查房规则做的足够好，基本上可以单库实现join操作

提高并发的能力









