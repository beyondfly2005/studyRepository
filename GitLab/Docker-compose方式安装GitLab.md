# GitLab

> 创建目录

```bash
mkdir -P /usr/local/docker/gitlab
```

> 创建docker-compose.yml文件

```bash
vim docker-compose.yml
```



> 配置docker-compose.yml
>
> 写入如下内容

```yml
version: '3'
services:
    web:
      image: 'twang2218/gitlab-ce-zh'
      restart: always
      container_name: 'gitlab'
      hostname: '192.168.44.129'
      environment:
        TZ: 'Asia/Shanghai'
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://192.168.44.129:8085'  #外部访问地址  与nginx端口一致
          gitlab_rails['gitlab_shell_ssh_port'] = 220  # ssh访问用的端口
          unicorn['port'] = 8888     #gitlab 内部端口
          nginx['listen_port'] = 8085
      ports:
        - '8085:8085'
        - '8443:443'
        - '220:22'
      volumes:
        - /uer/local/docker/gitlab/config:/etc/gitlab
        - /uer/local/docker/gitlab/data:/var/opt/gitlab
        - /uer/local/docker/gitlab/logs:/var/log/gitlab
```





### GitLab基本配置

> 设置可见性访问

设置 -> 可见性与访问控制： 私有

> 账户和限制设置

头像Gravatar 取消√， 可能需要翻墙 影响速度

> 注册限制

启用注册 取消 √ ，不启用注册



### 创建管理员账户 非ROOT

管理区域 概述 用户  添加用户  gaolonfei  邮箱 

编辑密码 至少8位  设置为12345678

如果忘记密码 可以使用root账户进行修改，所以请牢记root账号密码



### 创建一个项目

项目 您的项目  新建一个项目 ，添加自述文件MD

- 私有  个人的

- 内部   注册用户使用

- 公开  所有用户都可以使用



### 本地安装Git

安装 git  下载地址：https://git-for-windows.github.io/   

​				https://github.com/git-for-windows/git/releases/download/v2.26.2.windows.1/Git-2.26.2-64-bit.exe

安装 TortoiseGit-2.10.0.2-64bit.msi 海龟git  下载地址 https://tortoisegit.org/download/

安装 TortoiseGit-LanguagePack-2.10.0.0-64bit-zh_CN.msi 中文语言包

设置语言 右键 TortoiseGit -> 设置 -> 常规设置 -> 语言 -> 中文简体



### 本地Git克隆项目

复制git项目路径 http://192.168.1.252:8085/gaolongfei/hello-gitlab.git

选择项目 目录，右键 git clone / git 克隆  自动识别复制的路径，确定 ，输入用户名 密码 确定后 即可拉取项目

> 如果输错用户名或者密码  会报 `git 未能顺利结束 (退出码 128) `  `tortoisegit Authentication failed `

任意目录右键 git bash here 输入命令， 即可清除错误的用户名/密码

```bash
git config --system --unset credential.helper
```



### 修改文件 测试提交

修改项目中的文件 git 提交->master

- 提交  提交到本地库

- 提交并推送  提交到本地库 并推送到远程仓库

- 推送  推送到远程仓库

  默认提交到master 主分支

  可以随时创建分支，提交到分支

  

### 生成公钥

> 目的 ：使用ssh方式拉取和推送项目，实现免密登录，为持续集成做准备

git安装目录 C:\Program Files\Git\usr\bin\ssh-keygen.exe

可以在cmd中 或者 git bash中执行

```bash
ssh-keygen.exe -t rsa -C "498601485@qq.com"  #注册在gitlab上的地址
```
默认存放到      `(/c/Users/Administrator/.ssh/id_rsa)`    

如果已经存在 会提示覆盖

`Enter passphrase (empty for no passphrase):                                     `    直接回车；（输入密码短语（无密码短语为空））

生成的文件在 C:\Users\Administrator\.ssh

id_rsa			私钥

id_rsa.pub	公钥			编辑打开 复制文件内容 添加到gitLab

> 请勿重复生成秘钥，公钥 私钥是一对，重复生成服务器记录旧的公钥，用新的私钥无法获取代码，会报权限错误，等同于旧钥匙开新锁



### GitlLab添加用户公钥

用户 settings 用户设置  -> SSH秘钥

增加SSH秘钥  key 中 粘贴上步复制的公钥

自动生成的标题信息可以 随意修改 ；点击 “Add Key” 进行 保存



### GitlLab项目修改为ssh协议 

进入gitlab 项目 -> 您的项目 -> 项目名  

添加了GitLab用户公钥后

项目的协议自动 变成了ssh 项目地址也变成了以ssh协议开头的地址



### TortoiseGit设置ssh

TortoiseGit -> 设置  -> 网络 -> SSH

SSH客户端默认为 TortoiseGit下面的ssh

C:\Program Files\TortoiseGit\bin\TortoiseGitPlink.exe

修改为 git安装目录下ssh

C:\Program Files\Git\usr\bin\ssh.exe

如果确定 不起作用，勾选上方的 启用代理服务器



### 使用ssh方式克隆代码

复制ssh协议的项目地址

目录右键 git克隆 默认填充复制的地址，采用ssh方式进行克隆

确定，此时不再需要输入用户名密码， 直接从服务器

> 注意： 秘钥不要重复生成，否则会报权限认证错误