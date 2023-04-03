# GIt分支管理

```bash
#创建分支
git branch feature1

#查看分支
git branch

#切换分支
git checkout feature1

vim a.txt
git add a.txt
git submit -m 'add a.txt'

# 基于feature1 创建分支11
git branch feature11

# 创建并切换分支
git chekcout -b fature11-1
# 等同于
# git branch feature11-1
# git checkout feature11-1

# 删除分支
git branch -d  featureName

# 强制删除分支
git branch -D featureName

# 合并分支 合并分支3到当前分支
git merge feature3 

# 推动到远程
git push

git checkout feature1 

# 将本地分支推送到远程
git push origin feature1

#在本地删除远端分支
git push origin :feature1

# 将本地featur1 推送到远端 并命名分支为f1
git push orgin feature1:f1


```

#### git分支合并

```bash
#查看日志 倒序
git log

#退出
q

#每次提交一行展示
git log --oneline

git log --oneline -3
git log --oneline -5  # 取前5条
git show logid

#更好的展示日志 Pretty git branch graphs
git log --all --decorate --oneline --graph
git log --all --decorate --oneline --graph

# 更多日志展示
#https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs

# 将以上命令配置自定义命令
vim ~/.gitconfig
[alias]
  dog = log --all --decorate --oneline --graph
  dog = log --all --decorate --oneline --graph

#使用
$ git dog 


```

```bash
# 合并模式
#Fast Forward 合并模式

--no-ff 创建一个新的合并节点 并产生一个新的commit



git merge f1 --no-ff

git rebase master

# rebase 黄金法则：绝对不要在公共分支使用reabase



git rebase 将f1分支移动到master分支的最后一次提交，将master分支的所有提交并入过来

分支冲突

解决冲突

cat a.txt

<<<<<HEAD

当前分支的变化

======

要合并的分支的内容

''>>>>>''

vim vi 编辑器 修改

git mergetool

 vimdiff 比较


#命令查看当前文件夹内的文件
$ ll 

# a.txt.orig 冲突现场

rm  a.txt.orig  删除

合并工具

```

### Git中的回滚和撤销

```bash
# ^ 表示前一次提交 ^^前两次撤销
git reset master^
git reset master^^

# 撤销到前5次提交
git reset master~5

#绝对撤销
git reset 版本id


git reset --help

#git reset 模式 
# --soft 留在工作目录 暂存区 也不会丢掉 只是移动了head指针
# --mixed 暂存区文件全部丢掉，但在工作目录中存在
# --hard  工作目录和暂存区的文件 全部丢掉 简单粗暴
#reset不是重置 前往 变成 goto

git reflow
git reset HEAD
git reset HEAD^

#
git revert 
#撤销某次操作
#此次操作之前和之后的commit和history
#并且把这次撤销作为一次最新的提交

git log

git revert id

#在公共分支上master使用 git revert 保留历史记录 可以回溯
#在特性分支使用git reset 直接回退

git revert HEAD^
git revert HEAD~5
git revert id

#git revert 是用一次新的commit提交来回滚之前的commit
#git rest 是将旧的commit指针移动了一下 并没有删除旧的commit



```

### git ignore

```bash
# 忽略项目文件 .idea .settings .proj .classpath
touch .gitignore
vim .gitignore

#忽略系统自动生成的目录 编译文件 中间 文件 clas文件
#敏感的配置文件
package.sh
*.sh
.setting/
!*.txt   排除
/a/*.class


# www.gitignore.io 网站 生成忽略文件

```



###　同步fork过的仓库

```bash
# fork 过的仓库

git remort -v  #查看远程分支

#upstream 上游仓库 你基于其他人的开源项目 复制fork过来 进行修改 贡献代码
git remote add upstream http://xxx
git fetch upsteam 
git fetch upsteam master
git fetch upsteam dev

git push = fetch + merge
git branch
git branch -r 查看远程的分支

#合并远程分支
rebase mege 
# merge 使用的历史
# 使用rebase

git rebase uptream/master



```

### github 免密推送

```bash
#https协议 ssh 协议
#local
#http https
#ssh
#git协议
git clone

#http协议的问题
#需要输入用户名 密码

#ssh公钥
创建sshkey

#本地生成sshkey
ssh-keygen -t rsa -b 4096 -C "your@mail"


git push #不再使用密码

```

### git 工作流

```bash
# git工作流
# https://github.com/xirong/my-git/blob/master/git-workflow-tutorial.md
```



###　git 常用的图形化工具

```bash
# sourceTree  支持git和svn
www.sourcetree.com

# TortoiseGit

# Intellj Idea


# VS Code

```

