# CRM项目系统管理-后台管理项目完整版


> > https://www.bilibili.com/video/BV1sp4y1z7Jf
>
> 2021版：
>
> https://www.bilibili.com/video/BV1tK411g7VV
>
> 最新文档已经转到 语雀

### 1、学习目标

1 CRM系统概念

2 CRM模块划分

3 数据表结构设计

4 项目环境搭建与测试

5 项目功能

​	用户登录

​	密码修改退出

​	全局异常统一处理

​	非法请求拦截

​	记住我

### 2 CRM系统概念与项目开发流程

#### 2.1 CRM基本概念

#### 2.2 CRM分类

- 按客户类型的不同，分为B2B和B2C
- 按CRM管理侧重点不同，分为操作型和分析型。大部分为操作型

#### 2.3 企业项目开发流程

- 调研部（市场/调研组/BD组）
- 产品组
- UI组
- 开发组
- 测试组

### 3 CRM系统模块划分

1 基础模块

- 登录
- 退出
- 记住我
- 密码修改

2 营销管理

- 营销机会管理
- 客户开发计划

3 系统管理

- 字典管理
- 用户管理
- 角色管理
- 资源管理

4 客户管理

- 客户信息管理
- 流失客户管理

5 服务管理

- 服务创建
- 服务分配
- 服务处理
- 服务反馈
- 服务归档

6 数据报表

- 客户构成分析
- 客户贡献分析
- 客户服务分析
- 客户流失分析

### 4 CRM系统数据库设计

#### 4.1 E-R图表简介

##### 4.1.1 营销管理模块

###### t_sale_chance 营销机会表

###### t_cus_dev_plan 客户开发计划项

- 营销机会id
- 计划项
- 计划时间
- 执行效果
- 创建时间
- 更新时间
- 是否有效

##### 4.1.2 客户管理

###### 4.1.2.1 客户信息管理

客户表t_customer

客户联系人 t_customer_linkman

客户交往记录 t_customer_contact

客户订单 t_customer_order

客户订单详情 t_customer_details

###### 4.1.2.2 客户流失管理

客户流失表 t_customer_loss

客户暂缓表 t_customer_reprieve

##### 4.1.3 服务管理

客户服务表 t_customer_serve

##### 4.1.4 系统管理

###### 4.1.4.1 权限管理模块E-R模型

用户            角色               模块 

​       用户角色     权限表

###### 4.1.4.2 字典&日志

数据字典 t_datadic

系统日志 t_log

#### 4.2 表结构详情

t_sale_chance

```
id int(11)
chance_source varchar(200)
customer_name varchar(100)
cgjl	客户开发成功几率
overview
link_man
link_phone
description
assign_man
assign_time
state
dev_result
is_valid
create_date
update_date
```

```sql
create table t_sale_chance (
    id int(11),
    chance_source varchar(300),
    customer_name varchar(100),
    cgjl int(11) comment '客户开发成功几率',
    overview varchar(300) ,
    link_man varchar(100) ,
    link_phone varchar(100) ,
    description varchar(1000) ,
    assign_man varchar(100) ,
    assign_time varchar(100) ,
    state int(4) ,
    dev_result int(4) ,
    is_valid int(4),
    create_date datetime,
    update_date datetime
)
```



###### t_cus_dev_plan 客户开发计划项

```
id
sale_chance_id 	- 营销机会id
plan_item	- 计划项
plan_date	- 计划时间
exe_affect	- 执行效果
create_date	- 创建时间
update_date	- 更新时间
is_valid	- 是否有效
```

```sql
create table t_cus_dev_plan(
    id int(11),
    sale_chance_id int(11) comment  '营销机会id',
    plan_item varchar(100)comment  '计划项',
    plan_date datetime comment  ' 计划时间',
    exe_affect varchar(100) comment  '执行效果',
    create_date datetime comment  '创建时间',
    update_date datetime comment  '更新时间',
    is_valid int(4) comment '是否有效'
)
```

客户表 t_customer

```
id int(11)
khno 
name
area
cus_manager
level
myd 	客户满意度
xyd 	客户信用度
address 
post_code
phone
fax
web_sit
yyzzzch	营业执照注册号
fr		法人代表
aczj	注册资金
nyye	年营业额
khzh	开户账号
dsdjh	地税登记号
gsdjh	国税登记号
state	流失状态
is_valid
create_date
update_date
```

