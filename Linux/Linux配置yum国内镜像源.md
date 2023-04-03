# Linux配置yum国内镜像源

参考文档
https://blog.csdn.net/inslow/article/details/54177191

以修改为阿里云镜像为例

##### 修改CentOS默认yum源为阿里源 

阿里源 mirrors.aliyun.com

##### 1、首先备份系统自带yum源配置文件/etc/yum.repos.d/CentOS-Base.repo

```bash
cd /etc/yum.repos.d/
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

##### 2、下载ailiyun的yum源配置文件到/etc/yum.repos.d/

```bash
CentOS7
[root@localhost ~]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

CentOS6
[root@localhost ~]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
	
CentOS5
[root@localhost ~]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
```

```bash

#当前系统为CentOS7所以 执行
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
#如果下载报错可以手动下载 ，然后上传到服务器
#并修改 Centos-6.repo 改名为CentOS-Base.repo
mv Centos-6.repo CentOS-Base.repo
```

##### 3、运行yum makecache生成缓存

```bash
yum clean all
yum 
yum makecache
```

生成缓存时报错
[Errno 14] PYCURL ERROR 6 - "Couldn't resolve host 'mirrors.aliyun.com'"  或无法解析'mirrors.aliyun.com'
参考文档：https://blog.csdn.net/hjfcgt123/article/details/86499553
因是DNS服务器错误，不能解析mirrors.aliyun.com这个域名

##### 4、最终解决方案

```bash

#编辑/etc/resolv.conf文件，添加如下代码：
nameserver 223.5.5.5
nameserver 223.6.6.6

# 或者
echo 'nameserver 223.5.5.5'>>/etc/resolv.conf 
echo 'nameserver 223.6.6.6'>>/etc/resolv.conf 
# 查看配置
cat /etc/resolv.conf

# 执行命令 重新启动网络配置文件。
service network restart

#重启网络后 
# 查看配置
cat /etc/resolv.conf

#配置信息丢失
#在/etc/sysconfig/network-scripts/ifcfg-eth0 文件中添加上面的DNS server信息，注意有=这个符号
#注意网卡名称 可能不是这个名称 可能是 ifcfg-eno1
# 可以 cd /etc/sysconfig/network-scripts/ 进入目录 ll查看下 目录信息 一般第一个是网卡 ifcfg-et ifcfg-en开头 数字结尾
cd /etc/sysconfig/network-scripts/
vim ifcfg-eth0
DNS1=223.5.5.5
DNS2=223.6.6.6

```
