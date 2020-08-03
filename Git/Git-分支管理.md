> 视频  https://www.bilibili.com/video/BV15E411F75x?p=560

### git简介



### git安装

git-scm.com/download

git branch fea



git checkout 分支名



git add

git commit



工作区

暂存区

本地库

```flow
s1=>start: 工作区
s2=>start: 暂存区
s3=>start: 本地库

s1->s2->s3
```



工作区 

git add 

暂存区  

git commit 

本地库

![markdown](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1588955284258&di=a7fe92e5f2f3ad72c1c18e3d3f9b7903&imgtype=0&src=http%3A%2F%2Fwww.uml.org.cn%2Fpzgl%2Fimages%2F2017041302.png "markdown")

![markdown](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1588955284258&di=a7fe92e5f2f3ad72c1c18e3d3f9b7903&imgtype=0&src=http%3A%2F%2Fwww.uml.org.cn%2Fpzgl%2Fimages%2F2017041302.png "markdown")

![](C:\Users\beyond\AppData\Local\Microsoft\Windows\INetCache\IE\2R71MJBF\v2-01368ac7859e73cf8b882d644fdb1589_720w[1].jpg)





# 平台即服务Paas



#### GitLab 代码管理平台



##### 代码管理的必要性

 - 代码整合

   + 代码复制

   + 防止代码丢失

   + 代码回滚
- 代码备份

     

#### 什么是git

开业的分布式版本管理系统，用于敏捷高效的管理人和或大或小的项目

版本控制软件，



与常用的cvs svn不同，采用分布式管理方式，不必服务器端软件支持



#### 如何安装git

Git-2.18-64.bit

TortoiseGit- 海龟git

sourceTree macOS

https://tortoisegit.org/download

git命令行工具

git version



克隆git资源作为工作目录

在克隆的资源上添加或修改文件

如果其他人修改了，可以

提交修改

在修改完成后





获取或新建项目

git 支持本地运行



第三方代码托管平台

GitHub

Gitee 码云 放码过来

码市



git 密码 beyondbike810206



**克隆代码 **

> git clone https://github.com/beyondfly2005/github-test.git

**提交代码**

> git add .gitignore

> git commit  -n  " 修改过滤文件 忽略工程文件"

**推送代码**

> git push

> 第一次推送 提示需要进行认证
>
```bash
## 需要使用如下命令 进行认证
git config --global user.email "498601485@qq.com"
```

**然后在推送代码**

```bash
git push 
```



#### 使用TortoiseGit简化操作



#### GitLab 自建git代码托管平台

https://gitlab.com

##### 什么是gitLab

开源的版本管理系统 实现自托管Git项目仓库，可通过web界面

拥有与gitHub类似的功能 

利用 ruby on Rails



##### 基于Docker安装GitLab

使用相对稳定的10.10.5


```bash
docker  search gitlab-ce-zh 社区版本免费
```

```bash
docker pull 
```

```yaml
version: '3'
services:
  web:
    image: 'twang2218/gitlab-ce-zh'  
    restart: always
    environment:
      TZ: 'Asia/Shanghai'
      GITLAB_OMNIBUS_CONFIG: |
            external_url 'https://域名'
            gitlab_rails['gitlab_shell_ssh_port'] = 2222
            unioncorn['port'] = 8088
            nginx['listen_port'] = 80
      ports:
        - "80:80"
        - "443:443"
        - "2224:22"
      volumes:
        - "/srv/gitlab/config:/etc/gitlab"
        - "/srv/gitlab/logs:/var/log/gitlab"
        - "/srv/gitlab/data:/var/opt/gitlab"

```

> docker compose up

```bash
docker compose up

docker ps


```

**修改管理密码**

root  /12345678



**GitLab 基本设置**

**可见性与访问控制**

**账号和限制设置**

启用Gr头像 可能需要翻墙 如果很卡打不开 可以关掉



**注册限制**

​	不启用注册

**持续集成和部署**



###　GitLab账号管理

##### 新建用户

##### 创建一个项目

> 私有 拉取代码都需要登录
>
> 内部 必须是已经登录用户
>
> 公开 都可以访问



#####　使用SSH的方式拉取和推送项目



为免密登录 为最后的持续集成做准备

生成 SSH KEY



git/usr/bin

ssh-keygen  -t  rsa -C "498601485.com"  --gitlab 注册的地址



> 使用ssh方式

ssh://