```sql

create table t_customer(
    id int(11),
        khno varchar(20),
        name varchar(50),
        area varchar(20),
        cus_manager varchar(20),
        level varchar(30),
        myd  varchar(30) comment '客户满意度',
        xyd varchar(30)	comment '客户信用度',
        address varchar(200)  ,
        post_code varchar(10) ,
        phone varchar(20),
        fax varchar(20),
        web_sit varchar(50),
        yyzzzch varchar(50)	comment '营业执照注册号',
        fr	varchar(50)	comment '法人代表',
        aczj varchar(20) comment '注册资金',
        nyye varchar(50) comment '年营业额',
        khzh varchar(50) comment  '开户账号',
        dsdjh varchar(50) comment 	'地税登记号',
        gsdjh varchar(50) comment 	'国税登记号',
        state varchar(50) comment 	'流失状态',
        is_valid int(4),
        create_date datetime,
        update_date datetime
)
```

客户联系人 t_customer_linkman

```
id
cus_id
link_name
sex
zhiwei
ofice_phone
phone
is_valid
create_date
update_date
```

```sql
create table t_customer_linkman(
    id int(11),
        cus_id int(11),
        link_name varchar(20),
        sex varchar(20),
        zhiwei varchar(50),
        office_phone varchar(30),
        phone varchar(30),
        is_valid int(4),
        create_date datetime,
        update_date datetime
)
```

客户交往记录 t_customer_contact

```
id
cus_id
contact_time
address
overview
create_date
update_date
is_valid
```

```sql
create table t_customer_contact(
    id int(11),
    cus_id int(11),
    contact_time datetime,
    address varchar(500),
    overview varchar(100),
    create_date datetime,
    update_date datetime,
    is_valid int(4)
)
```

客户订单 t_customer_order

```
id
cus_id
order_no
order_date
address
state
create_date
update_date
is_valid
```

```sql
create table t_customer_order (
    id int(11),
    cus_id int(11),
    order_no varchar(40),
    order_date datetime,
    address varchar(500),
    state int(4),
    is_valid int(4),
    create_date datetime,
    update_date datetime
)
```

客户订单详情 t_customer_details

```
id
order_id
goods_name
goods_num
unit
price
sum
is_valid
create_date
update_date
```

```sql
create table t_order_detail(
    id int(11),
     order_id int(11),
     goods_name varchar(100),
     goods_num int(11),
     unit varchar(20),
     price decimal(10,2),
     sum int(11),
     is_valid int(4),
     create_date datetime,
     update_date datetime
)
```

客户流失表 t_customer_loss

```
id
cus_no
cus_name
cus_manager
last_order_time
confirm_loss_time
state
loss_reason
is_valid
create_date
update_date
```

```sql
create table t_customer_lose(
    id int(11),
     cus_no int(11),
     cus_name varchar(20),
     cus_manager varchar(20),
     last_order_time datetime,
     confirm_loss_time datetime, 
     state int(4),
     loss_reason varchar(1000),
     is_valid int(4),
     create_date datetime,
     update_date datetime
 )
```

客户暂缓表 t_customer_reprieve

```
id
loss_id
measure
is_valid
create_date
update_date
```

```sql
create table  t_customer_reprieve(
    id int(11),     
    loss_id int(11),
     measure varchar(500),
     is_valid int(4),
     create_date datetime,
     update_date datetime
)
```

客户服务表 t_customer_serve

```
id
serve_type
overview
customer
state
service_rquest
create_people
assigner
assgn_time
service_proce
service_proce_pople
service_proce_time
service_proce_result
myd
is_valid
update_date
create_date
```

```sql
create table  t_customer_serve(
    id int(11),
     serve_type varchar(30),
     overview varchar(500),
     customer varchar(30),
     state varchar(20),
     service_rquest varchar(500),
     create_people varchar(100),
     assigner varchar(100),
     assgn_time datetime,
     service_proce varchar(500),
     service_proce_pople varchar(20),
     service_proce_time datetime,
     service_proce_result varchar(500),
     myd varchar(50),
     is_valid int(4),
     update_date datetime,
     create_date datetime
)
```

用户表 t_user

```
id
user_name
user_pwd
true_name
email
phone
is_valid
create_date
update_date
```

