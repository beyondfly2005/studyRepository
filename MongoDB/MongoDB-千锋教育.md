## 学会MongoDB 玩转API接口

### 课程介绍

#### 第一章：MongoDB基础

> 数据库简介
>
> MongoDB简介
>
> MongoDB安装
>
> MongoDB基本操作
>
> MongoDB文档增删改查CRUD
>
> MongoDB实战教学管理数据库设计

#### 第二章：MongoDB高级

> MongoDB分页&排序
>
> MongoDB聚合查询
>
> MongoDB优化索引
>
> MongoDB权限机制
>
> MongoDB备份还原
>
> 实战可视化管理工具

#### 第三章：玩转API接口

> mongoose简介(Schema&model)
>
> mongoose使用
>
> 接口概念
>
> 接口开发规范(Restful API)
>
> 接口测试工具(Postman&Insomnia)
>
> 实战教学管理系统学生模块接口开发
>
> 实战接口文档开发apiDoc

### MongoDB教学目标

> 能够说出数据库的作用和种类
>
> 能够独立完成数据库设计
>
> 能够独立完成MongoDB数据库CURD
>
> 能够实现

## 第一章 MongoDB基础



### 1.2 MongoDB简介

#### 1.2.1 什么是MongoDB

#### 1.2.1 MongoDB能做什么

- 存放项目数据
- 实战

#### 1.2.2 MongoDB下载

windows版本下载 https://www.mongodb.org/dl/win32

linux版本下载 https://www.mongodb.org/dl/linux

https://www.mongodb.com/download-center/community

版本：

- 2.x
- 3.x
- 4.x

### 1.3 MongoDB安装

#### 1.3.1 Linux环境安装

> mac系统 同linux安装 只需要在命令前面加sudo

```bash
#步骤1 下载
curl -o https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz
#步骤2 解压
tar -zxvf mongodb-linux-x86_64-3.0.6.tgz
#步骤3 将解压包拷贝到指定目录
mv mongodb-linux-x86_64-3.0.6 /usr/local/mongodb
#步骤4 创建数据库存放目录与日志存放目录
mkdir -p /usr/local/mongodb/data /usr/local/mongodb/logs
#步骤5 启动MongoDB服务
/usr/local/mongodb/bin/mongod --dbpath=/usr/local/mongodb/data --logpath=/usr/local/mongodb/logs/mongodb.log --logappend --port=27017 --fork

#后期登录即可
/usr/local/mongodb/bin/mongo 
# 退出
exit
```

默认配置下 虚拟机内的MongoDB将不能被其他客户端访问，需要进行配置才行

```bash
# 创建conf目录
mkdir conf
# 新建 mongod.conf文件
cd conf
touch mongod.conf
# 修改配置文件
vim mongod.conf
```

写入以下内容

```yaml
systemLog:
   #MongoDB发送所有日志输出的目标指定为文件
   # #The path of the log file to which mongod or mongos should send all diagnostic logging information
   destination: file
   #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
   path: "/usr/local/mongodb/logs/mongodb.log"
   #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。 
   logAppend: true
storage:
   #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
   ##The directory where the mongod instance stores its data.Default Value is "/data/db".
   dbPath: "/usr/local/mongodb/data"
   journal:
      #启用或禁用持久性日志以确保数据文件保持有效和可恢复。
      enabled: true
processManagement:
   #启用在后台运行mongos或mongod进程的守护进程模式。
   fork: true
net:
   #服务实例绑定的IP，默认是localhost
   #想让其他机器访问，需要绑定局域网ip
   bindIp: localhost,192.168.0.110 
   #bindIp
   #绑定的端口，默认是27017
   port: 27017
```

启动

```bash
/usr/local/mongodb/bin/mongod -f /usr/local/mongodb/conf/mongod.conf

#或者
cd /usr/local/mongodb/bin
mongod ../conf/mongod.conf
```

通过客户端工具如 Studio 3T for MongoDB 或者 NoSQL manager for MongoDB 连接不上的话

可以尝试查看防火墙状态

```bash
# 查看防火墙状态 
# 如果执行以下命令后 显示active（running）”，说明防火墙已经启动  如果显示disavtive（dead） 说明防火墙 没有启动
systemctl status firewalld.service

# 关闭防火墙
systemctl stop firewalld.service

# 永久关闭防火墙
systemctl disable firewalld.service
```



#### 1.3.2 Window环境安装

步骤1： 下载  https://www.mongodb.com/download-center/community

步骤2： 解压

步骤3：创建服务

```
bin/mongod.exe --install --ddpath 磁盘路径 --logpath 日志路径
bin/mongod.exe --install --ddpath e:\mongodb\data --logpath e:\mongodb\logs
```

### 1.4 MongoDB基本操作

##### 1.4.1 基本概念

- 生活中：仓库 货架 物品
- 计算机：数据库(database) 集合(collection) 数据/文档(document)

##### 1.4.2 查看数据库

​	语法：show databases

​	默认有三个数据库：admin local config

##### 1.4.3 选择数据库

​	语法：use 数据库名

​	例如：ues admin

​	隐式创建：选择不存在的数据库不会保错，后期当该数据库有数据时，系统会自动创建。

##### 1.4.3 查看集合

​	语法：show collections



##### 1.4.4 创建集合

​	语法：db.createCollection("集合名称")

​	示例：db.createCollection("c1")

