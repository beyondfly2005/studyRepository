# git 创建仓库的过程

> 在github上创建了一个名为studyRepository的资料仓库
>
> 主要用户记录学习的过程，防止资料丢失



###  github 创建仓库

登录github

创建新的仓库里

编写仓库描述

增加自述文件readme.md

复制仓库地址

> https://github.com/beyondfly2005/studyRepository.git



#### 创建本地仓库

```bash
# 本地创建仓库
git init

#查看远程分支
git remote -v  

git remote add upstream https://github.com/beyondfly2005/studyRepository.git

# 添加文件
git add *

#提交
git commit -'添加仓库自述文件'

# 推送到远程仓库
git push upstream master
```







 