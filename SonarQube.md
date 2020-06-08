# SonarQube

Gitlab 与 SnonarQube集成

Jenkins 与 SonarQube集成

SonarQube 基于Java开发，所有需要OpenJDK

MySQL6.5以上

4G内存以上

sonarQube-7.0.zip

sonarqube-scan-cli-4.0

sonar_plugins.tar.gz



### 环境准备

```bash
systemctl stop firewalld
systemctl disable firewalld
setenforce 0

```



####　安装依赖工具

```bash
yum install git java unzip wget -y
wget https://mirrors.tuna.tsing.edu.cn/mysql/yum
wget https://mirrors.tuna.tsing.edu.cn/mysql/yum
wget https://mirrors.tuna.tsing.edu.cn/mysql/yum
wget https://mirrors.tuna.tsing.edu.cn/mysql/yum
yum localinstall -y mysql-communtiry-*
```



### 配置域名解析

修改hosts 文件 添加

```bash
127.0.0.1 
本机ip/虚拟机ip sonar.anchuang.com

# 准备sql数据库
systemctl start mysqld
mysqladmin password 123456
mysql -uroot -p123456 -e "CREATRE DATEBASE sonar default CHARACTERSET utt8;"
mysql -uroot -p123456 -e "show databases;"


#

```

### 下载sonarqube 并解压只/usr/local

```bash
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.0.zip
unzio sonarqube-7.0.zip -d /usr/local/
useradd sonar
chown -R sonar.sonar /usr/local/sonarqube-7.0/
ln -s/usr/local/sonarqube-7.0/ /usr/local/sonarqube


```

### 修改数据库配置连接

```bash
vim sonarqube/conf/sonar.properties
sonar.jdbc.usrname=root
sonar.jdbc.password=123456
sonar.url=

tail -f /usr/lcoal/


netsta

##端口 9000
http://sonar.anchuang.com:9000/

#其他计算机要与他连接 添加域名解析

# 默认登录
admin / admin

# 生成token令牌
Provide a token
# Jenkins 使用这个令牌 与sonar连接
# 令牌只出现一次 记得保存连接

# maven 项目扫描

sonar.scanr

# 下载客户端工具
scnaer

# 安装汉化插件
中文 汉化包 market 搜索 
安装 
没有安装html css插件 会检查 java js html
css 检查插件
html 检查插件
java
sonar_plugins

#离线文件安装

#重启 移动要使用普通用户重启
su -sonar -c "/usr/local/"







```



### 项目分析

```bash
sonar-scanner命令

在jenkins中安装  在jenkins 中进行代码质量检测

unzip sonar-scanner

cd /usr/local/sonar -scanner-4.0

vim conf/sonner-scanner/sonar.properties

sonar.host.url=http://sonar.192.168.1.101:9000

sonar.login= #之前生成的token令牌 前提是服务器端 开启了验证

sonar.sourceEncoding=UTF-8


#token令牌 前提是服务器端 开启了验证
#在 配置 权限 强制使用 Force User authentication 开启 保存


/usr/local/sonar-scanner/bin/sonar-swcanner
-Dsonar.projectKey=html \
-Dsonar.source=
cd /var/lib/jemkins/workspace

-X 进入调试模式

#域名解析
vim /etc/host
#加入一行
192.168.1.101 sonar.anchaung.com




```


```bash
bugs 
漏洞
坏味道
覆盖率
重复

#java 代码 可以使用 maven进行检测

mvn sonar:sonar \
-Dsonar.host.url=http://
-Dsonar.log=



```

Jenksin 抓取代码 扫描质量 编译 集成 发布

```bash
# 系统管理  -> 系统设置 ->

Jenkins凭据 （Sonar Token）

#系统管理 -> 全局配置 ->
/usr/local/sonar-scanner

sonar.projectname=#{JOB_NAME}
sonar.projectKey=html
sonar.source=  


git tag -a "v 1.5" -m

```





gitlab

jenkins 

sonarqube

maven -> nexus

docker -> reponsity



钉钉通知

Jenkins 集成钉钉概述



1 Jenkins为什么要通知

使用钉钉推送消息的优势

- 实现简单
- 实时提醒
- 便于查看

3、为什么不使用邮件通知

钉钉配置发消息机器人

钉钉 创建群组 机器人

webhook 地址 别人网这个地址发消息



群聊 添加自定义 机器人  头像 Jenkins

生成tokern

Jenkins

Jenkins 钉钉插件





Jenkins 构建后的操作 钉钉通知 ->