##### 1.4.5 删除集合

​	语法：db.集合名称.drop()

​	示例：db.c1.drop();

##### 1.4.6 小结

​	删除数据库：use c1;  db.dropDatabase()

​	数据库：查看、 创建、选择、删除	

```
查看：show databases
创建：有单独的语法，但是通常使用隐式创建
选择：use 数据库名
删除：1-选择数据库： use 数据库；
	 2-删除所选库： db.dropDatabase()
```

​	集合：查看、创建删除

```
查看：show collections
创建：db.createCollection("集合名")  通常使用隐式创建
删除：db.集合名称.drop()
```

### 1.5 MongoDB文档增删改查

#### 1.5.1 明确需求

#### 1.5.2 Create增加

语法：db.集合名称.insert(JSON数据)

说明：集合存在则直接插入数据，集合不存在则隐式创建集合 再插入数据

示例：

```
use test2
db.c1.insert({uname:"zhangsan",age:18})
```

- 数据库或集合不存在会隐式创建
- 对象的键key 统一不佳引号方便查看，查询集合数据时，系统自动增加引号
- mongodb会给每条数据增加一个_id的键 作为主键
- _id 的构成 时间戳4位+机器码3位+PID两位+计数器3位

**思考一：**是否可以自定义 _id 可以，但不推荐

**思考二：**如何一次性插入多条记录

```
db.c1.insert([
{uname:"zhangsan",age:18}，
{uname:"lisi",age:18}，
{uname:"wangwu",age:18}
])
```

**思考三：**一次性插入10条数据

```javascript
use test1
for(var u=1; i<=10;i++){
  db.c2.insert({uname:"a"+i,age:i})
}
```



#### 1.5.3 Retrieve查询

基础语法：db.集合名称.find(条件,[查询的列])

```
条件
	查询所有数据 {}或者不写
	查询age=6的数据 {age=6}
	既要age=6并且姓名=男  {age=6,sex:'男'}
查询的列（可选参数）
	不写参数 - 查询全部列(字段)
	{age:1}  只显示age列(字段)
	{age:0}  除了age的列/字段都显示
	注意：默认情况下，系统自定义的_id都会显示
```

示例：

```
# 查询所有数据
db.c1.find() 
db.c1.find({})

# 查询年龄大于5岁的数据
db.c1.find({age:{$gt:5}})

# 查询年龄是5岁、8岁、10岁的数据
db.c1.find({age:{$in:[5,8,10]}})

#只查询年龄列
db.c1.find({},{age:1})

#查询除年龄之外的列
db.c1.find({},{age:0})
```

升级语法：

```
db.集合名.find({key:value})
db.集合名.find({键:值})
				将值 替换为{运算符:值}	
db.集合名.find({键:{运算符:值}})
```



| 运算符 | 作用     |
| ------ | -------- |
| $gt    | 大于     |
| $gte   | 大于等于 |
| $lt    | 小于     |
| $lte   | 小于等于 |
| $ne    | 不等于   |
| $in    | in       |
| $nin   | not in   |
| $eq    | 等于     |



#### 1.5.4 Update修改

**基础语法：**db.集合名称.update(条件 , 新数据 , [是否新增] , [是否修改多条])

```
是否新增：指条件匹配不到数据则插入 true 插入，false不插入(默认为不插入)
是否修改多条：匹配成功的数据都修改 ture 都修改 false 只修改第一条（默认值false）
```

**升级语法**

```
db.集合名称.update(条件 , 新数据)
					    {修改器: {键：值}} 替换新数据
db.集合名称.update(条件 , {修改器: {键：值}})
```

**修改器**

| **修改器** | **作用**   |
| ---------- | ---------- |
| $inc       | 递增       |
| $rename    | 重命名列   |
| $set       | 修改列值   |
| $unset     | 删除列     |
| $rename    | 修改字段名 |



**准备工作**

```javascript
use test2
for(var u=1; i<=10;i++){
  db.c2.insert({uname:"zs"+i,age:i})
}
```

**示例**

```bash
#将{uname:"zs1"} 改为{uname:"zs2"}
db.c2.update({uname:"zs1"},{uname:"zs2"});
db.c2.find()
# 默认不是修改 而是替换，只修改一个属性，会把其他属性丢失

#应使用修改器来修改数据，如下
db.c2.update({uname:"zs2"} , {$set: {uname:"zs22"}})

# 将{uname:"z10"}的年龄增加2岁或减少2岁
db.c2.update({uname:"zs10"} , {$inc: {age:2}})
db.c2.update({uname:"zs10"} , {$inc: {age:-2})

# 插入数据db.c4.insert({uname:"神龙教主",age:888,who:"男",other:"非国人"})
#uname 神龙教主    (修改器 $set )
#age 增加111		(修改器 $inc)
#who 改为sex		(修改器 $rename)
#other 删除		(修改器 $unset)
db.c4.update({uname:"神龙教主"},{$set:{uname:"神龙教主"}});
db.c4.update({uname:"神龙教主"},{$inc:{age:111}});
#db.c4.update({uname:"神龙教主"},{$set:{sex:"男"},$unset:{who:"男"}});
db.c4.update({uname:"神龙教主"},{$rename:{who:"sex"}});
db.c4.update({uname:"神龙教主"},{$unset:{other:"男"}});

#合并以上 多个修改器的同时使用，一次执行
db.c4.update({uname:"神龙教主"},{
	$set:{uname:"神龙教主"},
	$inc:{age:111},
	$rename:{who:"sex"},
	$unset:{other:"男"}
});
```

