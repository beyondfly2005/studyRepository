# Git 服务器搭建

> 参考文档
>
> https://www.runoob.com/git/git-server.html
>
> https://www.cnblogs.com/showker/p/9456061.html
>
> https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664

### 1、安装Git

```bash
$ yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel
$ yum install git

# 创建git用户和组
$ groupadd git
$ useradd git -g git
```

### 2、创建证书登录

收集所有需要登录的用户的公钥，公钥位于id_rsa.pub文件中，把我们的公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

如果没有该文件创建它：

```bash
$ cd /home/git/
$ mkdir .ssh
$ chmod 755 .ssh
$ touch .ssh/authorized_keys
$ chmod 644 .ssh/authorized_keys
```

### 3、初始化Git仓库

选定一个目录作为Git仓库，假定是/home/git-repo/sample.git，在/home/gitrepo目录下输入命令：

```bash
$ cd /home
$ mkdir git-repo
$ chown git:git git-repo/
$ cd git-repo

$ git init --bare sample.git
Initialized empty Git repository in /home/git-repo/sample.git/
```



### 4、客户端克隆仓库

```bash
git clone git@192.168.44.129:/home/git-repo/sample.git
```