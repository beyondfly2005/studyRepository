> 视频地址

https://www.bilibili.com/video/BV1QV411d7at?p=1

##### 尚硅谷大数据技术之DataX

### 第1章 概述

#### 1.1 什么是DataX

DataX 是阿里巴巴开源的一个异构数据源离线同步工具，致力于实现包括关系型数据库(MySQL、Oracle等)、HDFS、Hive、ODPS、HBase、FTP等各种异构数据源之间稳定高效的数据同步功能。

#### 1.2 DataX的设计

#### 1.3 框架设计

#### 1.4 运行原理

### 第2章 快速入门

#### 2.1 官方地址

#### 2.2 前置要求

#### 2.3 安装DataX

### 第3章 使用案例

#### 3.1 从stream流读取数据并打印到控制台

/.json

```
channel:5	#并发数
```



#### 3.2 读取MySQL中的数据到Oracle

```properties

python bin/datax.py
# 从stream流读取数据
python datax.py -r streamreader -w streamwriter

# 从oracle读  写入mysql
python bin/datax.py -r oraclereader -w mysqlwriter
```

```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "name": "oraclereader", 
                    "parameter": {
                        "column": ["*"], 
                        "connection": [
                            {
                                "jdbcUrl": ["jdbc:oracle:thin:@192.168.1.254:1521:orcl"], 
                                "table": ["*"]
                            }
                        ], 
                        "password": "lam123", 
                        "username": "rootepxd"
                    }
                }, 
                "writer": {
                    "name": "mysqlwriter", 
                    "parameter": {
                        "column": ["*"], 
                        "connection": [
                            {
                                "jdbcUrl": "jdbc:mysql://192.168.11.75:3336/rootepxd", 
                                "table": ["*"]
                            }
                        ], 
                        "password": "root", 
                        "preSql": [], 
                        "session": [], 
                        "username": "root"
                    }
                }
            }
        ], 
        "setting": {
            "speed": {
                "channel": "6"
            }
        }
    }
}
```

```bash
cd job
cat oracle2mysql.json
vim oracle2mysql.json

#将以上json内容填入oracle2mysql.json

:wq  #保存退出

#执行
cd /opt/datax/datax
./bin/datax.py ./job/oracle112mysql8.json
```