```bash
# 第三个参数：找不到新增
# 修改不存在的数据
# 修改不存在的数据，不存在则不修改
db.c4.update({uname:"张三丰"},{$set:{uname:"张四丰"}});
db.c4.update({uname:"张三丰"},{$set:{uname:"张四丰"}},false);

# 修改不存在的数据，不存在则作为新数据插入
db.c4.update({uname:"张三丰"},{$set:{uname:"张四丰"}},true);

```

```bash
# 第四个参数：是否修改多条
# 第四个参数为false默认值，只修改匹配到的第一条数据
db.c3.update({},{$set:{age:30}});
db.c3.update({},{$set:{age:30}},false,false);

# 修改符合条件的多条数据 第四个参数为true,匹配多条
db.c3.update({},{$set:{age:30}},false,ture);
```



#### 1.5.5 Delete删除

语法：db.集合名.remove(条件, [是否删除一条])

```
参数：是否删除一条，ture  只删除一条
				false 删除匹配到的多条记录，默认值是false
```

示例

```bash
db.c3.find();

#只删除一条
db.remove({},true);
# 删除多条
db.remove({});
db.remove({},false);
```



#### 1.5.6 小总结

**增 Create**

```
db.集合名.insert(JSON数据)
```

**删 Delte**

```bash
db.集合名.remove(条件,[是否删除一条true,false删除多条 默认值是false])
# 默认删除多条
```

**改 Update**

```
db.集合名.update(条件,新数据,[是否新增],[是否修改多条])
# 升级语法
db.集合名.update(条件,{修改器:{键, 值} })
```

**查 Read/Retrieve**

```
db.集合名.find(条件,[查询的列])
```



### 1.6 MongoDB实战教学管理数据库设计

#### 1.6.1 实战开发流程

​    1、产品经理（PM）：做需求分析和原型设计

​    2、UI设计师： 美化需求设计、交互设计 生成UI设计图

​    3、开发工程师：确定系统架构、程序设计（数据库设计、接口设计、代码编写）

​	    前后端分离

​	    后端：数据库设计、接口设计

​    	前端：静态页面、交互页面开发、调用后端接口

​    4、前后端联调

​	    模块测试

​    5、系统测试

​    6、上线部署

​    7、线上测试

​    8、功能交付

#### 1.6.2 数据库设计流程

根据UI设计稿

1、 确定功能模块所属集合

2、确定集合字段

```
UI设计稿每个战士内容对应一个字段
创建时间字段
更新时间字段
状态字段

设计表：可以先使用中文，随着逐步熟悉，再使用英文字段设计
```

#### 1.6.3 练习

需求：根据教学系统，设置存放学生信息的集合，并插入20条测试数据

代码：

```javascript
#1、先中文 根据UI设计图的元素
#编号、学号、姓名、姓名、电话、性别、年龄、学历、备注
#2、再设计英文字段
use school
for(var num=1;num<20;num++){
	db.student.insert({
		id:num,
		no: "QF"+num,
		uname: "神龙教"+num,
		tel:11111,
		sex:"女",
		age: num,
		school:"研究生",
		remarks: "备注"
	});
}
```

```bash
# 格式化显示数据
db.student.find().pretty()
```



## 第二章 MongoDB高级

### 2.1 MongoDB分页&排序

#### 2.1.1 明确需求

数据库是存放数据的

从数据库获取的数据 需要排序、多页显示、分页显示 如何实现？

#### 2.1.2 准备数据

```
use test3
db.c1.insert({_id:1,name:"a",set:1,age:1})
db.c1.insert({_id:2,name:"b",set:1,age:2})
db.c1.insert({_id:3,name:"c",set:1,age:3})
db.c1.insert({_id:4,name:"d",set:1,age:4})
db.c1.insert({_id:5,name:"e",set:1,age:5})
db.c1.find()
```

#### 2.1.3 排序

- 语法：db.集合名.find().sort(JSON数据)

- 说明：Json数据的键 是要排序的字段，值：1升序 -1降序

- 练习：按年龄升序 降序排序查询

  ```bash
  use test3
  db.c1.find().sort({age:1})
  db.c1.find().sort({age:-1})
  ```

  

#### 2.1.4 limit与skip方法

- 语法：db.集合名.find().skip(数字).limit(数字)

- 说明：skip 指定数量 limit 限制查询的数量

- 练习

  ```bash
  # 降序查询2条数据
  db.c1.find().sort({age:-1}).limit(2)
  db.c1.find().sort({age:-1}).limit(2).skip(0)
  # 降序跳过2条并查询2条 （=每页2条 查询第二页的数据）
  db.c1.find().sort({age:-1}).limit(2).skip(2)
  ```

  

#### 2.1.5 实战分页

- 需求：数据库1-10条数据，每页显示2条

- 分页：

  ```bash
  #第1页
  db.c1.find().sort({age:-1}).limit(2).skip(0)
  #第2页
  db.c1.find().sort({age:-1}).limit(2).skip(2)
  db.c1.find().sort({age:-1}).limit(2).skip(4)
  db.c1.find().sort({age:-1}).limit(2).skip(6)
  #第5页
  db.c1.find().sort({age:-1}).limit(2).skip(8)
  
  skip的值 = (当前页-1)* 每页显示条数
  ```

