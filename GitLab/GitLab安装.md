# GitLab安装


### GitLab安装

> 未采用docker方式安装 的安装方法

```bash
gitlab_rails['stmt_port'] = 465
gitlab_rails['stmt_username'] = "123@qq.com" #发件人邮箱账户
gitlab_rails['stmt_sassword'] = "" #发件人邮箱授权码
gitlab_rails['stmt_domain'] = "qq.com" 
gitlab_rails['stmt_authentication'] = "login"
gitlab_rails['stmt_enable_starttls_auto'] =  true
gitlab_rails['stmt_tls'] =  true


# 启动和定制
gitlab-ctl reconfigure
gitlab-ctl start 
gitlab-ctl restart 
gitlab-ctl status
gitlab-ctl stop
#每次修改/etc/gitlab/gitlab.rb都需要reconfigure

```


```bash
# GitLab收不到邮件的问题

# WARNING: COMMAND_FAILED: '/usr/sbin/iptables -w10 -D FORWARD -i docker0 -o docker0 -j DROP' failed: iptables: Bad rule (does a matching...that chain?)

https://www.jianshu.com/p/f58b8b65641a
https://www.cnblogs.com/sunhongleibibi/p/12074315.html
https://soulchild.cn/811.html
https://www.cnblogs.com/weifeng1463/p/8489563.html
https://www.jianshu.com/p/b32cb8eb48aa
```


### 用户、群组、项目

1、创建组

2 创建项目 项目隶属于某个组

3 创建用户 为用户分配组



私有 ：群组及其

内部: 任何加入

公开：任何人


### GitLab 配置邮箱


> 参考文档
>
> https://www.jianshu.com/p/f58b8b65641a

```bash

# 进入容器（宿主机内执行）
docker exec -it gitlab bash #

#编辑配置文件（容器内执行）
$ vim /etc/gitlab/gitlab.rb #编辑gitlab配置文件
```

#基础配置
```
external_url 'http://gitlab.yourdomain.com/' #修改为gitlab访问域名
gitlab_rails['time_zone'] = 'PRC' #将标准时修改为中国时间
gitlab_rails['gitlab_shell_ssh_port'] = 22 #修改ssh端口

gitlab_rails['gitlab_default_can_create_group'] = false #限制普通用户创建组
gitlab_rails['gitlab_username_changing_enabled'] = false #username不能修改
```

#邮箱配置:
以163个人邮箱为例：
```
gitlab_rails['gitlab_email_enabled'] = true
gitlab_rails['gitlab_email_display_name'] = 'gitlab'
gitlab_rails['gitlab_email_reply_to'] = 'noreply@example.com'
gitlab_rails['gitlab_email_subject_suffix'] = '[GITLAB]'

gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.163.com"
gitlab_rails['smtp_port'] = 465
gitlab_rails['smtp_user_name'] = "xxx@163.com"
gitlab_rails['smtp_password'] = "xxx"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = true
gitlab_rails['gitlab_email_from'] = 'xxx@163.com'
gitlab_rails['smtp_domain'] = "163.com"
user["git_user_email"] = "xxx@163.com"
```
使修改的配置文件生效
$ gitlab-ctl reconfigure #使修改的配置文件生效（容器内执行）
$ gitlab-ctl stop #停止gitlab服务（容器内执行）
$ gitlab-ctl start #启动gitlab服务（容器内执行
$ gitlab-ctl restart #重启gitlab服务（容器内执行）


```
