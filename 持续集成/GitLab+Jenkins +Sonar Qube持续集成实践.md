# 持续集成

> 课程大纲

1、持续集成

- 集成

2 、持续部署

4、持续集成的实现思路 git jenkins shell

版本控制系统

​	常用的版本控制系统

- svn
- git

git版本控制

- 基本使用
- git与github关联

gitlab仓库

- 配置域名
- 配置邮箱
- gitlab的基本使用
- gitlab中的 用户组  用户  项目
- gitlab 的基本运维 备份 恢复 升级

jenkins持续集成工具

- jenkins是什么

- jenkins安装? 多种方式方法

- jenkins汉化

- jenkins插件 加速插件 安装插件 导入已有的插件 使用插件 

- jenkins free style 自由风格项目 shell命令

-  jenkins 集成gitlab 两者互通

- 先手动搭建一套集群环境，然后实现代码上线

- 使用jenkins 构建项目 html php java 

- 构建项目 html php java  完善整个上线的脚本 部署和会馆

- jenkins自动化 构建java项目 源码包 jar war

- 手动搭建一套java集群环境 然后实现代码自动上线

- maven 将java的源代码 编译为war包 

- jenkins 构建java项目

- jenkins 部署后有通知机制

  	- 邮件
  	- 钉钉

- 代码检测质量 源代码质量扫描 

  扫描没问题 再maven编译打包 

- -SonarQube 

  - 安装SnoarQube
  - 手动将代码推送至Sonarqube测试
  - Jenkins集成Sonarqube

- jenkins 流水线 pipline

  - pipeline 基本语法
  - pipeline 实现html项目流水线部署
  - pipeline实现java项目的流水线部署

- Jenkins分布式构建

  - 一条命令 或图形界面点一下

- jenkins 权限控制



http://docs.xuliangwei.com/15622376571642.html

http://www.oldboyedu.com



### 持续集成

> 集成

> 持续集成

频繁的 一天多次 将代码集成到主干系统



![](https://img-blog.csdnimg.cn/20181126230558790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1MzY4MTgz,size_16,color_FFFFFF,t_70)

持续集成带来的好处



> 持续集成的目的

让产品可以快速迭代，同事还能保持高质量

它的核心措施是，代码集成到主干之前，先进行自动化测试。只要有一个测试用例失败，就不能集成，当然持续集成并不能完全消除bug，而是让他们非常容易发现和改正。



> 什么情况下需要使用持续集成

如果项目开发的规模比较小，软件集成不是问题

如果项目很多 需要不断的添加功能



### 持续交付

> 什么是持续交付

持续交付指的是在持续集成的环境基础之上，将代码部署到预生产环境。

持续交付->代码开->单元测试->合并代码->功能测试(黑盒测试)->手动->部署到生产环境；

Jenkins 点击部署，

![](https://pic1.zhimg.com/80/db7198e3c39e4656e18efcb4bd1b20b1_720w.jpg)

### 持续部署

持续部署是持续集成的下一步；基于交付集成之上的，无论何时代码

半自动 持续交付，人为参与一下，

测试环境 持续部署

生产环境 持续集成 半自动就可以了

![](https://pic1.zhimg.com/80/f96f19e4d567aad5006d841963a86e41_720w.jpg)

Jenkins ->捕捉gitlab上的代码

Jenkins-> 代码测试

Jenkins -> 构建jar war包

Jenkins -> 黑盒测试/功能测试

Jenkins ->Snog

Jenkins ->shell/onsable 打包部署

nginx /code/rainbow





### 版本控制系统

> 什么是版本控制系统

version control system 

常见的版本管理方式

对文件加以版本号的管理 增加时间、

版本命名

readme-20190208-v1.doc

readme-20190312-v2.doc

> 版本控制系统解决了什么问题

- 追溯文件历史变化

- 多人团队协同开发

- 代码集中统一管理



> 常用的版本控制系统

常见的版本控制系统分为两种：

- svn 集中版本控制系统 svn为代表
- git 分布式版本控制系统的代表

集中式

分布式



### GitLab

> 安装 https://about.gitlab.com/install

git-ce

