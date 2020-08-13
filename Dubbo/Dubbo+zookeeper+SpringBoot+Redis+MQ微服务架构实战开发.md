# Dubbo+zookeeper+SpringBoot+Redis+MQ微服务架构实战开发



> 视频地址：https://www.bilibili.com/video/BV1VJ411g7Zq?t=53

## P17 设计表需要注意什么

#### 1、项目开发流程



#### 2、开发阶段规范

##### 2.1 编码规范

参照《阿里巴巴开发规范》

##### 2.2 数据库设计规范

**三大范式**

- 第一范式：列不可分，原子性
  - 反例：地址 广东省 深圳市，地址可以再进行拆分
- 第二范式：要有主键，每条记录要有唯一性
  - 主键建议是自增列（前提是：没有做分库分表）
  - 切记：**不用用uuid作为主键**
- 第三范式：不用存在传递依赖（不浪费磁盘空间）
  - 存在传递依赖
    - t_studen(id,name,class_id,class_name)
    - t_class(id,name...)
  - 不存在传递依赖
    - t_studen(id,name,class_id)
    - t_class(id,name...)

**反范式**

- 反的是第三范式

  - 1、考虑的通过空间换时间  浪费一点磁盘空间，加快单表查询速度

    例如商品表（id,name, type_id,type_name）

    商品类别表（type_id,type_name）

    时间业务较大考虑：异步都用上商品类别名称

    单表查询速度 ——>优于   多表关联查询速度

  - 带来的坏处 维护变得复杂

    数据表更

    以前：商品类别名称发送了变化，只需要修改商品类别表

    现在：要多维护一张表的数据，商品表中的类别名称也需要进行同步维护

  - 结论：

    查询的速度变快了，更新的速度编码了

    使用场景：读多写少

  - 2、业务场景的需要，历史快照

    举例：订单表（id,order_no, user_address_id,phone,reciver）

    用户-地址 t_user_address(id,address)

    用户地址会随时变更，不能只用id管理查询，需要记录每次的地址信息

**不用减了物理外键，建立逻辑外键**

	- 物理外键带来的的好处是：数据的正确性一致性，从数据库层面去控制
	- 物理外键带来的坏处是：性能问，每次你对数据的操作都要做检查 
 - 建立逻辑外键来解决正确性的问
   	- 通过程序的手段来实现
   	- 录入商品，选择商品分类

**字段的选择上，尽量考虑合适即可**

​		mysql 五百万

- 整形
  - int
  - bigint
  - tinyint
  - 例如：年龄用tinyint足够 用int浪费空间 
  - int(3)  int(32) 占用的空间是一样的 int(3) 可以存入 int(32)的数字
- ​	字符串
  - varchar
  - char
  - 定长用char 如 密码 手机号码
  - 不定长用varchar
- **浮点型** **（重点面试题）**
  - 忘记fload dubbo
  - 使用bigint decimal
  - 比如不要求严谨的可以使用fload dubbo 比如微博积分
  - 金额 很严谨的 不能使用fload dubbo，fload dubbo 精度有限，很容易出问题
  - 金额 存入 bigint ，存入之前先乘以100 取出时 除以100
  - decimal 是支持小数点 并且是精确的



## P18 搭建项目开发骨架

#### 1、项目技术栈

技术栈：Dubbo+SpringBoot+SpringMVC+Spring+MyBatis+MySQL+JQuery+Thymeleaf

分布式文件系统：FastDFS（淘宝）

搜索引擎：Solr

分布式缓存：Redis

消息中间件：RabbitMQ

#### 2、项目整体架构

```
qf-v7
	qf-v7-api
		qf-v7-product-api
	qf-v7-basic
		qf-v7-common
		qf-v7-entity
		qf-v7-mapper
	qf-v7-service
	qf-v7-temp
	qf-v7-web
```

```
dubbo-mall
	dubbo-mall-api
		dubbo-mall-product-api
	dubbo-mall-basic
		dubbo-mall-common
		dubbo-mall-entity
		dubbo-mall-mapper
	dubbo-mall-service
	dubbo-mall-temp
	dubbo-mall-web
		dubbo-mall-background
```

## P19 使用PowerDesigner设计表

#### 数据库表的设计

商品信息

​	id name price sale_price sale_point stock images desc +5个基本自段

​	垂直拆分 

​	商品的基本信息 	id name price sale_price sale_point stock images

​	商品的描述 id desc  product_id

商品类别

​	id，pid,name,desc,

#### PowerDesigner使用

1、创建PhysicalDataMode物理模型

2、选择数据库DBMS类型 MySQL

3、为模型命名dubbo-mall

4、工作区右侧palette为调色板/工具箱

5、New新建-> physical Diagram 物理图表；可以通过创建多个物理图表来进行数据库模块拆分，避免表太多拥挤在一起

**6、导出SQL**菜单栏Database - Generation Database

7、Navicat 在数据库上 右键 运行SQL 选择导出的SQL执行 创建表



## P20 实现entity+mapper层