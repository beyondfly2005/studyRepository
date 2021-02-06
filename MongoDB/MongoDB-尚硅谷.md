## MongoDB教程

> MongoDB夯实基础 尚硅谷出品 李立超
>
> 视频地址：https://www.bilibili.com/video/BV1NW411T7vT
>
> MongoDB的基础视频，适合有 Node.js 基础的同学学习。基于前端课程



## 数据库（Database）

- 数据库是按照数据结构来组织，存储和管理数据库的仓库。
- 我们的程序都是在内存中运行的，一旦程序运行结束或者计算机断电，程序运行中的数据都会丢失。
- 所以我们就需要将一些程序程序运行的数据持久化到硬盘中，以确保数据的安全。
- 简单来说，数据库就是存储数据的仓库。

## 数据库的分类

数据库主要分为两种：

- 关系型数据库（RDBMS）
  - MySQL、Oracle、DB2、SQL Server、SYBASE
  - 关系型数据库中都是用表来存储
- 菲关系性数据库
  - MongoDB、Redis ...
  - 键值对存储数据
  - 文档数据库MongoDB

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

- MongoDB对32位系统支持不佳，所以3.2版本以后没有再对32位系统的支持

- 安装 （Windows64位系统）

- 配置环境变量 将MongoDB的安装路径\bin 配置到path中

- 测试安装 cmd 输入 mongo

- C盘根目录创建文件夹data\db

- cmd 输入mongod启动mongodb服务器

- 客户端：再打开一个cmd 输入 mongo ；连接mongoDB 出现 >  表明服务器启动成功

- 32位系统，第一次启动服务器需要输入: mongod --storageEngine=mmapv1

  以后启动 mongod

- 指定其他路径 mongod --dbpath d:\mongodb\data

- 默认端口21017 指定端口  mpmdod --port 10086 

  - 不要使用已被占用的端口 如3306 80 8088
  - 端口号最大不要超过65535

## 将MongoDB设置为系统服务

- 手动创建服务

  - 创建目录 

    ```bash
  mkdir c:\data\db  
    mkdir c:\data\log
    ```
  
  - 创建配置文件 
    
      c:\progrogramFile\MongdDB\Server3.2\
    
      bin同目录下mongod.cfg文件
    
      写入
  
      ```config
  systemLog:
    destination: file
      path: c:\data\log\mongod.log
  storage:
      dbpath: c:\data\db
    ```
  
- 以管理员身份打开命令行cmd
  
  - 执行如下命名
  
    ```bash
    sc.exe create MongoDB binPath="\"c:\Program Files\MongoDB\Server\3.2\bin\mongod.exe\" --service --config="\c:\program Files\MongoDB\Server\3.2\mongod.cfg\"" DisplayName="MongoDB" start="atuo"
    ```
  
  - 启动Mongodb服务
  
  - 如果启动失败，证明上面的操作有误，在控制台输入：
  
    ```
    sc delete MongoDB
    ```
    
    删除之前的配置服务，然后从第一步再来一次

## MongoDB基本操作

#### 基本概念

​	数据库 database

​	集合 collection

​	文档 document

#### 常用命令

Mongod Shell 可以编写js代码

```bash
#显示当前所有数据库
show dbs
#显示当前所有数据库
show databases
#进入指定的数据库
use db1
```

数据库和集合 不需要显示创建，在第一次向数据库中添加文档时，自动创建

```bash
#显示当前所在的数据库
db
```

```bash
#显示数据库中所有的集合
show collections
```

#### 数据库的CRUD

 - 插入文档

   ```bash
db.<collection>.insert(doc)  
   ```

   向test数据库students集合中插入一个学生对象 
   
   ```bash
   db.students.insert({name:'孙悟空'}，age:18,gender:'男')	#db表示当前数据库
   test.students.insert({name:'孙悟空'}，age:18,gender:'男')
   ```
   
- 查询当前集合中的所有文档

  ```bash
  db.<collection>.find()	#查询当前集合中的所有文档
  ```

- insertone



## 安装图形化工具

##### 工具一

工具名称：NoSQL Manager for MongoDB freeware

下载地址：https://www.mongodbmanager.com/download

安装文件：mongodbmanagerpro_inst.exe

