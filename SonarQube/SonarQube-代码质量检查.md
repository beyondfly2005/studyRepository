# 基于SonarQube代码质量检查工具总结

https://www.jianshu.com/p/cebcc401c00c

### SonarQube 基本概念

##### 1、什么是SonarQube

SonarQube是一个开源的代码质量管理系统，用于检测代码中的错误、漏洞和代码规范。他可以与现有的Gitlab、Jenkins集成，以便在项目拉取后进行连续的代码检查。

##### 2、使用SonarQube前提

- SonarQube基于Java开发，所有需要安装Open JDK8版本
- SonarQube需要依赖MySQL数据库，只是5.6以上版本
- SonarQube的消息示例至少需要4GB内存，如果是大型实例需要16G内存

### SonarQube服务安装

#####　１．环境准备

```bash
# 修改主机名
hostnamectl set -hostname sonarqube

#修改网络
cd /etc/sysconfig/network-scripts/
vim ifcfg-etch0
# 修改IPADDR=192.168.1.xx
# :wq保存退出
systemctl restart network
```


```bash
依赖的文件
mysql-community-client-5.6.45-2
mysql-community-common-5.6.45-2
mysql-community-lib-5.6.45-2
mysql-community-server-5.6.45-2
sonarqube_plugin-7.0
sonar-scanner-cli-4.0
```

### 环境准备

```bash

systemctl stop firewalld
systemctl disable firewalld
setenforce 0
```
###### 安装Sonarqube依赖

```bash
yum install git java unzip wget -y
wget https://mirrors.tuna.tsinghua.edu.cn/mysql
wget https://mirrors.tuna.tsinghua.edu.cn/mysql
wget https://mirrors.tuna.tsinghua.edu.cn/mysql
wget https://mirrors.tuna.tsinghua.edu.cn/mysql
yum localinstall -y mysql-commu
```


###　登录

admin admin

生成令牌  记得保存令牌

Maven项目扫描

