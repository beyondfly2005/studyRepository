# Git

#### 什么是Git

#### Git和GitHub

#### Git的安装和配置

```bash

git config --global user.name "zhangsan"

git config --global user.email "xx@qq.com"

git status #状态

# 三个区：工作区 --git add -> 暂存区 --git commit--> 本地仓库

git init  #初始化项目 .git文件夹

git add
git add -A

git commit -m "修改记录信息"

git　branch  #查看所在的分支

#创建分支
git checkout -b 'feature1'

#推送代码到远程仓库的某个分支
git push  
git push --set-upstream origin feature1

# 拉取
git pull
git branch
git branch -a 

git checkout -b 'fea02'
git branch 
git branch -a
git pull 

# 合并分支
git checkout fea02  #跳转分支
git merge fea01
```

#### GitHub/GitLib的使用