##### 工具二

工具名称：Studio 3T for MongoDB

下载地址：https://studio3t.com/download/

文件名称：studio-3t-x64.msi



## 插入文档

```bash
#插入（可以一个或多个）
db.student.insert()

#插入一个
db.student.insertOne()

#插入多个
db.student.insertMany()

```

## 查询文档

```bash
# 查询集合中符合条件的文档
db.student.find()

# find中可以接受一个对象作为条件参数
db.student.find({})  # 查询集合中

db.student.find({_id:"hello"})  # {字段名:值}  {属性名:值}
db.student.find({age:28})
db.student.find({age:16})
db.student.find({age:16,name:"白骨精"});

db.student.find({age:28})

#查询集合中符合条件的第一个文档
db.student.findOne()

db.student.findOne({age:28});

# 区别
# find() 返回的是一个数组， findOne()返回的是一个对象

# 查询结果数量
db.student.find().count();
db.student.find().length();
```



## 修改文档

```bash
db.collection.update(查询条件,修改为的新对象)

#update 默认情况下 会用新对象替换为新对象，

#例 将沙和尚的 年龄改为28 其他属性 如 name age都会丢失
db.student.update({name:"沙和尚"}, {age:28})

#如果需要修改 指定的属性，而不是替换，这时应使用修改操作符 来实现修改
# $set 操作符 设置属性
<update >
db.student.update({name:"沙和尚"},$Set:{name:"沙和尚"})
db.student.find("_id":ObjectId("xxxxxx"));

# $unset:操作符  删除属性
# address无论写什么 都会被删除这个属性
db.student.update({name:"沙和尚"},$unet:{address:"AAA"}

# 修改多个条件的文档
db.collection.updateMany(查询条件,修改为的新对象)  #匹配到多个  就会修改多个

# 修改一个条件的文档
db.collection.updateOne(查询条件,修改为的新对象)	#修改一个

db.collection.update #默认只改第一个 如果匹配到多个的话

db.collection.update(查询条件,修改为的新对象,{选项})
db.collection.update(查询条件,修改为的新对象,{multi:true})	#修改多个
```



## 删除文档

```bash
## 语法
db.collection.deleteOne()；	#删除一个
db.collection.deleteMany()；	#删除多个
db.collection.remove()；		#删除一个或多个，如果第二个参数 传递true 则会删除一个；默认会删除多个
db.collection.remove({条件}，<justone: true or false>)；
db.collection.drop()；	#删除集合
db.dropDatabase()；		# 删除数据库

# remove 可以根据条件查询文档，
# remove 会删除符合条件的多个文档，默认可以删除多个
db.collection.remove({查询条件}，true); #最后一个参数是 justone :ture 

## 实例
db.student.remove({_id:"hello"})

## 如果只传递一个空对象作为参数，则会删除文档中的所有文档  ;  性能较差
db.sutdent.remove({})
db.sutdent.remove() ；# 报错

db.sutdent.drop() # 删除清空集合 要比db.sutdent.remove({}) 性能好


## 删除很少用 ，一般需要删除是，使用一个字段 做标记删除即可

```

##### 练习一

##### 练习二



## 文档之间的关系

关系分为三种：

 - 一对一  内嵌文档方式 体现一对一关系

   ```
   夫  - 妻
   人  - 身份证号
   ```

   

 - 一对多  也可以通过内嵌文档的方式实现

   ```
   父母 - 孩子
   用户 - 订单
   文章 - 评论
   ```

   内嵌文档方式存在的问题时，随着“子”越来越多，文档越来越大

 - 多对多

   ```
   分类 - 商品
   老师 - 学生
   
   ```

   

一对一 示例：

```bash
db.wifeAndHusband.insert(
  [
	{name:"黄蓉"},
	husband:{name:"郭靖"}
  ]
)
```

一对多示例：

```bash
db.order.insert({
	list:["苹果","香蕉","桃子"],
	user_id:ObjectId("1001")
});
db.order.insert({
	list:["牛肉","香肠","鸡肉"],
		user_id:ObjectId("1002")
});
db.order.insert({
	list:["猕猴桃","香蕉","桃子"],
		user_id:ObjectId("1001")
});

#查询 1001用户的订单
var user_id = db.user.findOne({name:"孙悟空"})._id;
db.order.find({user_id:user_id});
```

