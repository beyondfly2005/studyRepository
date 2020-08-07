https://www.bilibili.com/video/BV1Wa4y1v7uA?from=search&seid=8006137873669384588



MySQL5.7的安装

#### 0、更换yum源

1、打开mirrors.aliyun.com，选择centos的系统点击帮助

2、执行命令：yum install wget -y

3、改变某些文件的名称

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

4、执行更换yum源的命令

```bash
weget -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
weget -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
```

5、更新本地缓存

```bash
yum clean all
yum makecache
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

grep "password"


```



















































