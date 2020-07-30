## Linux安装LibOffice

--参考文档
https://blog.csdn.net/sheqianweilong/article/details/84475315



#### 文件准备

--国内镜像源  中国科技大学
http://mirrors.ustc.edu.cn/tdf/libreoffice/stable/
http://mirrors.ustc.edu.cn/tdf/libreoffice/stable/

#### 文件下载

需要下载3个文件

```properties
LibreOffice_6.3.5_Linux_x86-64_rpm.tar.gz
LibreOffice_6.3.5_Linux_x86-64_rpm_sdk.tar.gz
LibreOffice_6.3.5_Linux_x86-64_rpm_langpack_zh-CN.tar.gz
```
#### 检查是否安装

```bash
# 查看libreoffice的版本
libreoffice version

# 查看libreoffice的安装位置
which libreoffice
```


#### wget方式下载安装

centos有外网情况下，直接复制以下代码

```bash

wget http://mirrors.ustc.edu.cn/tdf/libreoffice/stable/6.3.5/rpm/x86_64/LibreOffice_6.3.5_Linux_x86-64_rpm.tar.gz
wget http://mirrors.ustc.edu.cn/tdf/libreoffice/stable/6.3.5/rpm/x86_64/LibreOffice_6.3.5_Linux_x86-64_rpm_sdk.tar.gz
wget http://mirrors.ustc.edu.cn/tdf/libreoffice/stable/6.3.5/rpm/x86_64/LibreOffice_6.3.5_Linux_x86-64_rpm_langpack_zh-CN.tar.gz


cd /usr/local

wget http://mirrors.ustc.edu.cn/tdf/libreoffice/stable/6.3.5/rpm/x86_64/LibreOffice_6.3.5_Linux_x86-64_rpm.tar.gz

wget http://mirrors.ustc.edu.cn/tdf/libreoffice/stable/6.3.5/rpm/x86_64/LibreOffice_6.3.5_Linux_x86-64_rpm.tar.gz

#执行如下命令，解压文件（操作/usr/文件夹需要sudo权限）

sudo mkdir /usr/libreoffice
sudo tar -zxvf LibreOffice_6.3.5_Linux_x86-64_rpm.tar.gz -C /usr/libreoffice/
sudo tar -zxvf LibreOffice_6.3.5_Linux_x86-64_rpm_sdk.tar.gz -C /usr/libreoffice/
sudo tar -zxvf  LibreOffice_6.3.5_Linux_x86-64_rpm_langpack_zh-CN.tar.gz   -C /usr/libreoffice/

# 进入到的RPMS目录
cd RPMS 

#上面三个文件解压之后，/usr/libreoffice/下面会有3个文件夹,里面都有一个RPMS文件夹

LibreOffice_6.3.5_Linux_x86-64_rpm 
LibreOffice_6.3.5_Linux_x86-64_rpm_sdk 
LibreOffice_6.3.5_Linux_x86-64_rpm_langpack_zh-CN

# 分别进入到RPMS的目录下 ,执行入下命令
cd /usr/libreoffice/LibreOffice_6.3.5.2_Linux_x86-64_rpm/RPMS
cd /usr/libreoffice/LibreOffice_6.3.5.2_Linux_x86-64_rpm_sdk/RPMS
cd /usr/libreoffice/LibreOffice_6.3.5.2_Linux_x86-64_rpm_langpack_zh-CN/RPMS
```

#### yum方式安装
```bash

sudo yum localinstall *.rpm

# 执行以上步骤，默认会将libreoffice的可执行安装在/opt/lib/libreoffice/…soffice.bin,也可以使用yum来指定安装路径:

yum -c /etc/yum.conf --installroot=/usr/local --releasever=/  localinstall *.rpm

该命令简单解释如下：
-c /etc/yum.conf 表示指定yum配置文件地址
–installroot=/usr/local 表示指定自定义的安装目录

啰嗦一句：libreoffice不能进行批量转格式，因此可能会需要尝试到安装多个软件，或者使用docker来实现批量转化，但目前公司由于网络限制docker安装、镜像拉取上存在很大难度，故此先搁浅了。

注：用yum来进行rpm的安装，不要用rpm命令来进行安装。因为有依赖关libgnomevfs-2.so.0(64bit)，它被软件包 libobasis5.0-gnome-integration-6.1.3-2.x86_64 需要，所以不要使用rpm命令来进行安装， rpm -ivh *.rpm 命令无法解决上面的依赖系。使用yum遇到上面的依赖关系的时候可以从网络下载相应的包来解决依赖关系。

如果顺利，到此为止，libreoffice成功安装了.

libreoffice的使用：
soffice --headless --convert-to 目标格式（如pdf） 转格式文件 --outdir 目标文件夹
--libreoffice6.0 完成格式转换
libreoffice6.0 --invisible --convert-to pdf:writer_pdf_Export --outdir  "/root/" "bb.xls"
--把test.doc转换成html，保存在test目录
libreoffice6.0 --invisible --convert-to html --outdir ./test test.doc 


--进入安装目录
/usr/local/opt/libreoffice6.3

/usr/local/opt/libreoffice6.3 --headless --accept=”socket,host=127.0.0.1,port=8100;urp;”- -nofirststartwizard &
/usr/bin/libreoffice6.3 --headless --accept=”socket,host=127.0.0.1,port=8100;urp;”- -nofirststartwizard &
/usr/local/opt/libreoffice6.3/program/soffice --headless --accept="socket,host=127.0.0.1,port=8100;urp;" --nofirststartwizard &
成功则返回pid
```

