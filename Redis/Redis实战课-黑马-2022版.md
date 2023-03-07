# Redis实战课-黑马-2022版

> https://www.bilibili.com/video/BV1cr4y1671t
>
> ## P1 Redis课程介绍导学
> 
#### 你新增理想的Redis课程是什么样子的
- 知识全面
- 与时俱进
- 贴合企业开发
- 理论结合实战
- 由浅入深
- 通俗易懂
- 高频面试
 

##### 内容全面
- 八种不同的数据结构


##### 理论结合实战
- 缓存穿透
- 热点Key

基础篇
- Redis常用数据结构及命令
- Jedis的应用和优化
- SpringDataRedis的应用和优化
实战篇
- 共享Session企业缓存解决方案
- 描述中的Redis应用
- 社交APP中的Redis应用
- Redis特殊数据结构的应用
高级篇
- 主从模式
- 哨兵模式
- 集群模式
- 多久缓存
- Redis应用最佳实践
原理篇
- Redis常见数据类型的底层结构
- Redis通信模型
- Redis的内存策略
- Redis的常见面试题


## P2 Redis入门课程介绍


## P8 Redis数据结构介绍
Redis数据结构
Redis是一个Key-value的数据库，key一般是String类型，value的类型多种多样

###### 基本类型
- String
- Hash
- List
- Set
- SortedSet
###### 特殊类型
- GEO
- BitMap
- HyperLog



官方文档
http://redis.io/commands/string

```bash
help 
help @String
help @hash
```



## Redis通用命令

- 查询命令帮助
```bash
help @
```


- KEYS 查看符合模板的所有key
```bash
help keys
KEYS * 
KEYS a*   
```
*模糊查询 性能较差 ，单线程 会阻塞其他请求

- DEL 删除
```bash
DEL name
DEL k1 k2 k3 
```

- EXISTS 判断key是否存在
```bash
help exists
```

- EXPIRE 设置有效期，到期时 这个key 会自动删除
```bash
EXPIRE age 20  ## 设置age的有效期20秒  
```

- TTL 查看可以的剩余有效期
```bash
```

## String类型
String类型 字符串类型 是Redis中最简单的存储类型
根据字符串的格式不同，访问三类
- String 普通字符串
- int 整数类型 可以做自增 自减操作
- float 浮点类型 可以做自增、自减操作

### String类型的场景命令
- set
- GET
- MSET  批量添加
- MGET  批量获取
- INCR 整形的key自增1
- INCRBY 自增指定步长
- DECR
- INCRBYFLOAT 浮点型自增指定步长
- SETNX 添加一个key 如果存在不执行，不存在才执行
- SETEX 添加一个key 并设置有效期

## P11 Redis Key的层级格式

key的结构
key运行有多个单词形成的层级结构，多个单词之间用: 冒号隔开 形式如下
项目名:业务名:类型:id

