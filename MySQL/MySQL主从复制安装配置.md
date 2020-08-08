> 视频

https://www.bilibili.com/video/BV1Wa4y1v7uA

#### 1、基础设备准备

```bash
# 操作系统
centos 6.5
# mysql版本
5.7
#两台虚拟机：
192.168.44.129
192.168.1.139
```



#### 2、安装mysql数据库

```bash
#详细安装步骤参考文档
MySQL5.7详细安装步骤.md
```



#### 3、在两台数据库中分表创建数据库

```sql
--两台必须全部执行
create database msb;
```



#### 4、在主服务器配置开启binlog日志

```bash
#登录mysql
mysql -uroot -p123456
#查看binlog是否开启
show variables like ‘%log_bin%’
显示bin  OFFF  表示未开启
```



```bash
#修改配置文件，执行以下命令打开mysql配置文件
vim /etc/my.cnf
#在mysql模块中添加如下配置信息
log-bin=master-bin	#二进制文件名称
binlog-format=ROW 	#二级制日志格式，有row statement mixed三种格式，row制度是把改变的内容复制过去，而不是把命令在从服务器上执行一遍，statement指的是在主服务器上执行的sql语句，在从服务器上执行统一的语句。mysql默认采用基于语句的复制，效率比较高，mixed指的是默认采用基于语句的复制，一旦发现基于语句的无法精确的复制时，就会采用基于行的复制。
server-id=1			#要求各个翻译完全的id必须不一样
binlog-do-db=msb	#要同步的数据库名称
```



#### 5、配置从服务器登录主服务器的账号授权

```sql
--授权操作
set global validate_password_policy=0;
set global validate_password_length=1;
grant replication slave on *.* to 'root'@'%' identified by '123456';
--刷新权限
flush privileges;
```



#### 6、从服务器的配置

#### 7、重启主服务器并进行相关配置

#### 8、重启从服务器