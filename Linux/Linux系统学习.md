# Linux系统学习

#### Linux系统时间更新

- CentOS为例

```bash
#Linux的时间分为System Clock（系统时间）和Real Time Clock （硬件时间，简称RTC）。

# 系统时间：指当前Linux Kernel中的时间。
# 硬件时间：主板上有电池供电的时间。

#查看系统时间的命令：
$ date

#设置系统时间的命令：
$ date –set（月/日/年 时：分：秒）
 
#例：
$ date –set “10/11/10 10:15”
 
#查看硬件时间的命令： 
$ hwclock
 
#设置硬件时间的命令： 
$ hwclock –set –date = （月/日/年 时：分：秒）

```

##### 服务器时间与网络时间同步

```bash
# 1.  安装ntpdate工具 
$ yum -y install ntp ntpdate
 
#2.  设置系统时间与网络时间同步
$ ntpdate cn.pool.ntp.org
 
#3.  将系统时间写入硬件时间
$ hwclock --systohc
 
#4.强制系统时间写入CMOS中防止重启失效
hwclock -w
# 或
clock -w
```



##### 修改设置时区

```bash
$ tzselect
# select a continent or ocean
# 选择5 Asia
# select a count
# 选择9 China
# selecct time zone
# 选择1 Beijing Time
# Therefore TZ='Asia/Shanghai' will be used.
#Local time is now:	Tue May 26 16:12:34 CST 2020.
#Universal Time is now:	Tue May 26 08:12:34 UIs #the above informatin OK?
# 选择1 yes


# 也可以直接用下面命令直接更换时区
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```



#### Linux目录结构

-- linux 连接工具
	putty
	x-shell
	finalShell

--linux目录结构
		\bin 	二进制可执行文件ls cat mkdir 等
		\boot	系统引导时使用的文件
		\dev 	设备文件
		\etc	系统配置文件
		\home	存放所有用户文件的根目录	c:/users
		lib		系统 
		\opt	额外安装的可选应用程序
		\proc	虚拟文件系统 存放当前文件映射
		\root	超级管理



--查看linux发行版信息
uname -a

Arch Linux官方已经结束了对OpenOffice.org的支持，并选择了LibreOffice



#### Linux查看文件夹剩余空间

```bash
df -hl

df -hl /root
df -hl /root/.jenkins/workspace/IntellSecurity/

df -hl
df -hl 查看磁盘剩余空间
df -h 查看每个根路径的分区大小
du -sh [目录名] 返回该目录的大小
du -sm [文件夹] 返回该文件夹总M数
du -h [目录名] 查看指定文件夹下的所有文件大小（包含子文件夹）

#更新详细命令文档：
df --help
du --help
```

#### Linux 配置云加速

使用yum命令安装mysql时，发现下载速度很慢，于是决定换成阿里的yum源

解决方法
参考自:https://www.jianshu.com/p/b7cd2f9fb8b7
首先备份一下原先的yum源，避免出错无法恢复

cd /etc/yum.repos.d/
mv CentOS-Base.repo CentOS-Base.repo.bak
然后修改base.reop源

wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
安装epel.repo源

wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
刷新缓存

yum clean all
yum makecache



#### Linux 常用工具安装

##### vim命令安装

```bash
yum -y install vim
```



#### Linux分区调整

> 磁盘分区扩容

```bash
# 查看现有分区
df -h

ls /dev

然后挂载

df -T 只可以查看已经挂载的分区和文件系统类型。

fdisk -l 可以显示出所有挂载和未挂载的分区，但不显示文件系统类型。

parted -l 可以查看未挂载的文件系统类型，以及哪些分区尚未格式化。

lsblk -f 也可以查看未挂载的文件系统类型。

### 先停docker
systemctl stop docker

### 卸载挂载点
umount /var

## 提示umount: /var: target is busy.

## 终止占用的进程
fuser -km /var


# 重启docker
systemctl daemon-reload
 
systemctl restart docker
 
systemctl enable docker

```



#### Linux防火墙firewalld操作




**查看firewall服务状态**

```bash
systemctl status firewalld
```


**查看firewall的状态**

```bash
firewall-cmd --state
```

**查看防火墙规则**

```
firewall-cmd --list-all 
```

##### 开启
service firewalld start
##### 重启
service firewalld restart
##### 关闭
service firewalld stop


```bash
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp

#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload

# 参数解释
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```




#### Linux常见问题处理



##### 问题1
```pr
yum安装时出现 could not retrieve mirrorlist
```
```bash
 # 1. 修改网卡配置
 $ sudo vim /etc/sysconfig/network-scripts/ifcfg-ens33 

 # 2. 将ONBOOT改为yes，wq!保存退出
 ONBOOT=yes

 # 3. 重新启动网络
 $ service network restart

```

##### 问题2

```pro
ifconfig command not found
```
```bash
yum install net-tools
```



##### 问题3

```pro
vim command not found
```
```
yum install yum
```

 



#### Linux-Oracle性能优化

```bash
#使用top命令查看最占cpu资源的进程
top  
```

 PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                 1429 root      10 -10  126064  12496   9636 S   1.0  0.1  65:15.34 AliYunDun                                  1 root      20   0   51600   3772   2572 S   0.3  0.0   0:44.32 systemd                                
 1637 oracle    -2   0 1792328  12488  10736 S   0.3  0.1   4:59.84 oracle                                  1649 oracle    20   0 1796936  37140  32240 S   0.3  0.2   6:32.24 oracle                                     2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd                               
    3 root      20   0       0      0      0 S   0.0  0.0   0:00.60 ksoftirqd/0                            


记录pid=1637
                        
