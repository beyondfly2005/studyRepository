# CDH 6.3.2教程  企业离线数数据仓库
> https://www.bilibili.com/video/BV19L411r7U5

## P1 导言
与合并开始收费

课程介绍
- CDH与CM概述
- 部署CDH 
  - Kerberos认证
  - Sentry权限控制及性能调优
- CDH编译基础Flink  
- CDH搭建离线数仓

前置基础
- 熟悉Linux命令
- 熟悉Hadoop生态大数据足迹
- 熟悉电商数仓5.0


## P2 课程介绍
文档
CHD-6.3.2
电商数仓 V5.0

资料包
- CHD6
- 离线数仓Jar包
- 脚本 
  - kerberos
  - 不含kerberos
- Flume 拦截器
- Flink
  - 编译Flink
  - 编译好的Flink
- kerberos
  - Windows版kerberos客户端

课程内容
- 1、Cloudera Manager
- 2、数据采集模块
- 3、数仓值数仓环境准备
- 4、编译Flink
- 5、Kerberos
- 6、Senttry安全

## P3 CM概述
Hadoop三大发现版本
- A 
- H HDP  
- C CDH

CDH收购HDP，并构成CDP

CDH 6.3.2 是免费版 最新版
- 离线数仓 集成Hive 
- 不包含一些组件 可以经过二次编译

### 1.1 CM简介
#### 1.1.1 CM简介
Cloudera Manager 是一个拥有集群自动化安装、中心化管理、集群监控、报价功能的一个工具，
使得安装集群从几天缩短在几小时内，运维人员从数十个人降低到几人以内，极大地提高集群管理的效率。


CHD是组件

集群的监控和报警

1.1.2 CM架构
- 1 Cloudera Manager 软件由Cloudera管理分部存储库，有点类似Maven的中央仓库，主要存储CDH的各个版本的安装包
- Server: 负责软件安装、配置，启动和停止服务，管理服务运行的集群。
- Management Service ：由一组执行各种监控，警报和报告功能角色的服务。
- Database 存储配置和监视信息 
- Agent： 安装在每台主机上，负责启动和停止的过一次没配置、监控主机
- Clients ： 是用于放服务器器进行交互的接口（API和Admin Console）

## P4 阿里云购买
- 包年包月
  - 一般用于企业购买 比较贵
- 按量付费
  - 可以进行扩容
- 抢占式实例 不能扩容 
  - 用于购买前已经确定的服务资源 比前两个要便宜
  - 4CPU  16G  通用型G5
  - 无确定使用时长
  - 数量：5台
  - 镜像 CentOS 7.5  64位
  - 存储： ESSD云盘 40G
  - 网络和安全组：默认专有网络
  - 带宽：按使用流量  峰值100Mbps
  - 安全组： 默认安全组
- 系统配置
  - 自定义密码  设置密码 8-30字符 大小写及特殊字符
  - 实例名称 hadoop[101,3]  3是占用位数 不是服务器台数
- 停机
  - 节省停机方式
- 安全组
  - 默认开放 22端口
  - 使用默认安全组 或者新建安全组 
  - 添加 或者导入 开发端口json文件
  - 将IP 0.0.0.0 改为自己的互联网IP地址 ，防止服务器受到攻击
  - 将所有的服务器 加入配置好的安全组
  
## P5 CM部署前准备

使用XShell连接阿里云服务器 ，使用共有IP连接
内部使用 私有IP连接

yum -y install lrzsz

准备host文件 将主机名称与IP地址进行对应

```text
192.168.1.1 hadoop101
192.168.1.2 hadoop102
192.168.1.3 hadoop103
192.168.1.4 hadoop104
192.168.1.5 hadoop105
```

vim  /etc/host 
将5台服务器的ip与主机名的对照填入每一台服务器的host文件

使用ssh连接阿里云的时候 经常会自动断开，解决方案
```bash
vim /etc/ssh/sshd_config

## 找到以下两行
#ClienttAliveInterval 0
#ClienttAliveCountMax 3

## 去掉注释 改为
ClienttAliveInterval 30  //客户端每个多少秒向服务器端发送一个心跳
ClienttAliveCountMax 1800 //客户端
```
5台服务器都需要修改

完成后重启
```bash
service sshd restart

```

#### SSH免密登录

## P6 脚本

## P7 部署前JDK安装

## P7 部署前MySQL安装

## P8 部署前CM安装部署