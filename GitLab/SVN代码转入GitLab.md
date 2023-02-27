# SVN 转 Git

> 代码库的转换

- 如何保留 svn日志
- 如何将SVN库 迁移到 gitLab 库
- 


### 方法一
``` bash
#svn地址： svn://192.168.1.254/www/Project/Source/acTool

#开始之前的准备
$ svn --version
$ git --version  #没有安装需要安装 参考下一节
$ perl --version


cd /home/
mkdir svn-git
cd /home/svn-git

git svn clone http://192.168.1.254/www/Project/Source/acTool  --no-metadata -T trunk -b branches -t tags

git svn clone http://192.168.1.254/www/Project/Source/acTool --username=admin --no-metadata --authors-file=users.txt

```

### 方法二
```bash
cd /home/svn-git

svn log URL -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > users.txt


svn log svn://192.168.1.254/www/Project/Source/acTool -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > users.txt


git svn clone svn://192.168.1.254/www/Project/Source/acTool --trunk="trunk" --tags="tags" --branches="branches" --authors-file=./users.txt --no-metadata 


git svn clone svn://192.168.1.254/www/Project/Source/acTool --no-metadata --authors-file=users.txt GitProject


# git 项目仓库地址 ssh://git@192.168.1.252:220/root/AcTool.git
# 进入项目目录
cd AcTool

# 关联远程仓库

git remote add origin ssh:git@192.168.1.252:220/root/AcTool.git
git remote add origin git@192.168.1.252:220/gaolongfei/AcTool.git
git remote add origin ssh://git@192.168.1.252:220/root/AcTool.git

git remote -v
git remote -v

# 显示如下
# origin  git@192.168.1.252:220/root/AcTool.git (fetch)
# origin  git@192.168.1.252:220/root/AcTool.git (push)

# 关联错了 重新关联 删除关联
git remote rm origin


```


> 参考文档 
>
> https://www.cnblogs.com/goodwell21/p/10044818.html
>
> https://www.cnblogs.com/goodwell21/p/10044818.html
>
> https://www.cnblogs.com/wuer888/p/7655856.html
>
> https://blog.csdn.net/shmely/article/details/82848964
>
> [https://git-scm.com/book/zh/v2/Git-%E4%B8%8E%E5%85%B6%E4%BB%96%E7%B3%BB%E7%BB%9F-%E8%BF%81%E7%A7%BB%E5%88%B0-Git](https://git-scm.com/book/zh/v2/Git-与其他系统-迁移到-Git)
>
> https://www.cnblogs.com/wzd5230/p/9638117.html
>
> https://www.cnblogs.com/ronli/p/git_svn_clone.html
>
> https://www.cnblogs.com/vwater/p/10332838.html
>
> https://www.jianshu.com/p/a68ff08f5856
>
> 

```bash
# 智慧安监平台迁移

# 准备工作
# 项目svn地址 svn://192.168.1.254/www/研发企业版/04源代码/trunk/IntellSecurity
# gitlab地址 http://192.168.1.252:8085/root/IntellSecurity.git
# gitlab ssh地址 ssh://git@192.168.1.252:220/root/IntellSecurity.git

# 新建文件夹 用于存放 从svn 获取的最新代码 以及历史记录，并用于这个项目的git 本地仓库
# d:\svn-git\
# 右键 git bash here 已经安装了git客户端
# 生成项目的svn用户与git用户对照表
$ svn log svn://192.168.1.254/www/研发企业版/04源代码/trunk/IntellSecurity -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > users.txt



```


> Arch linux 安装 git

```bash
sudo pacman -S git

#报错
#错误：无法打开文件 /var/cache/pacman/pkg/git-2.24.1-4-x86_64.pkg.tar.zst: Unrecognized archive format

# 删除下载的tar压缩包重新下载
cd /var/cache/pacman/pkg/
rm -rf git-2.24.1-4-x86_64.pkg.tar.zst

#重新安装 git
sudo pacman -S git

#安装报错
#发生错误，没有软件包被更新。
#pacman: symbol lookup error: /usr/lib/libcurl.so.4: undefined symbol: SSL_COMP_free_compression_methods

#怀疑是

# 查找
cd /usr/lib/

#
ls -l
ls -l  |more  #一屏显示不下 多屏显示  或者输出到文件 ls -l  > a.txt
ls -al |more

# libcurl.so.4 -> libcurl.so.4.4.0

#删除
rm -rf libcurl.so.4

#再次安装
pacman -S git

#报错
#error while loading shared libraries: libcurl.so.4

find . -name "libcurl"
find . -name "libcurl*"
find . -name "libcurl.so.4"


cd /usr/lib64
find . -name "libcurl*"
find . -name "libcurl.so.4.4.0"

# 创建软连接
ln -s libcurl.so.4.4.0 libcurl.so.4 


# 重新安装libcurl
# 参考文档 
# https://www.cnblogs.com/lantingg/p/8872314.html
# https://blog.csdn.net/qq_40340448/article/details/103707698
# https://blog.csdn.net/weixin_43938510/article/details/88143293
# 官网 https://curl.haxx.se/download.html 下载最新版本
wget https://curl.haxx.se/download/curl-7.70.0.tar.gz
tar -zxvf curl-7.70.0.tar.gz
cd curl-7.51.0
./configure
./configure --disable-ldap --disable-ldaps
make  
make install
curl

# 出现如下信息表示安装成功
curl: try 'curl --help' or 'curl --manual' for more information

$ curl --help

#重新安装 git
sudo pacman -S git

cd /
find -name *libcurl.so*


vim /etc/ld.so.conf
加入 /usr/local/lib/

/sbin/ldconfig -v

# 再次安装
$ pacman -S git

#仍然报错
#错误：无法打开文件 /var/cache/pacman/pkg/git-2.24.1-4-x86_64.pkg.tar.zst: Unrecognized archive format


#怀疑版本或者mirror 源的问题
#https://www.cnblogs.com/yumtaoist/archive/2012/06/01/2531306.html

# 手动安装参考 
#https://www.cnblogs.com/rstyro/articles/10817855.html

# 手动安装
wget https://github.com/git/git/archive/v2.21.0.tar.gz

#安装依赖 arch-linux 不能使用yum 此步骤跳过
$ yum -y install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker

#解压
tar -zxvf v2.21.0.tar.gz

#进入解压目录
cd git-2.21.0/

#编译
make prefix=/usr/local/git all

#编译安装Git在/usr/local/git路径
make prefix=/usr/local/git install

#配置环境变量
# 编辑环境配置文件
vim /etc/profile

# 末尾添加
export PATH=/usr/local/git/bin:$PATH

# 立马生效
source /etc/profile

#测试git
git version #查看安装的git版本

```