#### 2.1.6 小总结

​	

```
db.集合名.find()
	.sort({列: 1/-1})  #排序
	.skip(数字)  #跳过指定的数量
	.limit(数字) 限制查询条数
	.count() 统计总数量
	
```

### 2.2 MongoDB聚合查询

##### 2.2.1 明确需求

思考：如何统计数据，如何实现分组统计等？

解决：数据统计通过find().count()可以实现，分组统计通过MongoDB的聚合查询实现。

##### 2.2.2 概念

聚合查询：就是把数据聚集起来，然后统计

##### 2.2.3 语法

- 语法

```
db.集合名.aggregate([
	{管道:{表达式}}
	...
])
```

- 常用管道

```
$group	将集合中的文档分组，用于统计结果
$match	过滤数据，只要输出符合条件的文档
$sort	聚合数据进一步排序
$skip	跳过指定文档数
$limit	限制集合数据返回文档数
... 
```

- 常用表达式

```
$sum  总和  $sum:1 同count表示统计
$avg  平均
$min  最小值
$max  最大值
```

##### 2.2.4 准备数据

```
use test4
db.c1.insert({_id:1,name:"a",sex:1,age:1})
db.c1.insert({_id:2,name:"b",sex:1,age:2})
db.c1.insert({_id:3,name:"c",sex:2,age:3})
db.c1.insert({_id:4,name:"d",sex:2,age:4})
db.c1.insert({_id:5,name:"e",sex:2,age:5})
```

##### 2.2.5 练习

- 统计男生、女生的总年龄

```
db.c1.aggregate([
	{$group:{
		_id: "$sex",
		rs:{$sum:"$age"}
	}}	
])
```

- 统计男生女生的总人数

```
db.c1.aggregate([
	{$group:{
		_id: "$sex",
		rs:{$sum:1}
	}}
])
```

- 求学生总数和平均年龄

```
db.c1.aggregate([
	{$group:{
		_id: null,
		total_num:{$sum:1},
		total_avg:{$avg:"$age"},
	}}	
])
```

- 查询男生、女生人数，按人数升序

```
db.c1.aggregate([
	{$group:{
		_id: "$sex",
		num:{$sum:1}		
	}},
	{$sort:{num: 1}}
])
```



### 2.3 MongoDB优化索引

#### 2.3.1 生活中的索引

公交车站 站牌

字典 目录、文件目录

写字楼/办公区域 公司门牌索引

#### 2.3.2 数据库中的索引

- 说明：索引是一种排序好的便宜快速查询的数据结构
- 作用：帮助数据库高效的查询数据

#### 2.3.3 索引优缺点

- 优点

  提高数据查询的效率，降低数据库的IO成本

  通过索引对数据库进行排序，降低数据排序的成本，降低CPU的消耗

- 缺点

  占用磁盘空间

  大量索引影响SQL语法效率，因而为每次插入和修改数据都需要更新索引

#### 2.3.4 语法

- 语法：db.集合名.createIndex(待创建索引的列, [额外选项])

- 参数

  ```
  待创建索引的列：{键:1 ...,键:-1}
  说明：1升序， -1降序，例如{age:1}表示创建age索引并按照升序的方式存储
  额外选项：设置索引的名称或者唯一索引等等
  ```

- 删除索引语法

  ```bash
  # 删除全部索引
  db.集合名.dropIndexes();
  
  # 删除指定索引
  db.集合名.dropIndex(索引名)
  ```

- 查看索引语法

  ```
  db.集合名.getIndexes()
  ```

#### 2.3.5 练习

##### **准备数据**

向数据库中新增十万条数据

```
use test5;
for(var i=1;i<=100000;i++){
	db.c1.insert({name:"aaa"+i,age:i});
}
```

##### **创建普通索引**

```bash
# 给name添加普通索引
db.c1.createIndex({name:1})
# 删除name索引
db.c1.dropIndex("name_1")

db.c1.getIndexes()
# 给name创建索引，并将索引命名为 idx_name
db.c1.createIndex({name:1},{name:"idx_name"})
# 删除这个索引
db.c1.dropIndex("idx_name")
```

##### **创建复合/组合索引**

语法：db.集合名.createIndex({键1:排序方式 , 键2 : 排序方式}, [额外选项])

```bash
# 组合索引：由多个字段组成的索引 称为组合索引或复合索引
# 给name和age添加组合索引
db.c1.createIndex({name:1,age:1})
# 或
db.c1.createIndex({name:1,age:1},{name:"idx_nameAndAge"})
```

##### **创建唯一索引**

语法：db.集合名.createIndex({索引列:排序方式 }, {unique: 列名})

```bash
# 删除全部索引  系统索引不会被删掉
db.c1.dropIndexes();

# 设置唯一索引
db.c1.createIndex({name:1}, {unique:"name"})

# 测试唯一索引 不能重复插入
db.c1.insert({name:"zhangsan",age:25})
db.c1.insert({name:"zhangsan",age:25})
```

 重复插入 提示错误

```
"writeError" : {
		"code" : 11000,
		"errmsg" : "E11000 duplicate key error index: test5.c1.$name_1 dup key: { : \"zhangsan\" }"
	}
```



#### 2.3.6 分析索引(explain)

语法：db.集合名.find().explain('executionStats')

说明：

