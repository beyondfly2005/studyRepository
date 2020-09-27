> 视频地址：https://www.bilibili.com/video/BV1cW411r7c5



下载尚硅谷全套教程:→ [BV1Kb411W75N](https://www.bilibili.com/video/bv1Kb411W75N/)（看完就懂）


《《行业标杆_大数据学习路线》》

2020最新路线:[cv5213600](https://www.bilibili.com/read/cv5213600/)

【推荐观看】
Linux(大数据定制版):[BV1ZW411k7Qh](https://www.bilibili.com/video/bv1ZW411k7Qh/)
Shell:[BV1hW41167NW](https://www.bilibili.com/video/bv1hW41167NW/)
Hadoop:[BV1cW411r7c5](https://www.bilibili.com/video/bv1cW411r7c5/)
Zookeeper:[BV1PW411r7iP](https://www.bilibili.com/video/bv1PW411r7iP/)
Hive(升级版):[BV1W4411B7cN](https://www.bilibili.com/video/bv1W4411B7cN/)
Flume(升级版):[BV184411B7kU](https://www.bilibili.com/video/bv184411B7kU/)
Kafka(升级版):[BV1a4411B7V9](https://www.bilibili.com/video/bv1a4411B7V9/)
HBase(升级版):[BV1Y4411B7jy](https://www.bilibili.com/video/bv1Y4411B7jy/)
HA:[BV1zb411P7KY](https://www.bilibili.com/video/bv1zb411P7KY/)
Sqoop:[BV1jb411A7tc](https://www.bilibili.com/video/bv1jb411A7tc/)
Azkaban:[BV1t4411B7Rh](https://www.bilibili.com/video/bv1t4411B7Rh/)
Oozie:[BV1jb411A7Ar](https://www.bilibili.com/video/bv1jb411A7Ar/)
Scala:[BV15t411H776](https://www.bilibili.com/video/bv15t411H776/)
Scala数据结构和算法:[BV1zb411h78b](https://www.bilibili.com/video/bv1zb411h78b/)

【项目实战】
电信客服:[BV17t411W7wZ](https://www.bilibili.com/video/bv17t411W7wZ/)
机器学习与推荐系统:[BV1R4411N78S](https://www.bilibili.com/video/bv1R4411N78S/)
电商推荐系统:[BV1X4411Y7ZN](https://www.bilibili.com/video/bv1X4411Y7ZN/)
电商数仓V2.0:[BV1df4y1U79z](https://www.bilibili.com/video/bv1df4y1U79z/)
Flink:[BV1Qp4y1Y7YN](https://www.bilibili.com/video/bv1Qp4y1Y7YN/)
阿里云数仓(离线):[BV1AJ411Q7ox](https://www.bilibili.com/video/bv1AJ411Q7ox/)
阿里云数仓(实时):[BV1dJ411k7BE](https://www.bilibili.com/video/bv1dJ411k7BE/)

尚硅谷教程如何下载（全部免费，无需转发集赞，直接下载）：[cv3860455](https://www.bilibili.com/read/cv3860455/)

教程学习中可能遇到的非技术问题（只有声音、pdf无法复制、提取码等错误）：[cv3829288](https://www.bilibili.com/read/cv3829288/)





#### 2.3 Hadoop三大发型版本

Hadop三大发行版本：Apache、Cloudera、Hortownworks



#### 2.4 Hadoop优势（4高）

1、高可靠性

2、高扩展性

3、

#### 2.5 Hadoop组成（面试重点）

##### 2.5.1 HDFS架构概述

HDFS

NameNode：存储文件的元数据，如文件名，文件目录结构，文件属性

DataNode：存储



##### 2.5.2YARM架构概述

###### ResourceManager(RM)主要作用

处理客户端请求

监控NodeManager

启动或监控ApplicationMaster

资源的分配与调度

###### NodeManager(NM) 主要作用

- 管理单个节点上的资源

- 处理来自ResourceManager的命令

- 处理来自Application的命令



###### ApplicationMaster(AM)作用



###### Container





##### 2.5.3 MapReduce架构概述

MapReduce将计算过程分为两个阶段：Map和Reduce

1、Map阶段机芯处理出入数据

2、Reduce阶段对Map结果进行汇总

Map是分的过程，Reduce是合的过程



#### 2.6 大数据技术生态体系

![img](https://img-blog.csdnimg.cn/20181203131639560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NjU3OTA5,size_16,color_FFFFFF,t_70)

数据来源层

- 数据库数据（结构化数据）
- 文件日志（半结构化数据）
- 视频、PPT（非结构haul数据）

数据传输层

- sqoop数据传递 负载数据的导入导出
- Flume日志收集
- Kafka消息队列

数据存储层

- HDFS 文件存储
- HBase菲关系性数据库
- Kafka队列中的数据

资源管理层

- YARN资源管理

数据计算层

- MapReduce离线计算
- Spark Core内存计算

数据分析

- Hive数据查询 （SQL查询）
- Mahout数据挖掘

- Spark Mlib数据挖掘
- Spark R数据分析
- Spark SQL 数据查询
- Spark Streaming 实时计算
- Storm实时计算

- Flink 

任务调度器

- Oozie任务调度
- Azaban任务调度
- Crontab linux自带

数据平台配置和调度

- Zookeeper



2.7 推荐系统框架图

![img](https://img-blog.csdnimg.cn/20181203132727633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NjU3OTA5,size_16,color_FFFFFF,t_70)



### 第3章 Hadoop环境搭建

##### 3.1 虚拟机环境准备

1、克隆虚拟机

```
创建完整克隆
```

2、修改克隆虚拟机的静态IP

```
vim /etc/udev/rules/d/70-persistend-net.rules
dd 删除名称为 eth0的  NAME="eth0"
将NAME="eth1" 改为 NAME="eth0"
复制ATTR{address}=="xxx"  复制引号内的内容
:wq 保存退出

vim /etc/sysconfig/network-script/ifcfg-eth0
修改HWADDR=xxx   修改物理IP地址，粘贴上一步复制的内容
修改IPADDR

检查ONBOOT=yes
BOOTPROTO=static
GATEWAY=网关地址
DNS1=
```

3、修改主机名

```
vim /etc/sysconfig/network
修改HOSTNAME=hadoop101

检查host是否配置好
vim /etc/hosts

重启网络服务 重启主机名称服务器
或者 直接重启虚拟机
reboot

在虚拟机内部 ping宿主机
修改宿主机 hosts文件 配置虚拟机的主机名与ip地址映射
在宿主机使用 ping 虚拟机 名称 方式 测试网络连接
ping hadoop001
```

4、修改防火墙

5、创建xx用户

6、配置xx用户具有root权限

```
vim /etc/sudoers 当前用户没有权限
su root
vim /etc/sudoers
复制行
root ALL=（ALL）  ALL
粘贴并修改为
user ALL=（ALL）  ALL
# 允许root 执行命令行在任何地方 allow root to run any commonds anywhere
```

7、 在/opt目录下创建文件夹

(1) 在/opt目录下创建module、software文件夹

software 存放安装文件 module 存放

```
sudo mkdir module
sudo mkdir software
```

(2) 修改module、software文件夹的所有者

```
sudo chown atguigu：atguigu module/  software/  # 修改文件所有者 和 所有组
拷贝jar包到
```

###### 安装JDK

```
# 配置java houme
vim 
export JAVA_HOME=/opt/module/jdk1.8.0_144
exprot PATH=$PATH

source /etc/
```

安装Hadoop

```
tar -zxvf 
cd hadoop-2.7.2

vim 
##HADOOP_HOME
exprot HADOOP_HOME=/opt/moudle/hadoop-2.7.2
exprot PAHT=
exprot PAHT=

source /etcprofile
hadoop

```

###### Hadoop目录结构

- bin目录：Hadoop的配置文件目录，存放Hdoop的配置文件
- etc 配置文件
- include
- lib目录：存放Hadoop的 本地库（对数据进行压缩解压缩功能）
- libexec 目录
- sbin目录：存放 启动或停止 Hadoop相关服务的脚本
- share 共享文件存放Hadoop的依赖jar包、说明文档 手册 官方开发案例

### 第4章 Hadoop运行模式

Hadoop运行模式包括：本地模式、伪分布式模式、以及完全分布式模式

Hadoop官网：http://hadoop.apache.org/

Hadoop生态

使用的版本2.7.2

#### 4.1 本地运行模式

##### 4.1.1官方Grep案例

1、创建：在hadoop-2.7.2 文件下面创建一个input文件夹

```java
mkdir input
cp etc/*.xml input/
bin/hadoop jar shar/hadoop/mapreducehadoop-mapreduce-examples-2.7.2.jar grep input/ output 'dfs[a-z.]+'
## output 一定不能存在 否则会报异常

```



##### 4.1.2 官方WordCount案例

1、创建：

​	在hadoop-2.7.2文件夹下面创建一个wcinput文件夹

```
mkdir wcinput
```

2、在wcinput文件夹下创建一个wc.input 文件

```
cd wcinput
touch wc.input
```

3、编辑wc.input文件

输入一单词 要有重复的

```
hadoop jar share/
```

4、查看结果

```
cd 
```

hadoop 

#### 4.2 伪分布式模式

完全按照分布式模式来配置，但是节点只有1台

##### 4.2.1 启动HDFS并运行MapReduce

1、配置集群

(a) 配置hadoop-env.sh

查看jdk的安装路径

```bash
echo $JAVA_HOME
# 以下显示jdk安装路径
/opt/module/jdk1.8.0_144
```

修JAVA_HOME

```bash
vim hadoop-env.sh
export JAVA_HOME=/opt/module/jdk1.8.0_144
```

(a) 配置 core-site.xml

```bash
vim etc/hadoop/core-site.xml
```

```xml
<!--指定HDFS中NameNode的地址-->
<property>
	<name>fs.defaultFS</name>
    <value>hdfs://hadoop101:9000/</value>
</property>
<!--指定Hadoop运行时产生文件的存储目录-->
<property>
	<name>hadoop.tmp.dir</name>
    <value>/opt/module/hadoop-2.7.2/data/tmp</value>
</property>
```

(c) 配置 hdfs-size.xml

```xml
<!--指定HDFS副本数量 默认为3-->
<property>
	<name>dfs.replication</name>
    <value>1</value>
</property>
```



###### 2、启动集群

格式化NameNode(第一次启动时格式化，以后就不要总格式化)

hdfs namenode -formate

启动

```
sbin/hadoop-daemon.sh start namenode

jps  #java 的ps 
```



#### 4.3 完全分布式运行模式























