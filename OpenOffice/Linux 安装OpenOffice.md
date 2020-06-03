1.http://www.openoffice.org/zh-cn/download/ 去官网链接下载linux版本的openOffice 以4.1.5 版本为例。
Apache_OpenOffice_4.1.7_Linux_x86-64_install-rpm_zh-CN.tar.gz

wget https://jaist.dl.sourceforge.net/project/openofficeorg.mirror/4.1.7/binaries/zh-CN/Apache_OpenOffice_4.1.7_Linux_x86-64_install-rpm_zh-CN.tar.gz
wget https://sourceforge.net/projects/openofficeorg.mirror/files/4.1.7/binaries/zh-CN/Apache_OpenOffice_4.1.7_Linux_x86-64_install-rpm_zh-CN.tar.gz

2.将压缩包上传至服务器上，并进行解压安装。
　　　　　　

1  tar -zxvf  对应的压缩包名字
2  cd 进入解压后的 /zh-cn/RPMS
3  yum localinstall *.rpm
4  cd desktop-integration
5  rpm -ivh openoffice4.1.5-redhat-menus-4.1.5-9789.noarch.rpm

默认会安装在/opt目录下。

3.启动服务　

1 /opt/openoffice4/program/soffice -headless -accept="socket,host=127.0.0.1,port=8100;urp;" -nofirststartwizard  临时启动
2 nohup /opt/openoffice4/program/soffice -headless -accept="socket,host=127.0.0.1,port=8100;urp;" -nofirststartwizard &  后台启动

查看进程

1
netstat -lnp |grep 端口号
大概显示成这样就算启动完了。

1
tcp        0      0 127.0.0.1:8100              0.0.0.0:*                   LISTEN      14362/soffice.bin



###  OPEN OFFICE 安装

> 安装文档
> https://www.cnblogs.com/ilvutm/p/11690390.html
> https://www.jianshu.com/p/db7733274da3
> https://www.cnblogs.com/fangts/p/11045564.html
> https://www.cnblogs.com/fangts/p/11045564.html
> https://blog.csdn.net/u013132051/article/details/53304562
> https://www.cnblogs.com/Oliver-rebirth/p/Linux_openOffice.html

#### Openoffice安装

```bash
#创建目录
mkdir -p /usr/local/openoffice
cd /usr/local/openoffice

#下载tar包
wget https://sourceforge.net/projects/openofficeorg.mirror/files/4.1.7/binaries/zh-CN/Apache_OpenOffice_4.1.7_Linux_x86-64_install-rpm_zh-CN.tar.gz

#如果出现 wget: command not found
yum -y install wget

#yum 安装太慢 或者全部失败
# 修改dns 域名服务 增加以下两行
vi /etc/resolv.conf 
nameserver 8.8.8.8
nameserver 114.114.114.114

#解压tar包
tar xf Apache_OpenOffice_4.1.7_Linux_x86-64_install-rpm_zh-CN.tar.gz -C /usr/local/openoffice
cd /usr/local/openoffice/zh-CN/RPMS

#安装相关的rpm包
rpm -ivh openoffice-*

#默认安装目录 /opt/openoffice4/program 中
#执行 
/opt/openoffice4/program/soffice -accept="socket,host=127.0.0.1,port=8100;urp;" -headless -nofirststartwizard &
/opt/openoffice4/program/soffice -accept="socket,host=127.0.0.1,port=8100:urp;" -headless 
#或者
cd /opt/openoffice4/program/
./soffice -headless -accept="socket,host=127.0.0.1,port=8100:urp;"
 -nofirststartwizard & 
#在命令的最后输入 & 可确保服务在后端运行


#注意
#若执行启动命令时报错 /opt/openoffice4/program/soffice.bin: error while loading shared libraries: libXext.so.6: cannot open shared object file: No such file or directory ，则需要安装 libXext 依赖包，根据 Linux 版本选择安装类型
#执行 
$ yum install libXext.x86_64
#在 /usr/lib64 或 /usr/lib 中找到 libXext.so.6 文件，复制到 /opt/openoffice4/program/ 目录中
$ cd /usr/lib64
$ find -iname libXext.so.6
$ cp libXext.so.6 /opt/openoffice4/program/
#对复制过来的文件执行 
$ cd /opt/openoffice4/program/
$ chmod 777 libXext.so.6

# 测试启动
/opt/openoffice4/program/soffice -accept="socket,host=127.0.0.1,port=8100;urp;" -headless -nofirststartwizard

# 报错
#no suitable windowing system found, exiting.

# 方法一 安装x-window
yum groupinstall "X Window System"

# 方法二 ubuntu系统可以尝试安装libxt6 and libxrender1
# apt-get install libxt6
# apt-get install libxrender


#关闭openOffice服务，
#命令：ps -ef|grep soffice

#4 启动openoffice
#临时启动
/opt/openoffice4/program/soffice -accept="socket,host=127.0.0.1,port=8100;urp;" -headless -nofirststartwizard

#放入后台永久运行
nohup /opt/openoffice4/program/soffice -headless -accept="socket,host=127.0.0.1,port=8100;urp;" -nofirststartwizard &

#加入到开机自启动
vim /etc/rc.local
nohup /opt/openoffice4/program/soffice -headless -accept="socket,host=127.0.0.1,port=8100;urp;" -nofirststartwizard &

#查看openoffice进程
ps -ef|grep soffice  或
ps -ef|grep openoffice

#启动之后 在program目录输入
netstat –tln查看是否启动成功！如上图所示有8100这个端口就可以使用了
netstat -lnp |grep 端口号

#--报错：Could not find a Java Runtime Environment!
#没有安装JDK

```