```json
{ 
    "queryPlanner" : {
        "plannerVersion" : 1.0, 
        "namespace" : "test5.c1", 
        "indexFilterSet" : false, 
        "parsedQuery" : {
            "$and" : [

            ]
        }, 
        "winningPlan" : {
            "stage" : "COLLSCAN", 
            "filter" : {
                "$and" : [

                ]
            }, 
            "direction" : "forward"
        }, 
        "rejectedPlans" : [

        ]
    }, 
    "executionStats" : {					//执行查询计划相关的统计信息
        "executionSuccess" : true, 			//执行成功的状态
        "nReturned" : 100002.0, 			//返回结果集数目
        "executionTimeMillis" : 15.0, 		//执行所需时间 单位：毫秒
        "totalKeysExamined" : 0.0, 			//索引检查的时间
        "totalDocsExamined" : 100002.0, 
        "executionStages" : {
            "stage" : "COLLSCAN", 			//索引扫描方式 **
            "filter" : {					//过滤方式
                "$and" : [

                ]
            }, 
            "nReturned" : 100002.0, 
            "executionTimeMillisEstimate" : 10.0, 
            "works" : 100004.0, 
            "advanced" : 100002.0, 
            "needTime" : 1.0, 
            "needFetch" : 0.0, 
            "saveState" : 781.0, 
            "restoreState" : 781.0, 
            "isEOF" : 1.0, 
            "invalidates" : 0.0, 
            "direction" : "forward", 
            "docsExamined" : 100002.0
        }
    }, 
    "serverInfo" : {
        "host" : "localhost.localdomain", 
        "port" : 27017.0, 
        "version" : "3.0.6", 
        "gitVersion" : "1ef45a23a4c5e3480ac919b28afcba3c615488f2"
    }, 
    "ok" : 1.0
}
```

索引扫描方式：

- COLLSCAN 全表扫描  较慢
- IXSCAN 索引 扫描
- FETCH 根据索引去检索指定的document

**练习**

```bash
# 对比未添加索引 与 添加索引后 查询效率
db.c1.find({age:18}).explain('executionStats');
# 未加索引之前
# "stage" : "COLLSCAN", 
# "totalDocsExamined" : 100002.0, 
# "executionTimeMillis" : 29.0, 

# 添加索引
db.c1.createIndex({age:1},{name:"idx_age"});
# 再次查询执行计划
db.c1.find({age:18}).explain('executionStats');

# 添加索引之后 效率对比
# "stage" : "IXSCAN", 
# "executionTimeMillis" : 0.0, 
# "totalDocsExamined" : 1.0, 
```



#### 2.3.7 选择规则 

添加索引的规则：什么时候需要添加索引，什么时候不添加索引

- 为经常需要做条件查询、排序、分组的字段添加索引
- 选择唯一向索引（PS:不同值较少如性别字段 不建议加索引，同值较少的 建议加）
- 选择较小的数据列，为较长的字符串使用前缀索引 （PS：索引文件更小，索引占用磁盘空间）

### 2.4 MongoDB权限机制

#### 2.4.1 需求

在命令窗口或者使用工具连接到 MongoDB，默认不需要输入账号、密码，直接就可以连接，这在实际工作，有且线上系统中是绝对被不允许的

使用权限机制，开启验证模式即可

#### 2.4.2 语法

**创建账号**

```
db.createUser({
	"user":"账号",
	"pwd":"密码",
	"roles":[{
		role:"角色",
		db:"所属数据库"
	}]
})
```

**角色**

```bash
# 角色种类
超级管理员角色：root
数据库用户角色：read、readWrite
数据库管理角色：dbAdmin、userAdmin
集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManger
备份恢复角色：backup、restore
所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminANyDatabase、dbAdminAnyDatabase

# 角色说明

```



#### 2.4.3 开启验证模式

##### **开启验证模式**

开启验证模式：指的是需要输入用户名和密码才能登陆MongoDB

##### 操作步骤

```
1、添加超级管理员
2、退出卸载服务
3、重新安装需要输入密码的服务（在原安装命令基础上加上 --auth）
4、启动服务
```

###### 步骤1、添加超级管理员

```
mongo

use admin

db.createUser({
	"user":"admin",
	"pwd":"123456",
	"roles":[{
		role:"root",
		db:"admin"
	}]
})
```

```
db.createUser({
	"user":"root",
	"pwd":"123456",
	"roles":[{
		role:"root",
		db:"admin"
	}]
})
```

注意：有的版本看不到admin库，但是不代表没有这个库，直接使用即可

###### 步骤2、退出卸载服务

```
# 以管理员身份打开Dos窗口
# 找到mongoDB安装目录
bin\mongod --remove 
```



###### 步骤3、重新安装需要输入密码的服务（在原安装命令基础上加上 --auth）

```
bin\mongod --installdbpath  --logpath  --auth

net start mongodb
net stop mongodb
```



###### 步骤4、启动服务



#### 2.4.4 通过超级管理员账号登录

方法一：

```
# 方法一
mongo 服务器IP地址:端口/数据库 -u 用户名 -p 密码
mongo 127.0.0.1:27017/admin -u admin -p 123456
```

方法二：

```
1-先登录 mongo
2-选择数据库 use admin
3-输入db.auth(用户名,密码) db.admin('admin','123456')
```



#### 2.4.5 练习

##### 准备

