# Docker 安装Docker Compose

### 安装docker-compose


CentOS 7 安装 docker-compose
> 参考文档：https://blog.csdn.net/A615883576/article/details/89490826
##### 1. compose 简介
	Compose 项目是 Docker 官方的开源项目，负责实现对 Docker 容器集群的快速
	编排。从功能上看，跟 OpenStack 中的 Heat 十分类似。
	
	其代码目前在 https://github.com/docker/compose 上开源。
	
	Compose 定位是 「定义和运行多个 Docker 容器的应用（Defining and running
	multi-container Docker applications）」，其前身是开源项目 Fig。

##### 2. centos 7 下使用 python-pip 安装 docker-compose
	首先检查 Linux 有没有安装 python-pip 包：yum install python-pip。
	没有 python-pip 包就执行：yum install epel-release -y 命令。
	执行成功之后，再次执行：yum install python -y。
	对安装好的 pip 进行升级：pip install --upgrade pip。
	升级完 pip 工具之后，使用：pip install docker-compose 安装 docker-compose。

##### 3. 国内的 epel 和 pip 源镜像

###### 1)  更换 epel 源
	yum install epel-release -y
	wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

###### 2)  更换 pip 源
	Linux 下，修改 ~/.pip/pip.conf (没有就创建一个)， 修改 index-url 至 tuna 源，内容如下：
	
	[global]
	index-url = https://pypi.tuna.tsinghua.edu.cn/simple




``` bash
# 历史版本1.23.2
curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 最新版本1.25.5
curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

如果下载很慢 可以去github下载，然后再上传服务器

https://github.com/docker/compose/releases

> 给docker-compose执行权限

```bash
chmod +x /usr/local/bin/docker-compose
```

> 测试安装是否成功

```bash
docker-compose --version
```



> 参考文档

```
docker-compose
https://www.jianshu.com/p/ee2383b6eedf

安装docker和docker-compose
https://www.cnblogs.com/ruanqin/p/11082506.html

CentOS 7 安装 docker-compose
https://blog.csdn.net/A615883576/article/details/89490826
```

