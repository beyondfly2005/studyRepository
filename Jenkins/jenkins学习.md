# Jenkins 

```bash
find / -name "jenkins"

/home/tomcat-cluster/apache-tomcat-7.0.52-803/work/Catalina/localhost/jenkins
/home/tomcat-cluster/apache-tomcat-7.0.52-801/work/Catalina/localhost/jenkins
/home/tomcat-cluster/apache-tomcat-7.0.52-802/work/Catalina/localhost/jenkins
/home/sysac/apache-tomcat-8.5.28-hd/work/Catalina/localhost/jenkins
/home/sysac/apache-tomcat-8.5.28-hd/webapps/jenkins

```

#### 什么是jenkins

​	Jenlins是一个开源持续集成工具，用java编写的

​	jenkins是一个调度平台，调用插件完成工作

#### 为什么要用Jenkins

​	Jenkins可以将各种开源软件集成

​	gitlab maven sonarqube   shell dingding通知

#### Jenkins的安装配置

> 官方安装介绍
>
> https://www.jenkins.io/zh/
>
> dokcer
>
> war包安装
>
> windows安装
>
> jar包安装 
>
> tomcat war包
>
> yum install 安装

> 版本

LTS长期支持版 稳定版



#### Jenkins的插件管理



```bash
# linxu 修改主机名
hostnamectl set-hostname jenkins

#查看网卡ip
cat /etc/sysconfig/network-script/ifcfg-eth0


#Jenksin c插件下载加速
# 改为清华大学源
https://mirror.tuna.tsinghua.edu.cn/
搜索
jenkins/update/update-center.json

# 复制url地址
# https://mirror.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json

# 插件管理 -> 高级 -> 升级站点
#URL 中填写 https://mirror.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```



#### Jenkins部署tomcat

Jenkins 通过 Deploy 插件热部署 java 程序

https://blog.51cto.com/wzlinux/2166241

























































