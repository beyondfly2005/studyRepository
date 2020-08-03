> rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

### 0-检验Docker是否安装
```bash
docker -version
```
### 1-卸载旧版本
```bash
$ sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine
```
或者
```bash
$ sudo yum remove docker \
    docker-common \
    docker-selinux \
    docker-engine
```



安装 Docker Engine-Community
使用 Docker 仓库进行安装
在新主机上首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。



### 2-安装所需的软件包
yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。
```bash
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
设置稳定的仓库
```bash
## 设置稳定的yum源,使用以下命令来
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

--安装 Docker Engine-Community
```bash
## 安装最新版本的 Docker Engine-Community 和 containerd，或者安装特定版本：
$ sudo yum install docker-ce docker-ce-cli containerd.io
$ sudo yum install -y docker-ce
$ sudo yum install -y docker-ce-17.09.0.ce
```

### 3-配置Docker加速

软件源设置为国内的源

```bash
# 备份本地yum源
mv /etc/yum.repos.d/CentOS-Base.repo   /etc/yum.repos.d/CentOS-Base.repo.bak
mv /etc/yum.repos.d/CentOS-Base.repo.backup  /etc/yum.repos.d/CentOS-Base.repo

# 获取阿里yum源配置文件
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

# 更新cache
yum makecache

# 更新
yum -y update
```


### 4-启动docker
```bash
$ systemctl start docker.service
```
### 5-开机启动

```bash
systemctl enable docker
```

### 6- 测试Docker

```bash
docker version
```