```
use shop
for(var i=1;i<=20;i++){
	db.goods.insert({name:"good"+i,price:i});
}
```

##### 需求

```
添加用户shop1 可以读shop数据库
添加用户shop2 可以读写shop数据库
注意：必须在对应的数据库创建用户
```

##### 添加用户并设置权限

```
#添加用户shop1 可以读shop数据库
use shop
db.createUser({
	"user":"shop1",
	"pwd":"123456",
	"roles":[{
		role:"read",
		db:"shop"
	}]
})
```

```
# 添加用户shop2 可以读写shop数据库
use shop
db.createUser({
	"user":"shop2",
	"pwd":"123456",
	"roles":[{
		role:"readWrite",
		db:"shop"
	}]
})
```

```
# 查看系统用户
use admin
db.ststem.users.find() #查看用户
```

```
# 验证 shop1 可读
# 以shop1用户登录
mongo 127.0.0.1:27017/shop -u shop1 -p 123456
db.goods.insert({name:"苹果X PLUS"})
```

```
# 验证 shop2 可读可以写
# 以shop2用户登录
mongo 127.0.0.1:27017/shop -u shop2 -p 123456
db.goods.insert({name:"苹果X PLUS"})
```



### 2.5 MongoDB备份还原

#### 明确需求

实际工作中，做好数据库备份工作，否则万一数据丢失、或者数据被破坏，损失和影响都是巨大的

#### 备份数据库mongodump

##### 语法

```
mongodump -h -port -u -p -d -o
```

```
语法说明：
	-h  host主机IP
	-port port端口
	-u  user数据库用户
	-p  password数据库密码
	-d  database数据库  数据库不指定则导出全部
	-o  open 备份到指定目录
```

##### 练习

```
# 备份全部数据库
mongodump -h 127.0.0.1 -port 27017 -u amdin -p 123146 -o E:\mongodb\backup

# 备份指定数据库 shop
mongodump -h 127.0.0.1 -port 27017 -u amdin -p 123146 -d shop -o E:\mongodb\backup2\shop  #没有权限
mongodump -h 127.0.0.1 -port 27017 -u shop2 -p 123146 -d shop -o E:\mongodb\backup2\shop
```

#### 还原数据库mongorestore

##### 语法

```
mongorestore -h -port -u -p -d --drop 备份目录
```

```
	-h  host主机IP
	-port port端口
	-u  user数据库用户
	-p  password数据库密码
	-d  database数据库 
	--drop 先删除数据库再导入，如不写 则覆盖已有数据
```

##### 练习

- 还原全部数据库

```
# 先删除几个数据库，这样导入才能看到效果
# 注意：不要删除admin数据库
use test1
db.dropDatabase()
use test2 
db.dropDatabase()
use test3 
db.dropDatabase()

mongorestore -u admin -p 123456 --drop E:\mongodb\backup
mongorestore -h 127.0.0.1 -port 27017 -u admin -p 123456 --drop E:\mongodb\backup
```

- 还原指定数据库

```
# 注意：不能使用admin ，只能使用这个库的用户，并且需要有写权限
# 注意：还原指定数据库，需要指定 这个的数据库所在的具体目录（多一级子目录）
mongorestore -u admin -p 123456 -d shop --drop E:\mongodb\backup2\shop
```



### 2.6 实战可视化管理工具

##### 简介

常用的可视化管理工具

- AdminnMongo  WEB网页管理  https://adminmongo.markmoffat.com
- Robo 3T      客户端软件    https://robomongo.org/download
- Studio 3T for MongoDB
- MongoVUE 客户端软件

##### 安装Robo 3T

​	按提示 下一步 下一步 安装即可

##### 使用Robo 3T

​	连接数据库 输入ip地址 和端口

​	注意：如果开启了权限验证，需要添加用户名、密码



## 第三章：玩转API接口

### 3.1 mongoose简介(Schema&model)

#### 明确需求

1- 为什么要学习数据库：

​	答：存放项目或网站的数据

2- 项目也是使用cmd敲命令吗

​	答：项目中是通过程序来连接数据库的

3- 如何通过程序连接数据库

​	答：使用mongoose

#### mongoose是什么

node中提供操作MongoDB的模块

#### mongoose能做什么

​	mongoose能够通过node语法实现MongoDB数据库的增删改查

​	从而实现用node来写程序来管理MongoDB数据库

#### mongoose下载安装

​	英文网：http://mongosejs.com

​	中文网：http://mongosejs.net

​	在nodejs环境下，使用npm 或者yarn来下载安装

```bash
npm i mongoose
yarn add mongoose #推荐yarn 下载更快
```

#### schema

schema 是用来约束MongoDB文档数据（有哪些字段，哪些是必填项，字段类型，字段的默认值等待）

#### model

一个模型对应一个集合，通过模型来管理集合中的数据

#### 小总结

为什么要学习mongoose 通过程序来管理mongoDB数据

是什么：node的一个模块 用于管理mongoDB数据库

能做什么：让node借助这个模块，实现管理mongoDB数据库

下载安装：通过npm或者yarn命令来安装

mongoose的核心概念：

	- schema 约束字段/列数据
	- model 模型 对应集合 后期用他来实现数据增删改查

### 3.2 mongoose使用

#### 安装

步骤1：创建API目录

步骤2：在api目录安装mongoose模块

```
# 在 项目目录API下
yarn add mongoose
# 前提是已经安装了 yarn
```