多对多示例：

```bash
db.teacher.insert({
	{name:"洪七公"},
	{name:"黄药师"}，
	{name:"龟仙人"}
})；
db.student.insert({
	{name:"郭靖"}，
	teacher_ids:[Oject_id("1001"),Oject_id("1002"),Oject_id("1003")]
})
```

## Sort和投影

```bash
# 查询文档时 默认是按照_id 排序排列的(升序)
# _id 又是按时间戳生成的 所以 也是按数据的插入时间和顺序排序的
db.emp.find({});

# sort() 函数 指定文档排序规则
db.emp.({}).sort(sal); #按工资排序 1：升序 -1：降序
db.emp.({}).sort(sal:-1); # 按工资降序
db.emp.({}).sort(sal:1,empno:-1) # 先按sal升序 在按empno降序

# limit() skip() sort() 三个函数可以按任意顺序调用
# 查询时 只查询某一个字段 或 某几个字段，称为投影
db.emp.({},{ename:1});  #只显示ename 默认包含_id
db.emp.({},{ename:1,_id:0}); #显示ename 不显示_id

```

## Mongoose简介

### Mongoose简介

- 之前我们都是通过shell来完成对数据库的各种操作的， 在实际开发中，大部分时候，我们都是通过程序来完成对数据库的操作。
- 而Mongose就是一个让我们可以通过Node来操作MongoDB的模块
- Mongoose是一个对象文档模型（ODM）库，他对Node原生的MongoDB模块做了进一步的优化封装，并提供了更多的功能，
- 在大多数情况下，他被用来把结构化的模式应用到一个MongoDB集合，并提供了验证和类型转换等好处。

官网地址： http://mongoosejs.com

官方文档： http://mongoosejs.com/docs/api.html

### Mongoose的好处

- 可以以文档创建一个模式结构（Schema）（约束：对插入的数据进行约束，必填 类型 字段格式 等）
- 可以对模型中的对象/文档进行验证
- 数据可以通过类型转换转换为对象模型
- 可以使用中间件来应用业务逻辑挂钩
- 比Node原生的MongoDB驱动更容易

### Mongoose新的对象

- Schema（模式对象）

  Schema对象定义约束了数据库中的文档结构

- Model 模型对象

  Model对象作为集合中的所有文档的表示，相当于MongoDB数据库中的集合collection

- Document 文档对象

  Documen表示集中的具体文档，相当于集合中的一个具体的文档

### Mongoose使用

创建工程目录

创建文件 

创建文件 01-helloMongoose.js

```javascript
/**
	1、下载安装Mongoose
		npm i mongoose --save  # 安装并添加到依赖
	2、项目中引入Mongoose
    	var mongoose = require("mongoose")
    3、连接MongoDB数据库	
	    mongoose.connect(‘mongodb://localhost/test’,{userMongoClient:true})
    	如果是默认端口 27017 可以不写
    4、监听MongoDB数据库的连接状态
    	在Mongose对象中，有一个属性叫connection,该对象表示的就是数据库连接
    	通过监听该对象的状态，可以监听数据库的连接与断开
    	mongoose.connection.once("open",function(){})
    	#监听数据的断开事件
    	mongoose.connection.once("close",function(){})；
    4、断开数据库连接 (一般不需调用)
    mongoose.disconnect();
**/

var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost:27017/test",{userMongoClient:true});
mongoose.connection.once("open",function(){
    console.log("数据库连接成功");
});
mongoose.connection.once("close",function(){
    console.log("数据库连接已断开");
});

//断开数据库连接
mongoose.disconnect();
```

创建文件mongoose_demo.js

```javascript
var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost:27017/test",{userMongoClient:true});
mongoose.connection.once("open",function(){
    console.log("数据库连接成功");
});

//创建shcemo(模式对象/约束对象)
var Schema = mongoose.Schema;
var studentSchema = new Schema({
    name:String,
    age:Number,
    gender:{
        type:String,
        default:"female"
    },
    address:String
});

//创建Model 数据库中的集合
//var = mongoose.model(modelName,Schema)
//modelName 就是要映射的集合名，mongoose会自定将集合名变为复数 student -> students,students -> 
//students child -> children
var studentModel = mongoose.model("student",studentSchema);

//向数据库中插入文档
//studentModel.create(doc,function(){})
studentModel.create({
    name:"孙悟空",
    age:18,
    gender:"male",
    address:"花果山"
},function(err){
    if(!err){
        console.log("插入成功！");
    }
})
```