安装JDK参考文档
https://www.jianshu.com/p/aa59ca90ac10
第一步：查看系统版本是32位还是64位
[root@yzj ~]# file /sbin/init 或者 file /bin/ls
file /sbin/init 或者 file /bin/ls

第二步：对应版本先下载JDK1.8
下载jdk包（需要联网）——方法2
本章使用的为后缀为tar.gz的文件（不需要安装），如jdk-8u131-linux-x64.tar.gz
[root@yzj jdk]# wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz

第三步：先检查是否有安装JDK：
[root@yzj ~]# rpm -qa | grep jdk
先把系统自带的干掉：
[root@yzj ~]# rpm -e --nodeps java-1.7.0-openjdk-1.7.0.45-2.4.3.3.el6.x86_64
[root@yzj ~]# rpm -e --nodeps java-1.6.0-openjdk-1.6.0.0-1.66.1.13.0.el6.x86_64

第四步：上传jdk文件到linux系统上，这里使用Xshell进行文件上传

第六步：设置环境变量

打开配置文件：vi /etc/profile
[root@yzj jdk]# vim /etc/profile
2.拉到文章的末尾，按i进行编辑，在最后面添加如下代码：
（JAVA_HOME 是jdk安装目录，和在Windows下配置一样 )
export JAVA_HOME=/usr/local/jdk/jdk1.8.0_131（安装的具体目录）
export JAVA_HOME=/usr/local/jdk/jdk1.8.0_221
export PATH=$PATH:$JAVA_HOME/bin

第七步：设置配置文件立即生效
source /etc/profile

第八步：查看配置是否成功
[root@yzj jdk]# java -version
若出现jdk版本号，则安装并配置环境变量成功，如果提示命令找不到的话，查看一下jdk的配置路径是否错误。

--转换命令
/program/soffice.exe -headless -invisible --convert-to pdf e:\tmp\123.docx --outdir e:\tmp
/usr/bin/libreoffice6.0 --headless --invisible --convert-to pdf /tmp/123.docx --outdir /tmp

-PS
find -name 'Apache_OpenOffice_4.1.7_Linux_x86-64_install-rpm_zh-CN.tar.gz'  当前目录下查找
find -name '*OpenOffice*'  当前目录下模糊查找
find /usr -name '*OpenOffice*'  /usr目录下模糊查找




--openOffice安装参考文档
http://www.mamicode.com/info-detail-496968.html
https://www.iteye.com/blog/huangronaldo-1628339