PS：yarm的安装

```
#下载node.js，使用npm安装yarn 全局安装
npm install -g yarn
#查看版本：yarn --version
```



#### 语法

```javascript
// 一、导入模块
const mongoose = require('mongoose');
// 二、连接数据库
const db=mongoose.createConnection('mongodb://user:pass@localhost:port/database',
    {useNewUrlParser:true,useUnifiedTopolgy:true},err=>{
   if(err) {
       console.log('数据库连接失败',err);
   }
});

// 三、设置数据模型(声明式哪个集合，限制字段格式和字段类型)
const model = db.model('user',{
	name:{type:String,default:'username'},
	age:{type:Number},
	sex:{type:String}
});

// 四、创建实例操作
// 增 ----------
const insertObj = new model(数据对象)
方法1：insertObj.save((err)=> db.close())
方法2：（推荐）
insertObj.save().then(res=> {
    return res
})
.catch(err => {
    console.log('插入失败'+err)
    return false
})

// 删 ----------
//方法1：
model.remove/deleteOne/deleteMany(条件对象,(err)=> db.close())
//方法2（推荐）
model.deleteOne(条件对象).then(res=> {
    return res.deletedCount
})
.catch(err => {
    console.log('删除失败'+err)
    return false
})

// 改 ----------
//方法1：
model.update/updateOne/updateMany(条件对象,数据对象,(err)=> db.close())
//方法2（推荐）
model.updateOne(条件对象).then(res=> {
    return res.nModified
})
.catch(err => {
    console.log('修改失败'+err)
    return false
})
// 查 ----------
方法1：model.find/findOne(条件对象, 要显示的自动数据对象, (err,result) => db.close())
方法2：
model.findOne(条件对象).then(res=> {
    return res
})
 .catch(err => {
    console.log(err)
    return false
})   
```

#### 练习

增create.js

```javascript
// 一、导入模块
const mongoose = require('mongoose');
// 二、连接数据库
const db=mongoose.createConnection('mongodb://192.168.0.110:27017/shop',{useNewUrlParser:true,useUnifiedTopolgy:true},err=>{
   if(err) {
       console.log('数据库连接失败',err);
   }
});

// 三、设置数据模型(声明式哪个集合，限制字段格式和字段类型)
const model = db.model('api',{
    uname:{type:String,default:'username'},
    //pwd:{type:String},
    pwd:String,
	age:{type:Number},
	sex:{type:String}
});

// 四、创建实例操作
// 增 ----------
const insertObj = new model({
    uname: '张三',
    pwd:'123456',
    age:20,
    sex:'男'
})


insertObj.save().then(res=> {
    console.log(res)
    db.close()
    return res
})
.catch(err => {
    console.log('插入失败'+err)
    return false
})
```

运行

```bash
# 运行
node create.js  
```

查r.js

```javascript
// 一、导入模块
const mongoose = require('mongoose');
// 二、连接数据库
const db=mongoose.createConnection('mongodb://192.168.0.110:27017/shop',{useNewUrlParser:true,useUnifiedTopolgy:true},err=>{
   if(err) {
       console.log('数据库连接失败',err);
   }
});


// 三、设置数据模型(声明式哪个集合，限制字段格式和字段类型)
const model = db.model('api',{
    uname:{type:String,default:'username'},
    //pwd:{type:String},
    pwd:String,
	age:{type:Number},
	sex:{type:String}
});

// 四、创建实例操作
// 读 ----------
model.findOne({uname:"张三"}).then(res=> {
    console.log(res);
    db.close()
    return res
})
 .catch(err => {
    console.log(err)
    return false
})   
```

rPage.js 排序 分页

```javascript
// 一、导入模块
const mongoose = require('mongoose');
// 二、连接数据库
const db=mongoose.createConnection('mongodb://192.168.0.110:27017/shop',{useNewUrlParser:true,useUnifiedTopolgy:true},err=>{
   if(err) {
       console.log('数据库连接失败',err);
   }
});


// 三、设置数据模型(声明式哪个集合，限制字段格式和字段类型)
const model = db.model('api',{
    uname:{type:String,default:'username'},
    //pwd:{type:String},
    pwd:String,
	age:{type:Number},
	sex:{type:String}
});

// 四、创建实例操作
// 读 ----------
//model.findOne({uname:"张三"})
model.find().limit(1).skip(1).then(res=> {
    console.log(res);
    return res
})
 .catch(err => {
    console.log(err)
    return false
})
```

#### 总结

安装：yarn add mongoose 或者 npm install mongoose

参考文档：http://mongoosejs.com  或 http://lmongoose.net

### 3.3 接口概念

##### 3.3.2 明确需求

随着互联网的发展，客户端层出不穷，

如何做到一次编写

##### 3.3.2 接口是什么

接口是一个文件，主要用于响应JSON数据(操作方便、体积小) 或xml

##### 3.3.3 接口能做什么

数据角度：让我们的项目静态固定数据动态展示（项目数据来源于数据库）

功能角度：天气接口 地图接口 

##### 3.3.4 小总结

为什么用接口：一次编写  多次接入，减少后端工作量，方便后期维护



### 3.4 接口开发规范(Restful API)

##### 3.4.1 明确需求

##### 3.4.2 什么是Restful

##### 3.4.3 标准的Restful规范怎么做

