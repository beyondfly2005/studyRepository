# git 提交到 GitLab

> 参考文档  
>
> https://www.runoob.com/git/git-branch.html
>
> https://blog.csdn.net/qq_34671951/article/details/89400082

```bash

# 1 新建或确定项目文件夹
# 2 gitlab复制 ssh协议的 git远程仓库地址

# 在项目文件夹中 右键 git bash here
# 初始化项目
git init


# 克隆项目
git clones ssh://git@192.168.1.252:220/gaolongfei/hello-gitlab.git
git pull ssh://git@192.168.1.252:220/gaolongfei/hello-gitlab.git

# 新建 index.html文件
touch index.html

git add index.html
git add * 

git commit -m 'new inedex.html'
git commit -m '新建/创建/添加 inedex.html  作为项目首页'


# git add 后取消
git reset HEAD <file>

# 提交到远程
git push origin

# 拉取远程代码
git pull

# 提示使用 git remote add <name> <url> 来设置远程仓库地址
# git remote add acserver ssh://git@192.168.1.252:220/gaolongfei/hello-gitlab.git
git remote add origin ssh://git@192.168.1.252:220/gaolongfei/hello-gitlab.git

#系统配置

# git config --global user.name "椰子"
# git config --global user.email "995852922@qq.com"

git config --global user.name "GaoLongfei"
git config --global user.email "498601485@qq.com"

# 推送到主分支
git push -u origin master


```



#### 什么是git

Git是一个免费的开源 分布式版本控制系统，旨在快速高效地处理从小型到大型项目的所有内容



#### 什么是GitLab

