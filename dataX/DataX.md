docker pull gitlabcezh/gitlab-ce-zh



> 视频地址

https://www.bilibili.com/video/BV1QV411d7at?p=1

> 参考文档

https://blog.csdn.net/inthat/article/details/84146346




# DataX

## dataX概述

DataX是一个异构数据源离线同步/迁移工具，致力于关系型数据库MySQL Oracle等各种异构数据库直接稳定高效的数据迁移

MySQL、Oracle

### DataX安装

#### 1、前置条件

- **Linux**
- **JDK(1.8以上，推荐1.8)**
- **Python(推荐Python2.6.X)**
- **Apache Maven 3.x (Compile DataX)**

```bash
# 查看linux版本
cat /etc/issue
cat /etc/redhat-release

# 查看JDK版本
java -version

#查看python版本
python -V

#查看maven版本
mvn -v
```

### 安装DataX

#### 安装方法：

- **DataX工具包工具包安装**
- **DataX源码安装**

#### 1、下载安装包  

下载地址：http://datax-opensource.oss-cn-hangzhou.aliyuncs.com/datax.tar.gz

安装手册：https://github.com/alibaba/DataX/blob/master/userGuid.md

#### 2、上传安装包
#### 3、解压安装包

```bash
tar -zxvf datax.tar.gz  -C /opt/datax/
cd /opt/datax
chmod -R 755 datax
```

#### 4、自检测试

```bash
## 自检脚本
python {YOUR_DATAX_HOME}/bin/datax.py {YOUR_DATAX_HOME}/job/job.json
cd /home/datax
#或者
cd /usr/local/datax
python ./datax/bin/datax.py ./datax/job/job.json
python /home/datax/datax/bin/datax.py  /home/datax/datax/job/job.json
python /opt/datax/datax/bin/datax.py  /opt/datax/datax/job/job.json


cd datax/bin
#会给你返回json模板格式
python datax.py -r mysqlreader -w mysqlwriter
#python datax.py ../job/job.json
```

#### 5、错误处理

##### **报错1**

```
  File "datax.py", line 114
    print readerRef
                  ^
SyntaxError: Missing parentheses in call to 'print'
```

**原因分析**

python2的print在python3中变为了print（）函数。

因此：**必须使用Python2！**

安装python2，并用 py -2 来执行python代码

```
py -2 datax.py -r mysqlreader -w mysqlwriter
```

##### 报错2

```bash
python.bin: /lib64/libc.so.6: version `GLIBC_2.17' not found 
```

**原因分析**

原因是未安装2.17版本的glibc库

**解决方案**

```bash
## 下载glibc并解压缩
wget https://ftp.gnu.org/gnu/glibc/glibc-2.17.tar.gz
tar -xvf glibc-2.17.tar.gz

## 编译安装
cd glibc-2.17
mkdir build
cd build
../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
make && make install

## 查看glibc共享库
ll /lib64/libc.so.6

## 现libc.so.6已经软链接到2.17版本
lrwxrwxrwx 1 root root 12 7月 21 10:11 /lib64/libc.so.6 -> libc-2.17.so

## 可以查看系统中可使用的glibc版本
strings /lib64/libc.so.6 |grep GLIBC_
```



### 安装Python2.6

#### 1、前置条件

安装python，需要用到gcc gcc-c++ autoconf automake 来安装编译环境

- ##### ArchLinux安装

```
pacman -S gcc 
pacman -S gcc-c++ 
pacman -S autoconf 
pacman -S automake
```

- ##### CentOS安装

```
yum install gcc gcc-c++ autoconf automake
```

#### 2、使用wget下载python

```bash
 wget http://www.python.org/ftp/python/2.6.6/Python-2.6.6.tgz
```

#### 3、解压python

```bash
tar xzf Python-2.6.6.tgz
cd Python-2.6.6
```

#### 4、编译安装python

```bash
 ./configure --prefix=/usr/local/python2.6
 make
 make install
```

#### 5、创建软连接

创建一个python2.6的链接

```bash
ln -sf /usr/local/python/bin/python2.6 /usr/bin/python2.6
```