model中的方法

新建文件model.js

```javascript
var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost:27017/test",{userMongoClient:true});
mongoose.connection.once("open",function(){
    console.log("数据库连接成功");
});

//创建shcemo(模式对象/约束对象)
var Schema = mongoose.Schema;
var studentSchema = new Schema({
    name:String,
    age:Number,
    gender:{
        type:String,
        default:"female"
    },
    address:String
});

//创建Model 数据库中的集合
//var = mongoose.model(modelName,Schema)
//modelName 就是要映射的集合名，mongoose会自定将集合名变为复数 student -> students,students -> 
//students child -> children
var studentModel = mongoose.model("student",studentSchema);
/**
  有了model 就可以对数据库进行增删改查
  Model.create(doc(s),[callback])
  	- 创建一个或多个文档，并添加到数据库中
  	- 参数：
  		doc(s) 可以是一个对象，也可以是一个对象的数组
  		callback 回调函数，当操作完成以后的回调函数
  Model.find()
  	- 查询符合条件的文档 总会返回数组
  Model.findById()
   	- 根据文档的id查询文档
  Model.findOne()
  	- 查询符合条件的第一个文档
  参数：
  		conditions 查询条件
  		projection 投影 需要取到的字段
  			两种方式
  			- 方式一 对象方式
  				{name:1,_id:0}
  			- 方式二 字符串方式
		        空格隔开 "name age" 不需要的字段 前面加减号 如"name age -_id" 
        options 查询选项
  		callback 回调函数
*/

studentModel.create([{
    name:"猪八戒",
    age:28,
    gender:"male",
    address:"花果山"
},{
    name:"唐僧",
    age:28,
    gender:"male",
    address:"花果山"
}],function(err){
    if(!err){
        console.log("插入成功！");
        console.log(arguments);
    }
});

studentModel.find({name:"唐僧"}，function(err, docs){
    if(!err){
        console.log(docs);
        console.log(docs[0].name);
    }
});

studentModel.find({name:"唐僧"}, {name:1,_id:0} ,function(err, docs){
    if(!err){
        console.log(docs);
        console.log(docs[0].name);
    }
});

studentModel.find({name:"唐僧"}, "name age -_id",function(err, docs){
    if(!err){
        console.log(docs);
        console.log(docs[0].name);
    }
});

# 跳过前三个 前三个不查询， limit:1 只查询一条数据/一个对象 最多显示一个
studentModel.find({name:"唐僧"}, "name age -_id", {skip:3 ,limit:1},function(err, docs){
    if(!err){
        console.log(docs);
        console.log(docs[0].name);
    }
});
# 返回的是对象
studentModel.find({},function(err, doc){
    if(!err){
        console.log(docs);
    }
});

# findById
studentModel.findById("59c4c3c2c1e4d5f1456c45",function(err, doc){
    if(!err){
        console.log(docs);
    }
});

//Document对象是Model的实例
// 所有带#号的方法 都是Document的方法
studentModel.findById("59c4c3c2c1e4d5f1456c45",function(err, doc){
    if(!err){
        console.log(doc instanceof studentModel);
    }
});


//修改
/**
  Model.update()
  Model.updateMany()
  Model.updateOne();
  Model.replaceOne();
  参数：
  	conditions 查询条件
  	doc 修改后的对象
  	options 配置参数
  	callback 回调函数
*/
studentModel.updateOne({name:"唐僧",{$set:{age:20}}},function(err){
    if(!err){
        onsole.log("修改成功");
    }
});


/**
	Model.remove();
	Model.deleteOne();
	Model.deleteMany();
*/
studentModel.remove({name:"白骨精"},function(err){
    if(!err){
        onsole.log("删除成功");
    }
}
                    
/**
	Model.count(condution,[callback])
	统计文档的数量
*/
studentModel.count({},function(err,count){
    if(!err){
        onsole.log(count);
    }
})
```