GitLab 是一个用于仓库管理系统的开源项目，使用[Git](https://baike.baidu.com/item/Git)作为代码管理工具，并在此基础上搭建起来的web服务。



#### 为什么要使用 git

- 主流的版本管理工具
- 分布式 支持远程
- 强大的分支管理
- 多种流程管理

#### git与SVN的区别

Git 不仅仅是个版本控制系统，它也是个内容管理系统(CMS)，工作管理系统等。

Git 与 SVN 区别点：

- **1、Git 是分布式的，SVN 不是**：这是 Git 和其它非分布式的版本控制系统，例如 SVN，CVS  VSS等，最核心的区别。
- **2、Git 把内容按元数据方式存储，而 SVN 是按文件：**所有的资源控制系统都是把文件的元信息隐藏在一个类似 .svn、.cvs 等的文件夹里。
- **3、Git 分支和 SVN 的分支不同：**分支在 SVN 中一点都不特别，其实它就是版本库中的另外一个目录。
- **4、Git 没有一个全局的版本号，而 SVN 有：**目前为止这是跟 SVN 相比 Git 缺少的最大的一个特征。
- **5、Git 的内容完整性要优于 SVN：**Git 的内容存储使用的是 SHA-1 哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。

#### git 安装配置

- git安装

  

- Tortoise git安装

  - 中文包安装

- 配置

```bash
## git配置用户信息
## 配置个人的用户名称和电子邮件地址：
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com


## git配置默认的文本编辑器
## git默认使用的文本编辑器, 一般可能会是 Vi 或者 Vim。如果你有其他偏好，比如 Emacs 的话，可以重新设置：
$ git config --global core.editor emacs

## git配置差异分析工具
## git提供了kdiff3，tkdiff，meld，xxdiff，emerge，vimdiff，gvimdiff，ecmerge，和 opendiff 等分析对比合并工具；可以指定在解决合并冲突时使用哪种差异分析工具。比如要改用 vimdiff 的话：
$ git config --global merge.tool vimdiff

## 查看配置信息
## 要检查已有的配置信息，可以使用 git config --list 命令：
git config --list

## 编辑配置文件
vim ~/.gitconfig 

## 直接查阅某个环境变量的设定，只要把特定的名字跟在后面即可，像这样：
$ git config user.name

$ #删除本地仓库  删掉.git文件夹
$ rm -rf .git

```



#### git 基本概念

##### 工作区、暂存区、版本库

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

##### 工作区、暂存区和版本库之间的关系

![img](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

##### 本地仓库与远程仓库

![img](https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2589547508,3390837051&fm=26&gp=0.jpg)

#### git 常用命令

- git init
- git clone 
- git pull
- git add
- git status
- git diff
- git commit
- git push 
- git branch
- git merge
- git log
- git ... 

#### git 分支管理

Git分支管理模型被称为**必杀技**特性，正因为此 Git从多个版本控制系统中区分出来

```bash
# 创建分支命令：
git branch (branchname)

# 切换分支命令:
git checkout (branchname)
## 当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

# 合并分支命令:
git merge
# 你可以多次合并到统一分支， 也可以选择在合并之后直接删除被并入的分支

# 列出分支 *代表当前分支
git branch

git checkout -b (branchname) 命令来创建新分支并立即切换到该分支
#等同于 git branch branchname + git checkout branchname
 
# 列出分支
git branch   # 列出本地分支
git branch -a  # 列出所有本地和远程分支
git branch -r  # 列出远程分支

# 删除分支命令：
git branch -d (branchname)

# 合并分支
git merge branchname
# 将分支branchname 合并到当前分支

# 合并冲突
git diff
```

###### 远程分支

```bash
# 查看远程分支
git branch -r

# 推送分支
git push origin sub1
git push upstream sub1

# 拉取远程分支
git pull upstream sub1

# 合并远程分支
# XXXX 没办法直接进行 或者专属的命令
# 拉取两个分支进行合并，然后推送到远程


#————————————————————————————————————————

# 查看远程库的信息
git remote

# 查看远程库的更多更详细信息
git remote -v

# 推送分支
# 推送分支，就是把该分支上的所有本地提交推送到远程库
git push origin master

# 推送其他分支 如sub1
git push origin sub1

# 如果远程库不叫origin 叫upstream
git push upstream sub1


# 显式地获得远程引用的完整列表
git ls-remote upstream

# 获得远程分支的更多信息
git remote show upstream

# 删除远程分支


```



#### git 日志管理

使用 Git 提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，我们可以使用 git log 命令查看

```bash
git log

# 查看历史记录的简洁的版本
git log --oneline

# --graph 选项，查看历史中什么时候出现了分支、合并。以下为相同的命令，开启了拓扑图选项
git log --graph

# --reverse 参数来逆向显示所有日志
git log --reverse --oneline

# 查找指定用户的提交日志可以使用命令：git log --autho
git log --author=Linus --oneline -5

#也可以指定日期，可以执行几个选项：--since 和 --before，但是你也可以用 --until 和 --after。
git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges

```



#### git 标签管理

如果你的项目/代码达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以使用 git tag 给它打上标签。

比如发布一个版本时，我们通常先在版本库中打一个标签（tag），这样就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来

```bash
#打标签
git tag -a v1.0

#追加标签  85fc7e7是提交id号
git tag -a v0.9 85fc7e7

#查看标签
git tag

#指定标签信息命令：
git tag -a <tagname> -m "runoob.com标签"

#PGP签名标签命令：
git tag -s <tagname> -m "runoob.com标签"

```

####  git 远程仓库

GitHub

Gitee

GitLab

BitBucket

开源中国 http://git.oschina.net/

coding.net https://coding.net/home.html

CSDN https://code.csdn.net/

京东 https://code.jd.com/

阿里云Code托管平台https://code.aliyun.com/

百度效率云 http://xiaolvyun.baidu.com/

腾讯云开发者平台 https://dev.tencent.com/

华为开源平台https://code.opensource.huaweicloud.com/

```bash
# 添加远程仓库
git remote add [shortname] [url]
git remote add origin 
git remote add gitlab

# git免密登录
# 生成秘钥
ssh-keygen -t rsa -C "youremail@example.com"

#查看远程仓库
git remote

# -v 参数，每个别名的实际链接地址
git remote -v

#从远程仓库下载新分支与数据：
git fetch

# 从远端仓库提取数据并尝试合并到当前分支
git merge

# 拉取 git pull 与 git fetch 和git merge的关系
git pull
# git pull= git fetch + git merge

# 拉取远程分支 
git fetch origin
git fetch origin master

# 同步远程分支
git merge origin/master

#推送你的新分支与数据到某个远端仓库命令:
# git push [alias] [branch]
git push origin sub1

# 删除远程仓库你可以使用命令
git remote rm [别名]

```


#### git 常用图形化工具

- sourceTree

- Tortoise git

####　git 在idea中的使用

- **更新代码**

  Subsersion -> Update Directory

  Subsersion -> Update File

  

  Git -> Repository -> Pull

  

- **新建文件添加到svn**

  Subversion - > Add to VCS

  

  Git 添加时自动提示

  Git -> Add

  

- **忽略新添加的文件**

  Subversion - > Ignore

  Git - > Add to .gitignore

   

- **提交代码**

  Subversion -> Commit Directory

  Subversion -> Commit  File

  

  Git  -> Commit File

  Git  -> Commit Directory

  注意区分：Commit 和Commit And Push



- **从代码库打开工程**

  Subversion -> SVN Checkout

  

  git init

  git clone / git pull



#### git 流程管理

​	git 工作流程

> 参考文档 https://blog.csdn.net/slowlifes/article/details/79569084

​	常用的三种工作流程

- Git flow
- Github flow
- Gitlab flow

Git flow工作流

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015122302.png)

```bash

```



#### git 常见问题

###### git fetch 和 git pull的区别

`git fetch` 命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。 它只会获取数据然后让你自己合并。 然而，有一个命令叫作 `git pull` 在大多数情况下它的含义是一个 `git fetch` 紧接着一个 `git merge` 命令。 如果有一个像之前章节中演示的设置好的跟踪分支，不管它是显式地设置还是通过`clone` 或 `checkout` 命令为你创建的，`git pull` 都会查找当前分支所跟踪的服务器与分支， 从服务器上抓取数据然后尝试合并入那个远程分支。

由于 `git pull` 的魔法经常令人困惑所以通常单独显式地使用 `fetch` 与 `merge` 命令会更好一些。



## Idea中使用Git

git add

git repositry  reet HEAD

git commit

git commit and push

git repositry  branch 



## TortoiseGit

