# GitLab



### GitLab 安装

> 从dockerhub查找可用的Gitlab镜像

```bash
docker search gitlab		#
docker search gitlab-ce		#社区版
docker search gitlab-ce-zh  #社区版中文版
```

> docker version  #查看docker 安装是否正常
>
> 提示：Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

```bash
#处理方法：
systemctl daemon-reload
systemctl restart docker.service
```

> 启动docker并 加入自动启动

```bash
#启动docker
service docker start
#docker加入开机启动
systemctl enable docker
```

> 拉取镜像

```bash
docker pull twang2218/gitlab-ce-zh
docker pull twang2218/gitlab-ce-zh
```

> 查看下载的镜像

```bash
docker images
```



> 创建存放目录
```bash
cd /usr/local/docker/gitlab/
vim docker-compose.yml

# 在docker-compose.yml文件中填入如下信息
```
```yaml
version: '3'
services:
    gitlab:
      image: 'twang2218/gitlab-ce-zh'  #可以加入版本号如 :9.4 默认使用最新版本
      restart: always                  #unless-stopped
      hostname: '192.168.0.109'        #也可以是域名
      environment:
        TZ: 'Asia/Shanghai'
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://192.168.0.109:8085'
          # gitlab_rails['time_zone'] = 'Asia/Shanghai'
          gitlab_rails['gitlab_shell_ssh_port'] = 2222
          unicorn['port'] = 8888
          nginx['listen_port'] = 8085
          # 需要配置到 gitlab.rb 中的配置可以在这里配置，每个配置一行，注意缩进。
          # 比如下面的电子邮件的配置：
          # gitlab_rails['smtp_enable'] = true
          # gitlab_rails['smtp_address'] = "498601485@22.com"
          # gitlab_rails['smtp_port'] = 465
          # gitlab_rails['smtp_user_name'] = "498601485@22.com"
          # gitlab_rails['smtp_password'] = "password"
          # gitlab_rails['smtp_authentication'] = "login"
          # gitlab_rails['smtp_enable_starttls_auto'] = true
          # gitlab_rails['smtp_tls'] = true
          # gitlab_rails['gitlab_email_from'] = '498601485@22.com'
      ports:
        - '8085:8085'
        - '8443:443'
        - '2222:22'
      volumes:
        - /usr/local/docker/compose/gitlab/config:/etc/gitlab
        - /usr/local/docker/compose/gitlab/data:/var/opt/gitlab
        - /usr/local/docker/compose/gitlab/logs:/var/log/gitlab
```

> 使用docker compose 启动 gitlab容器

```bash
docker-compose up  # 要在有yml文件的目录中 启动
```

> 等待很久之后打开浏览器

```
http://192.168.0.109:8085

# 密码设置为12345678

# 使用root/12345678登录

```

> 基本设置

管理区域-> 设置

可见性与访问控制 ：私有

账号和限制设置： 头像 √取消  头像系统Gravater需要翻墙 

注册限制 ： 注册√ 取消

访问http://192.168.0.109:8085/usrs/sign_in 将不允许注册



> 创建用户 为当前用户

区域管理 用户 新建用户



> 创建项目

> 提交



> 设置全局用户名

git config --global user.name



> 提交并推送





# 使用SSH方式拉取和推送项目



### 免密登录gitlab

免密登录是为持续集成做准备



### 生成SSH KEY

打开git安装目录 ssh-keygen.exe

```bash
ssh-keygen -t rsa -C "498601485@qq.com"
```

打开Git Bash Here 输入以上命令

提示目录 生成公钥的默认路径     ` (/c/Users/beyond/.ssh/id_rsa):              `     回车 进行确认

存在会提示替换

提示  `Enter passphrase (empty for no passphrase):                                     `    直接 回车

进入/c/Users/beyond/.ssh/ 目录 ，

> id_rsa  文件是公钥
>
> id_rsa.pub 是公钥

用记事本打开id_rsa文件



### GitLab账号中新建SSH公钥

用户-> Setting -- SSH秘钥 

添加 复制的 公钥，在标题中默认显示 用户的mail，可以修改标题 并保持公钥



将项目 ->您的项目 协议改为改为SSH ，并复制SSH协议下的git路径 



### TortoieseGit 设置 SSH客户端

右键 TortoieseGit -> 设置 -> 网络 -> SSH -> SSH客户端

默认为C:\Program Files\TortoiseGit\bin\TortoiseGitPlink.exe   默认为TortoiseGit安装目录下的

修改为C:\Program Files\Git\usr\bin\ssh.exe  改为git安装目录下的 ssh

> 如果确定不起作用，可以 勾选上方的“使用代理服务器”



### 使用SSH方式克隆代码

右键 Git克隆 默认用复制的地址（SSH协议的Git项目地址）进行项目克隆

提示 是否使用SSH进行连接 选择 “是”，克隆代码



### 测试使用SSH方式提交代码

修改 自述文件 或者项目内的其他文件

提交并推送

回到gitlab 刷新页面 ，查看文件修改是否成功