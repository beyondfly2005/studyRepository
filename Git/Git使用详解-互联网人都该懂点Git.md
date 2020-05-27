# Git使用详解

>  互联网人都该懂点Git

- 01 Git 是什么	

  https://www.bilibili.com/video/BV1HW411f7VJ?seid=15192460350089393723

- 02 Git基本操作

  https://www.bilibili.com/video/BV13W411Z7G5/?spm_id_from=333.788.videocard.0

- 03 Git中的分支

  https://www.bilibili.com/video/BV19W411r73J/?spm_id_from=333.788.videocard.0

- 04 Git中的合并

  https://www.bilibili.com/video/BV1bW411z7J8/?spm_id_from=333.788.videocard.0

- 05 Git回滚和撤销
  https://www.bilibili.com/video/BV1TW41117iU/?spm_id_from=333.788.videocard.0
  
- 06 Gitignore 和 fork 同步

  https://www.bilibili.com/video/BV1Vb411A7z2

- 07 Git免密推送

  https://www.bilibili.com/video/BV1zb411c7jr?seid=15192460350089393723

- 08 Git工作流

  https://www.bilibili.com/video/BV12t411U716?seid=15192460350089393723

- 09 Git 常用图形化工具

  https://www.bilibili.com/video/BV1it411C7uH?seid=15192460350089393723



### Git 是什么

> 为什么要使用版本管理系统

在没有版本管理系统之前，如何管理文件的不同版本，如何多人共享文档

> Git是什么

Git是一个分布式版本管理系统，最初是为了管理linux内核

直接记录快照，而不是文件之间的对比，在存储之前会计算校验和，采用哈希算法 SH1

文件的增删改 都是增加了一个快照

> Git相关概念

**Git仓库**  *** git仓库*** 保存项目源目录 .git directory

**工作目录**  对当前项目的某一个版本，**暂存区**  commit  -> 保存到 git仓库



**本地仓库**  

**远程仓库**  多人协作

> git文件状态

**已修改状态   暂存状态   已提交状态**

git仓库文件修改之后 **已修改状态 **

git add 之后 添加到暂存区  **暂存状态 **

git commit之后 提交到git仓库  **提交状态**



**GUI工具**

```bash
git version

git config --global user.name  'myname'

git config --global user.email '498601485@qq.com'

cat ~/.gitconfig 

# git 命令别名
[alias]
    co = checkout
    br = branch
    ci = comit
    st = status
    unstage = reset HEAD --
# http代理    
[http]    
[https]    

# 创建仓库
mkdir project1
cd project1

# 初始化 git仓库 默认创建master 分支
git init 
# 创建markd 自述文件
touch README.md

# git仓库状态
git status #查看自上次提交之后是否有修改
Untracked fiels #未跟踪的文件列表
git add README.md #添加到暂存区 -> 本地临时列表 

git status 
git rm README.md #删除
git add -A  #当前仓库 文件夹的所有文件加入


# 查看暂存区
ll -la #查看隐藏文件
cd .git
cat index

# 创建分支1
git branch feature1
# 查看分支
git branch
# 切换到分支1
git checkout feature1

#分支1 添加文件file1
touch file1
#添加到暂存区
git add file1
#提交
git commit -m 'add file1'


git branch feature2
git checkout feature2
cat file1

c
```



### 分支管理

```bash
# 创建分支
git branch (branchname)

#切换分支
git checkout (branchname)

#合并分支
git merge (branchename) #将xx合并到当前分支

# svn分支 需要创建多个目录 不同分支在不同目录
# git分支不需要创建多个目录，切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容


# git bash 清屏
clear

# 创建分支1(sub1)
git branch sub1
git checkout sub1
touch sub1.txt
vim sub1.txt
git add sub1.txt
git commint -m 'add sub1.txt'

#在分支1的基础上 创建分支11
git branch sub1-1
git checkout sub1-1
touch sub1-1.txt
vim sub1-1.txt
git add sub1-1.txt
git commit -m 'add sub1-1.txt'

# 将分支11合并到分支1
git checkout sub1 # 返回分支sub1分支
git merge sub1-1  #将sub1-1合并到当前分支 sub1 

#如果主分支 修改了， 当前分支需要依赖于主分支新修改的文件
# git 主分支同步到子分支
#切换到子分支
git merge master
```

