#### 重启Oracle数据库及监听器

**Oracle 重启监听**

> https://blog.csdn.net/zonghua521/article/details/78198019

###### **方法1：**

用root以ssh登录到linux，打开终端输入以下命令：

```bash
cd $ORACLE_HOME #进入到oracle的安装目录 

dbstart #重启服务器 

lsnrctl start #重启监听器 

cd $ORACLE_HOME #进入到oracle的安装目录

dbstart #重启服务器

lsnrctl start #重启监听器
```
-----------------------------------

###### **方法2：**

Sql代码
```bash
cd $ORACLE_HOME/bin #进入到oracle的安装目录 

./dbstart #重启服务器 

./lsnrctl start #重启监听器
```
-----------------------------------

###### **方法3：**
```bash
（1） 以oracle身份登录数据库，命令：su -oracle

（2） 进入Sqlplus控制台，命令：sqlplus /nolog

（3） 以系统管理员登录，命令：connect / as sysdba

（4） 启动数据库，命令：startup

（5） 如果是关闭数据库，命令：shutdown immediate

（6） 退出sqlplus控制台，命令：exit

（7） 进入监听器控制台，命令：lsnrctl

（8） 启动监听器，命令：start

（9） 退出监听器控制台，命令：exit
```
#### 重启Oracle实例
```bash
（1） 切换需要启动的数据库实例：export ORACLE_SID=C1

（2） 进入Sqlplus控制台，命令：sqlplus /nolog

（3） 以系统管理员登录，命令：connect / as sysdba

（4） 如果是关闭数据库，命令：shutdown abort

（5） 启动数据库，命令：startup

（6） 退出sqlplus控制台，命令：exit
```

```markdown
## Oracle-重启服务实例

sqlplus / as sysdba
# 或者
sqlplus /nolog；
conn sys / as sysdba

shutdown immediate ;
startup ;

```