```
订单模块
/order get请求   		 #查询列表
/order  post请求	     #创建订单
/order/编号 put请求     #修改订单
/order/编号 delte请求   #删除订单
/order
```

- 项目所有模块要有统一的标准
- 通过url就知道操作的资源是什么（哪个模块）
- 通过http method 就知道操作动作是什么？是添加post 还是删除delte
- 通过http status code就知道操作结果如何，是成功200 还是找不到404 还是内部错误500

##### 3.4.4 小总结

什么是restful API： 是一个架构或思想

作用：声明接口设计原则和约束条件

好处：统一开放规范，便于团队协作开放

### 3.5 接口测试工具(Postman&Insomnia)

##### 3.5.1 Postman是什么

Postman是一款接口测试工具

作用：模拟HTTP请求，进行接口测试，查看接口返回数据

官网地址：www.getpostman.com

​					https://www.postman.com/

##### 3.5.2 下载安装

官网地址 	https://www.postman.com/

双击运行安装程序，按引导逐步安装即可

##### 3.5.3 使用

启动postman

测试接口：https://jsonview.com/example.json

##### 3.5.4 应用场景

明确：postman是一个接口测试工具

后端用于：调试自己开发的接口，避免出错或产生bug

前端用于：1-测试接口是否可用 2-查看返回的数据

### 3.6 实战教学管理系统学生模块接口开发

#### 3.6.1 express简介

概念：express是基于nodejs开发的一个框架，原理是基于node内置http模块封装

好处：加快项目开发，便于团队协作

#### 3.6.2 express使用

安装下载：使用yarn命令安装

```bash
yarn add express
```

前提是node中，已经安装了yarn

yarn的安装

```bash
# npm安装方式yarn
npm install -g yarn

# 查看是否安装成功
yarn -v

# yarn的常用命令
# 初始化一个项目
yarn init
# 装包
yarn add packagename
yarn add packagename --dev
# 更新包
yarn upgrade packagename
# 删除包
yarn remove packagename
# 安装所有包
yarn
yarn install
# 发布包
yarn publish
# 查看包的缓存列表
yarn cache list
# 全局安装包 == npm -g
yarn global
```

**compress的使用**

新建http.js文件

```javascript
// 1. 引入express模块
const express = require('express')
// 2. 创建app对象，通过语法express 底层原理http模块的createServer
const app = express()
// 3、路由语法
app.get('/',(req,res) =>{
	res.send('hello,world');
})
// 4. 启动服务监听端口
app.listen(3000,()=> {
	console.log('http://localhost:3000')
});
```

启动

```bash
nodemon .\http.js
# 或者
node .\http.js
```

PS：nodemon安装

```bash
npm i -g nodemon
```



#### 3.6.3 学生添加接口

步骤1： 定义路由 /stu post

​	新建文件夹 controller, 其下新建stu.js 用于处理用户请求

```javascript
//导入模型 
const {
    createModel
} = require(process.cwd() + '/models/stu');

//定义处理方法
const create =(req,rep)=>{
	res.send("this is stu create") //响应固定数据 测试用
}

//导出
module.export={
	create
}
```

步骤2：响应任意json数据

```javascript
const create =(req,rep)=>{
	//res.send("this is stu create") //响应固定数据 测试用
    //1 接收数据 
    let postData = req.body
    //2 过滤(忽略)
    //3 操作数据库
    let rs = await createModel(postData)
    //4 判断返回
    if(rs){
        res.send({
            meta: {
                state: 200,
                msg: '添加成功'
            },
            data: null
        })
    } else {
        res.send({
            meta: {
                state: 500,
                msg: '添加失败'
            },
            data: null
        })
    }
}
```

步骤3：定义stu模型，定义创建数据的方法

新建目录models 目录下创建stu.js 用于连接mogodb并且定义模型，js内容如下

```javascript
// 一、导入模块
const mongoose = require('mongoose');
// 二、连接数据库
const db = mongoose.createConnection('mongodb://192.168.0.110:27017/shop',{useNewUrlParser:true,useUnifiedTopolgy:true},err=> {
   if(err) {
       console.log('数据库连接失败',err);
   }
});


// 三、设置数据模型(声明式哪个集合，限制字段格式和字段类型)
const model = db.model('stu',{
    uname:{type:String,default:'username'},  
    pwd:String,
	age:{type:Number},
	sex:{type:String}
});

// 四、方法
//const createModel = (postData) => {
const createModel = postData => {
    const insertObj = new model(postData);
    return insertObj.save().then(res=> {
        console.log(res)        
        return res
    })
    .catch(err => {
        console.log('插入失败'+err)
        return false
    })
}

module.exports = {
    createModel
}
```

步骤4：调用stu模型创建数据的方法，返回结果

controller\stu.js 

```javascript
    let postData = req.body
    //2 过滤(忽略)
    //3 操作数据库
    let rs = await createModel(postData)  //调用models/stu.js的createModel() 方法创建模型
    //4 判断返回
```

http.js中添加代码：

```javascript
const stuControll = require(process.cwd()+'/controller/stu')
app.post('/stu',stuController.create)
```

注意：踩坑：接口接收不到数据 

需要安装body-parser模块 否则接收不到post提交的数据

安装body-parser模块

```bash
yarn add body-parser
```

使用body-parser模块

```javascript
const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
```



#### 3.6.4 学生列表接口

#### 3.6.5 学生列表接口分页

### 3.7 实战接口文档开发apiDoc