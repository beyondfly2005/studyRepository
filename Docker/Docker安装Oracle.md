## Docker 安装 Oracle

> --参考文档
> https://www.cnblogs.com/OliverQin/p/9765808.html
> https://www.cnblogs.com/OliverQin/p/9765808.html
> https://www.cnblogs.com/baojunblog/p/11340258.html
> https://www.35youth.cn/685.html
> https://www.linuxidc.com/Linux/2018-08/153684.htm
> https://www.jianshu.com/p/42a2df318f59
> https://blog.csdn.net/bbwangj/article/details/82180620

### 

#### 检查Docker

```bash
# 查看docker是否安装
docker version
```


```tex
#如果输出以下信息 表明docker已经安装

Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b
 Built:             Wed Mar 11 01:27:04 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b
  Built:            Wed Mar 11 01:25:42 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
[root@localhost ~]# 
```

#### 配置Docker加速器
/etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）：

```bash
vim /etc/docker/daemon.json
{"registry-mirrors":["https://reg-mirror.qiniu.com/"]}

--重启docker
sudo service docker restart
```
#### 选择Oracle版本
```bash
# 查找可用docker
$ docker search oracle
```
	备选如下：
		oraclelinux                           Official Docker builds of Oracle Linux.         646                     [OK]                
		jaspeen/oracle-11g                    Docker image for Oracle 11g database            157                     [OK]
		oracleinanutshell/oracle-xe-11g                                                       92                                      
		oracle/graalvm-ce                     GraalVM Community Edition Official Image        65                      [OK]
		absolutapps/oracle-12c-ee             Oracle 12c EE image with web management cons…   38                                  
		araczkowski/oracle-apex-ords          Oracle Express Edition 11g Release 2 on Ubun…   30                      [OK]
		oracle/nosql                          Oracle NoSQL on a Docker Image with Oracle L…   24                      [OK]
		bofm/oracle12c                        Docker image for Oracle Database                23                      [OK]
		datagrip/oracle                       Oracle 11.2 & 12.1.0.2-se2 & 11.2.0.2-xe        19                      [OK]
		truevoly/oracle-12c                   Copy of sath89/oracle-12c image (https://git…   13                                     
		oracle/weblogic-kubernetes-operator   Docker images containing the Oracle WebLogic…   12                                      
		openweb/oracle-tomcat                 A fork off of Official tomcat image with Ora…   8                       [OK]
		iamseth/oracledb_exporter             A Prometheus exporter for Oracle modeled aft…   3                                       
		paulosalgado/oracle-java8-ubuntu-16   Oracle Java 8 on Ubuntu 16.04 LTS.              2                       [OK]
		oracle/coherence-operator             Kubernetes Operator for Oracle Coherence        2                                       
		softwareplant/oracle                  oracle db                                       2                       [OK]
		18fgsa/oracle-client                  Hosted version of the Oracle Container Image…   2                                       
		publicisworldwide/oracle-core         This is the core image based on Oracle Linux…   1                       [OK]
		roboxes/oracle7                       A generic Oracle Linux 7 base image.            1                                       
		bitnami/oraclelinux-runtimes          Oracle Linux runtime-optimized images           0                       [OK]
		arm64v8/oraclelinux                   Official Docker builds of Oracle Linux.         0                                       
		pivotaldata/oracle7-test              Oracle Enterprise Linux (OEL) image for GPDB…   0                                      
		bitnami/oraclelinux-extras            Oracle Linux base images                        0                       [OK]
		toolsmiths/oracle7-test                                                               0                                      
		amd64/oraclelinux                     Official Docker builds of Oracle Linux.         0                                       

```bash

--PS ：网上很多教程中的镜像已经没有了
--比如：sath89/oracle 比如：alexeiled/docker-oracle-xe-11g

--选择安装 jaspeen/oracle-11g
```



#### 拉取Docker镜像文件

```bash
$ docker pull jaspeen/oracle-11g  需要准备oracle安装文件
$ #推荐使用
$ docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```



####　创建Docker容器

```bash
#--创建 启动容器(启动花了一分钟唉，有点慢哦)
$ docker run -d --name oracle11g -p 1521:1521 registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g -v /opt/data/oracle:/home/oracle/app/oracle/oradata/
# docker run -d --name oracle11g -p 8080:8080 -p 1521:1521 registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g -v /opt/data/oracle:/home/oracle/app/oracle/oradata/
# docker run -d --name oracle11g -p 8080:8080 -p 1521:1521 registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g -v /opt/data/oracle:/u01/app/oracle sath89/oracle-12c

# 如果想将数据文件映射到主机方便管理，可以在启动容器时添加参数
$ docker run -d --name oracle11g -p 1521:1521  registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g -v /opt/data/oracle:/home/oracle/app/oracle/oradata/

# 第一次启动使用 docker run 第一次 第一次
docker run
# 之后使用
docker start
```
#### 启动Docker容器
```bash
# 启动容器里面的oracle
docker start oracle11g

# 查看运行的docker
docker ps -a

# 修改名称
docker rename  xxx oracle

# 启动容器

# 以后再启动 使用start
docker start oracle11g
```
#### 配置环境变量
```bash
# 进入容器
# 4.进入镜像配置
docker exec -it oracle11g bash
```


```bash
5.配置Oracle环境变量
切换到root用户
su - root
用户名：root
密码：helowin


[root@a8a161b66e1d /]# vim /etc/profile
在文件末未添加
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export ORACLE_SID=helowin
export PATH=$ORACLE_HOME/bin:$PATH


#配置sqlplus使用oracle
Linux下Oracle环境变量设置
编辑.bashrc 或者 .bash_profile，添加如下：

export ORACLE_SID=helowin
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export PATH=$ORACLE_HOME/bin:$PATH

# 这样就可以在oracle用户下输入sqlplus进入命令行客户端啦。

```

#### 连接到数据库

```bash
6.创建软链接
ln -s $ORACLE_HOME/bin/sqlplus /usr/bin

切换到oracle用户下（注意中间有-）
su - oracle
```

sqlplus 连接 数据库
```sql
[oracle@a8a161b66e1d ~]$ sqlplus /nolog

SQL*Plus: Release 11.2.0.1.0 Production on Wed Oct 10 12:54:06 2018

Copyright (c) 1982, 2009, Oracle.  All rights reserved.
SQL> conn / as sysdba
Connected.

SQL> alter user system identified by system;
User altered.

SQL> alter user sys identified by sys;
User altered.

SQL> create user rootep identified by lam123;
--alter user rootep identified by lam123;
User created.

SQL> grant connect,resource,dba to rootep;
Grant succeeded.

8.配置本地tnsnames.ora文件
文件位置
/home/oracle/app/oracle/product/11.2.0/dbhome_2/network/admin
复制代码
LS =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 121.78.157.187)(PORT = 1521))
    )
    (CONNECT_DATA =
       (SERVER = DEDICATED)
      (SERVICE_NAME = helowin)

    )
  )
```
```bash
#--如何连接到数据库
docker exec -it oracle11g bash
su - oracle 
# 密码oracle
sqlplus / as sysdba
```


#### 如何重启oracle监听

```bash
# 切换到oracle用户
$ lsnrctl start 
$ lsnrctl stop  
$ lsnrctl status 

# 进入容器内部
docker exec -it oracle11g bash

#切换到oracle账号  #密码oracle
su -oracle


lsnrctl status
lsnrctl status
lsnrctl stop
lsnrctl start

#使用sqlplus连接
sqlplus / as sysdba

sqlplus / as sysdba

sqlplus rootep/lam123@192.168.1.252:1521/orcl
sqlplus rootep/lam123@192.168.1.252:1521/HELOWIN

sqlplus rootep/lam123@192.168.1.252:1521/helowin
```

```bash

#--错误处理
TNS-12543: TNS:destination host unreachable
参考文档 https://blog.csdn.net/yabingshi_tech/article/details/46789183

# 原因：1521端口没有开放
# 开放 oracle 1521 端口
firewall-cmd --zone=public --add-port=1521/tcp --permanent
--重新加载
firewall-cmd --reload

```

```bash
# 停止防火墙
#停止运行： 
systemctl stop firewalld
# 启动： 
systemctl start firewalld
#查看状态： 
systemctl status firewalld 
#禁用，禁止开机启动： 
systemctl disable firewalld

```


#### 重启Oracle数据库

```sql
-- 重启oracle 数据库

--进入到容器内部
docker exec -it oracle11g bash

--连接数据库
sqlplus / as sysdba

-- 立即停止oracle
shutdown immediate ;

-- 启动数据库
startup ;
```



####　磁盘空间不足

```bash
# 查看整个硬盘大小
fdisk -l |grep Disk

# 查看所有分区
fdisk -l

# 查看系统的磁盘分配，已使用和可用情况
df -hl

# 可以查看每个文件夹的大小
du -sh *

# 查看当前目录下的文件和文件夹数
ls |wc -l

```



#### 重启Docker

```bash
# 停止docker服务
systemctl stop docker
# 重启docker
systemctl daemon-reload
 
systemctl restart docker
 
systemctl enable docker
```



#### Docker启动Oracle

```bash
# Docker启动Oracle
$ docker start oracle11g

$ docker start oracle11g

```



#### 错误处理

ORA-12514, TNS:listener does not currently know of service requested in connect descriptor
检查IP端口 服务名 是否正确 本次是ip输错了
检查tnsnames.ora配置是否正确
位置/home/oracle/app/oracle/product/11.2.0/dbhome_2/network/admin
参考文档https://www.cnblogs.com/baojunblog/p/11340258.html

导入dmp文件时怎么都不成功，报：
IMP-00058: ORACLE error 12545 encountered的错，多方查找是需要修改安装时的tnsnames.ora文件，
修改host为127.0.0.1 后才能导入!
参考文档 https://www.dazhuanlan.com/2019/09/27/5d8dc97563894/