#### 配置环境
安装完成，使用soffice 命令 是找不到的，
进入 /etc/profile 配置环境变量。
LibreOffice 默认是安装在/opt/LibreOffice_6.3 下
本次安装到了/usr/local/opt/libreoffice6.3 下

vim /etc/profile 

在最后加入
#export LibreOffice_PATH=/opt/libreoffice6.3/program  #默认目录
 export LibreOffice_PATH=/usr/local/opt/libreoffice6.3/program  #本次安装目录
export PATH=$LibreOffice_PATH:$PATH

然后保存,执行生效命令

source /etc/profile

#### 问题处理
##### 问题1：ZLIB_1.2.3.4' not found 

解决方法：安装个新版的zlib
wget http://zlib.net/zlib-1.2.11.tar.gz 
tar zxf zlib-1.2.11.tar.gz
cd zlib-1.2.11 
./configure 
make && make install 
cp /usr/local/lib/libz.so.1.2.11 /lib64/ 
cd /lib64/ 
rm libz.so.1 
ln -s libz.so.1.2.11 libz.so.1

##### 问题2：/lib64/libz.so.1: version `ZLIB_1.2.3.3’ not found
解决方法:安装个新版的zlib
参考：https://blog.csdn.net/m0_37644085/article/details/86606546
https://blog.csdn.net/yuanchheneducn/article/details/51314255?locationNum=9

wget http://zlib.net/zlib-1.2.11.tar.gz 
tar zxf zlib-1.2.11.tar.gz 
cd zlib-1.2.11
./configure 
make && make install 

如果无root权限运行到此步，将该路径写入.bashrc下就可以了
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/xxx/zlib/zlib-1.2.11

##### 问题3：version `ZLIB_1.2.3.4' not found 解决方法
参考文档
https://blog.csdn.net/mxiawang/article/details/103256267/
https://blog.csdn.net/lucky_greenegg/article/details/51199209?utm_source=blogxgwz8

但是如果有root权限,可以运行一下命令
cd /lib/x86_64-linux-gnu/
cp libz.so.1 libz.so.1_copy
rm libz.so.1 
ln -s libz.so.1.2.11 libz.so.1

安装个新版的glibc 守的选择2.15
wget http://ftp.gnu.org/gnu/glibc/glibc-2.15.tar.gz  
wget http://ftp.gnu.org/gnu/glibc/glibc-ports-2.15.tar.gz 
tar -xvf  glibc-2.15.tar.gz   
tar -xvf  glibc-ports-2.15.tar.gz  
mv glibc-ports-2.15 glibc-2.15/ports  
mkdir glibc-build-2.15    
cd glibc-build-2.15   
../glibc-2.15/configure  --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin  
make  
make install  

3、
#/lib64/libz.so.1: version `ZLIB_1.2.3.4' not found
#/lib64/libz.so.1: version `ZLIB_1.2.3.4' not found
#cd /usr/local/lib/libz.so.1.2.11

#/usr/local/opt/libreoffice6.3/program/oosplash
#/usr/local/zlib-1.2.11
#cd /usr/local/lib/libz.so.1.2.11
#cd /usr/local/zlib-1.2.11

#mkdir -p /lib/x86_64-linux-gnu/
#cp /usr/local/lib/libz.so.1.2.11 /lib/x86_64-linux-gnu/
#cp /usr/local/lib/libz.so.1.2.11 /lib/x86_64-linux-gnu/
#cd /lib/x86_64-linux-gnu/
#ln -s -f /usr/local/lib/libz.so.1.2.11/lib libz.so.1

cp /usr/local/lib/libz.so.1.2.11 /lib64/ 
cd /lib64/ 
rm libz.so.1 
ln -s libz.so.1.2.11 libz.so.1





> LiberOffice安装参考文档
> https://blog.csdn.net/diyiday/article/details/79852923