```sql
create table t_user(
    id int(11),
     user_name varchar(20),
     user_pwd varchar(50),
     true_name varchar(20),
     email varchar(30),
     phone varchar(20),
     is_valid int(4),
     create_date datetime,
     update_date datetime
)
```

角色表 t_role

```
id
role_name
role_remark
is_valid
create_date
update_date
```

```sql
create table t_role(
    id int(11),
     role_name varchar(50),
     role_remark varchar(200),
     is_valid int(4),
     create_date datetime,
     update_date datetime
)
```

用户角色 t_user_role

```
id
user_id
role_id
is_valid
create_date
update_date
```

```sql
create table t_user_role(
    id int(11),
     user_id int(11),
     role_id int(11),
     is_valid int(4),
     create_date datetime,
     update_date datetime
)
```

模块表 t_module

```
id
module_name
module_style
url
parent_id
parent_opt_value
gradle
opt_value
orders
is_valid
create_date
update_date
```

```sql
create table t_module (
    id int(4),
     module_name varchar(100),
     module_style varchar(100),
     url varchar(200),
     parent_id int(11),
     parent_opt_value varchar(100),
     grade int(11),
     opt_value varchar(200),
     orders int(11),
     is_valid int(4),
     create_date datetime,
     update_date datetime
)
```

权限表t_permission (角色模块关联表)

```
id
role_id
module_id
acl_value
is_valid
create_date
update_date
```

```sql
create table t_permission(
    id int(11),
     role_id int(11),
     module_id int(11),
     acl_value varchar(200),
     is_valid int(4),
     create_date datetime,
     update_date datetime
)
```

字典表  t_datadic

```
id
data_dic_name
data_dic_value
is_valid
create_date
update_date
```

```sql
create table t_datadic(
    id int(14),
     data_dic_name varchar(50),
     data_dic_value varchar(50),
     is_valid int(4),
     create_date datetime,
     update_date datetime
)
```



日志表  t_log

```
id
description
method
type
request_ip
exception_code
exception_detail
params
create_date
execute_time
create_man
result
```

```sql
create table t_log(
    id int(11),
     description varchar(250),
     method varchar(250),
     type varchar(20),
     request_ip varchar(50),
     exception_code varchar(50),
     exception_detail varchar(50),
     params varchar(500),
     create_date datetime,
     execute_time datetime,
     create_man varchar(200),
     result longtext
)
```



### 5 项目环境搭建与测试

#### 5.1 项目技术栈

前端 LayUI +  Fremarker 

数据库 MySQL

JDK：Java8

后端框架：SpringBoot（Mybatis3 Spring5.x SpringMVC）

#### 5.2 项目搭建与测试

##### 5.2.1 Idea下新建SpringBoot项目crm

##### 5.2.2 pom.xml引入坐标依赖

```
spring-boot-starer-paremt 2.2.2

spring-boot-starer-web

spring-boot-starer-aop

spring-boot-starer-freemarker

spring-boot-starer-test

mybatis-spring-boot-starer  2.1.1

pagehelper-spring-boot-starer   1.2.13

mysql-connector-java 

c3p0  0.9.5.5

common-lang3

fastjson  1.2.47

spring-boot-devtools

编译插件
代码生成工具
spring-boot-maven-plugin
```

##### 5.2.3 添加配置文件

```

## mybatis

## pageHelper

## 设置dao层日志打印级别

```



##### 5.2.4 添加视图转发源代码

IndexController

```java
public class IndexController {
	
    //系统登录页
    public String index(){
        
    }
    
    //欢迎页
    public String welcom(){
    }
        
	//后端管理主页面
    public String index(){
    }
}
```



##### 5.2.5 静态资源文件目录添加

 resources/public 金塔资源文件

##### 5.2.6 添加系统登录 主页面视图模板页

resources下 新建 view 添加index.ftl main.ftl

##### 5.2.7 添加SpringBoot应用启动类Starer



##### 5.2.8 项目结构

##### 5.2.9 浏览器访问登录页 主页



### 6 用户登录功能实现



### 13 基础信息模块-用户登录-前端代码实现

```
设置用户登录状态
1 利用session
   保存用户信息，如果会话存，则用户时登录状态，否则是非登录状态
2 利用cookie
  
```

```
$.cookie("userId",result.result.userId);
$.cookie("userName",result.result.userName);
$.cookie("trueName",result.result.trueName);
```

