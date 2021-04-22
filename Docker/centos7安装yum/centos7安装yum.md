##### 1、删除原有的yum

命令：

```
rpm -aq|grep yum|xargs rpm -e --nodeps
```



##### 2、根据系统版本 下载安装包

在浏览器中打开 http://mirrors.163.com/centos/7/os/x86_64/Packages/ ，找到以下四个文件：

```yaml
yum-*.rpm
yum-metadata-parser-*.rpm
yum-plugin-fastestmirror-*.rpm
python-iniparse-*.rpm
```

其中，*代表安装包版本
可以在windows系统上下载，然后上传至linux中，也可以使用命令： wget http://mirrors.163.com/.help/CentOS7-Base-163.repo 直接下载到linux中。


##### 3、安装yum

命令：rpm -ivh http://mirrors.163.com/centos/7/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm

安装包相互有依赖，安装时需要注意顺序：

```
1、安装python-iniparse
2、安装yum-metadata-parser
3、yum和yum-plugin-fastestmirror一起安装

```

4、运行makecache 生成缓存
命令：yum makecache

##### 4、运行makecache 生成缓存

命令：yum makecache

##### 5、运行yum clean all

命令：yum clean all

##### 6、更新yum文件

命令：yum update

##### 7、检查是否安装

命令：

```
yum install perl-DBI
```


提示：软件包 perl-DBI-1.627-4.el7.x86_64 已安装并且是最新版本，无须任何处理 ，表示安装成功。

记录我在centOS CentOS-7-x86_64-DVD-1810.iso 上安装yum的全过程，如果本机上已有yum,可以使用yum update 更新版本。

##### 8、使用

安装docker
Docker 软件包已经包括在默认的 CentOS-Extras 软件源里。因此想要安装 docker，只需要运行下面的 yum 命令：yum install docker -y