### Document对象的方法

- equals(doc)
- id
- get(path,[type])
- set(path,[type])
- update(update,[option],[callback])
- save([callback])
- remove
- isNew
- isInit(path)
- toJSON()
- toObject()

##### Document对象示例

新建04-document.js

```javascript
/**
	Document 和MongoDB数据库集合中的文档 是一一对应的 Document是Model的实例
	通过Model查询到结果都是 Document
	Model(doc) 构造函数
*/
var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost:27017/test",{userMongoClient:true});
mongoose.connection.once("open",function(){
    console.log("数据库连接成功");
});

//创建shcemo(模式对象/约束对象)
var Schema = mongoose.Schema;
var studentSchema = new Schema({
    name:String,
    age:Number,
    gender:{
        type:String,
        default:"female"
    },
    address:String
});

//var studentModel = mongoose.model("student",studentSchema);

//创建Model 数据库中的集合
//var = mongoose.model(modelName,Schema)
//modelName 就是要映射的集合名，mongoose会自定将集合名变为复数 student -> students,students -> 
//students child -> children
var studentModel = new Model({
    name:"奔波霸",
    age:48
    gender:"male",
    address:"碧波潭"
});
//创建成功 并不会添加到数据库，只是内存中创建
/**
	Model#save({option},{fn})
*/
studentModel.save(function(err){
    if(!err){
        console.log("保存成功");
    }
});


//修改
studentModel.findOne({},function(err,doc){
    if(!err){
        //方法一
        doc.update({$set:{age:28}},function(err){
            if(!err){
                console.log("修改成功");
            }
        });
        //方法二
        doc.age = 28;
        doc.save();
    }
});

//删除
studentModel.findOne({},function(err,doc){
    if(!err){ 
    	doc.remove(function(err){
            console.log("大师兄再见！");
        });
    }
});

studentModel.findOne({},function(err,doc){
    if(!err){  
        //get()方法
        console.log(doc.get("age");
        console.log(doc.age);
        
        //set()方法
        doc.set("name","猪九戒");	//数据库不变， 调用doc.save() 才会影响数据库
        doc.name="猪九戒";
        
        //id()方法
        console.log(doc.id());
        console.log(doc._id());
        
        //toJSON()
        
        //toObject()  将Document对象转换为普通的js对象 转换为普通js对象之后， 所有的Document的方法都不能使用了
        var o = doc.toObject();
        console.log();
        
        doc = doc.toObject();
        delte doc.address;	//用于删除属性 防止隐私信息 泄漏
        console.log(doc);
    }
});

```

### mongoose模块化

将创建连接



创建tools文件夹

创建conn_mondb.js

```javascript
var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost:27017/test",{userMongoClient:true});
mongoose.connection.once("open",function(){
    console.log("数据库连接成功");
});
```

使用这个模块

```javascript
require("./tools/conn_mongo");
```

创建文件夹models

添加student.js

```javascript
var mongoose = require("mongoose");
var Schema = mongoose.Schema;
var studentSchema = new Schema({
    name:String,
    age:Number,
    gender:{
        type:String,
        default:"female"
    },
    address:String
});

//定义模型
var studentModel = mongoose.model("student",studentSchema);

exports.model = studentModel;
```

使用这个模块 Model

```javascript
require("./tools/conn_mongo");
var Student = require("./model/student").model

Student.find({},function(err,docs){
   if(!err) {
       console.log(doc);
   }
});
```

改进

```javascript
var mongoose = require("mongoose");
var Schema = mongoose.Schema;
var studentSchema = new Schema({
    name:String,
    age:Number,
    gender:{
        type:String,
        default:"female"
    },
    address:String
});

//定义模型
var studentModel = mongoose.model("student",studentSchema);

#exports.model = studentModel; #原版
#module = studentModel; #这是在改变变量的值，并非导出这个对象
module.exports = studentModel;  #改进版
```

```javascript
require("./tools/conn_mongo");
#var Student = require("./model/student").model; #原版
var Student = require("./model/student");  #改进版

Student.find({},function(err,docs){
   if(!err) {
       console.log(doc);
   }
});
```

