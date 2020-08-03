# 环境搭建

Linux的安装，安装步骤比较繁琐，可以本地搭建一台学习使用

> 安装CentOS 

Linux是一个操作系统，可以将自己的电脑安装为双系统

也可以安装虚拟机

Vmvare 360一键安装



> 在虚拟机上安装CentOS

1、通过奖项安装，阿里云镜像地址 http://mirrors.aliyun.com/centos/7/isos/x86_64/下载完成安装

2、使用百度云链接下载

```
链接 https://pan.baidu.com/s/1e0YEzN3BW0DDXFtMscvvVA
提取码：716m
```

3、安装VMware虚拟机软件，然后打开我们的镜像即可使用



> CentOS7设置yum源

```bash
设置源
--查看配置文件
ll /etc/yum.repos.d
```

CentOS-Base.repo：在线的yum源配置文件
CentOS-Media.repo：本地yum源配置文件



```bash
--备份CentOS-Base.repo
--进入目录
cd /etc/yum.repos.d

--备份CentOS-Base.repo
cp CentOS-Base.repo CentOS-Base.repo.bak

--查看CentOS-Base.repo
cat CentOS-Base.repo


```



> 解决使用yum时，报错 : Cannot find a valid baseurl for repo: base/7/x86_6
>
> 参考文档：https://www.cnblogs.com/linnuo/p/6257204.html

```bash 
vi /etc/sysconfig/network-scripts/ifcfg-ens33 
修改 ONBOOT=no，改为ONBOOT=yes
（每个机子都可能不一样，但格式会是“ifcfg-eth数字” 或者 “ifcfg-ens数字”）
--保存退出
:wq 

--重启网络
service netwok restart

--测试网络
ping www.baidu.com
(如果有返回包 说明网络正常)

--查看网络
ip addr
(这时候 虚拟机有了自己的ip地址 inet 192.168.0.104，可以使用ssh链接虚拟机了)

--安装wget
yum -y install wget

--安装vim
yum -y install vim

--安装 ifconfig命令
sudo yum install net-tools
```



> CentOS7修改为国内yum源
>
> 参考文档 https://www.cnblogs.com/cerana/p/11179728.html

```
--进入目录
cd /etc/yum.repos.d/

--查看文件
ll

--备份源yum文件
cp CentOS-Base.repo CentOS-Base.repo.bak

--下载网页源或阿里源
wget http://mirrors.aliyun.com/repo/Centos-7.repo
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

--清除系统yum缓存并生成新的yum缓存
yum clean all # 清除系统所有的yum缓存
yum makecache # 生成yum缓存

--安装epel源
yum list | grep epel-release
yum install -y epel-release

--再次清除系统yum缓存，并重新生成新的yum缓存
yum clean all
yum makecache

--查看系统可用的yum源和所有的yum源
yum repolist enabled
yum repolist all

--测试安装
yum install openssh-server
```



## 常用的Linux命令

>  开机登录

系统开机会启动很多程序 在windows叫做服务，在linux叫做守护进程

```
--关机命令
shutdown

--查看shutdown的帮助文档
man shutdown

sync --同步数据到内存中
shutdown--然后在关机

linux没有输出错误，表明正确

shutdown -h 10 	#10分钟后关机
shutdown -h now #立即关机
shutdown -h 20:30 #指定时间关机
shutdown -h +10 #10分钟后关机
shutdown -r now #立即重启
shutdown -r +10 #10分钟后重启

--重启
reboot

halt #关闭系统 等同于 shutdown -h now 和poweroff

***
重启或关机前 ，建议运行sync命令将内存中的数据写入到磁盘 以免数据丢失
```



> 系统目录结构

1、一切皆是文件

2、根目录/  所有的文件都挂在这个节点下

```bash
ls  --查看文件列表
ls -al 查看所有文件 包含隐藏文件
ll  --查看文件列表
```

系统目录的解释

```bash
/bin
/boot
/dev
/etc  存放配置文件redis配置 tomcat配置
/home 用户主目录 类似users/administrator用户主目录
/lib   库文件 动态链接库
/lost+found 存放突然关机的一些文件
/media 设备U盘 光驱等
/mnt 用户临时挂载的设备
/opt 额外安装的软件存放位置
/proc 虚拟的目录 存放内存的映射
/root 管理员账号根目录
/sbin  S=SuperUser系统管理员使用的程序的存放目录
/srv
/sys
/tmp 临时文件
/usr 用户文件
/usr/bin
/usr/sbin
/usr/src
/var
/run
/www 存放服务器网站相关的资源文件
```



## 常用的基本命令

#### 目录管理

```bash
--绝对路径

--相对路径

cd 切回目录
./ 当前目录
cd .. 返回上一页目录

ls   	# 列出目录
ls -a	#all 全部文件
ls -l	#包含属性和权限
ls -al	#列出包含隐藏文件的 文件信息

ll		#=ls -l 

cd 命令 切回目录
cd 目录名(绝对路径 相对路径)

pwd  	# 用户当前所在的目录

mkdir 	#创建目录
mkdir -p test1/test2/test3 递归创建多级目录

--删除目录
rmdir test1  仅能删除空目录
rmdir -p test2/test3/test4 删除层级目录

--复制文件或目录
cp 源文件 目标文件
cp test.txt files/ 拷贝文件到目录

如果存在同名文件 会提示覆盖

--移除文件或目录
rm 
rm -f 忽略不存在的文件
rm -r 递归删除
rm -i 询问是否删除
#rm -rf / 危险 系统中的所有文件就别删除了 删库跑路
rm -rf 

--移动文件
mv 源文件 目标文件
mv -f 强制
mv -u 只替换已经更新过的文件

mv 重命名文件
mv test.txt test_bak.txt
```



## 安装Docker

官网安装参考手册 

> https://docs.docker.com/engine/install/centos/

> Docker安装

1、操作系统要求； 检查centos版本

```bash
cat /etc/redhat-release
```

2、安装准备环境

```bash
yum -y install   #-y 所有提示都为y
yum -y install gcc
yum -y install gcc-c++


```

3、清除以前的docker版本

```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

4、安装依赖的包

```bash
# 安装yum-utils软件包
yum install -y yum-utils 

#设置稳定的存储库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

5、安装DOCKER引擎

```bash
sudo yum install docker-ce docker-ce-cli containerd.io
```

6、启动Docker

```bash
systemctl start docker
```

7、搜索并拉取hello-world

```bash
docker search hello-world
docker pull hello-world 
```

8、启动运行容器

```bash
docker run hello-world 
```



> 处理报错 
>
> yum安装时提示：Another app is currently holding the yum lock; waiting for it to exit...

```bash
#执行命令
rm -f /var/run/yum.pid
```



```java
String userName = "张三";

```

```javascript
var usrName = "张三";
```

```pyth
userName = "张三";
```

```sql
select * from t_sys_user where loginCode='xtgt';
```

