## MySQL5.7的安装

> 视频地址

https://www.bilibili.com/video/BV1Wa4y1v7uA?seid=8006137873669384588

https://www.bilibili.com/video/BV1Wa4y1v7uA?t=956



#### 0、更换yum源

1、打开mirrors.aliyun.com，选择centos的系统点击帮助

2、执行命令：yum install wget -y

3、改变某些文件的名称

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

4、执行更换yum源的命令

```bash
rm -f  /etc/yum.repos.d/CentOS-Base.repo
wget -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
rm -rf Centos-7.repo
```

5、更新本地缓存

```bash
yum clean all
yum clean all
yum makecache
yum makecache 
```

错误处理

```
File contains no section headers.
file: file:///etc/yum.repos.d/CentOS-Base.repo, line: 1
'--2020-08-08 17:25:05--  http://mirrors.aliyun.com/repo/Centos-7.repo\n
```

处理方法

```bash
# 先删除原有的文件
rm -f  /etc/yum.repos.d/CentOS-Base.repo
# 然后重新下载阿里的
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

#清理缓存
yum clean all

ps:如果上述方法没有解决，尝试下面 

## 删除yum.repos.d目录下所有文件
rm -f /etc/yum.repos.d/*

##然后重新下载阿里的
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

## 清理缓存
yum clean all

##测试下载安装
yum install gcc
```



#### 1、查看系统是是否自带安装mysql

```bash
yum list installed | grep mysql
```

#### 2、删除系统中自带的mysql及其依赖（防止冲突）

```bash
yum -y remove mysql-libs.x86_64
# 将上步命令查到的依赖全部删除
```

#### 3、安装wget 命令

```
yum install wget -y
```

#### 4、给CentOS添加rpm源，并且选择较新的源

```
wget dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
```

#### 5、安装下载好的rpm文件

```
yum install mysql-community-release-el6-5.noarch.rpm -y
```

#### 6、安装成功之后，会在/etc/yum.repos.d/文件夹下新增两个文件

```bash
mysql-community.repo
mysql-community-source.repo
```

#### 7、修改mysql-community.repo文件

将5.6版本的enable=0  将5.7版本的enable=1

```bash
vim mysql-community.repo
```



```properties
[mysql56-community]
name=MySQL 5.6 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql57-community-dmr]
name=MySQL 5.7 Community Server Development Milestone Release
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/6/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

#### 8、使用yum 安装MySQL

```bash
yum install mysql-community-server -y
```

问题处理1

```
错误：软件包：2:postfix-2.10.1-9.el7.x86_64 (@anaconda)
          需要：libmysqlclient.so.18()(64bit)
          正在删除: 1:mariadb-libs-5.5.65-1.el7.x86_64 (@anaconda)
              libmysqlclient.so.18()(64bit)
          取代，由: mysql-community-libs-5.7.31-1
```



解决方法：

```
yum remove mysql-libs
```



问题处理2

```
错误：软件包：mysql-community-server-5.7.31-1.el6.x86_64 (mysql57-community-dmr)
          需要：libsasl2.so.2()(64bit)
 您可以尝试添加 --skip-broken 选项来解决该问题
 您可以尝试执行：rpm -Va --nofiles --nodigest

```

处理方法

```bash
vim mysql-community.repo
#将
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/6/$basearch/
改为
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
```

#### 9、启动mysql服务并设置为开机启动

```properties
# 启动直接需要生成临时密码，需要用到证书，可能证书过期，需要进行更新
yum update -y
# 启动mysql服务
service mysqld start
#设置musql开机启动
chkconfig mysqld on
```

#### 10 、获取MySQL的临时密码

```properties
grep "password" /var/log/mysqld.log

```

#### 11、使用临时密码登录

```bash
mysql -uroot -p
```

#### 12、修改密码

```sql
set global validate_password_policy=0;
set global validate_password_length=1;
alter USER 'root'@'localhost' IDENTIFIED by '123456';

```

#### 13、修改远程访问权限

```sql
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
flush privileges;
```

#### 14、设置字符集为utf-8

```bash
vim /etc/my.cnf
```

```
#在[mysqld]部分添加：
character-set-server=utf8
#在文件末尾新增[client]段，并在[client]段增加；
[client]
default-character-set=utf8

```

```bash
#修改完后需要重启mysql服务
service mysqld restart
```

















































