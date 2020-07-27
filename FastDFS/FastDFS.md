> 视频地址：https://www.bilibili.com/video/BV1bE411o7Uz

FastDSF的特点

- 高
- 个

### 5、搭建图片服务器

### 5.1 安装依赖的环境

FastDFS是C语言开发的，安装FastDFS需要从官网下载源码，然后进行编译，但编译gcc环境，如果没有gcc环境，需要安装gcc

```bash

# 安装gcc环境
yum -y install gcc-c++

# 安装依赖的libevent
yum -y insall libevent

# 上传安装包
fastdfs-nginx-module_v16.tar.gz
FastDFS_v5.05.tar.gz
libfastcommonV1.0.7.tar.gz
nginx-1.8.1.gz

#安装libfastcommon（官方提供）


# 安装FastDFS
#解压
tar -zxvf FastDFS_v5.05.tar.gz

cd FastDFS
./make.sh
./make.sh install

#安装成功将安装目录下的conf下的文件拷贝到/etc/fdfs/下(nginx需要)
cd /usr/local/fastdfs/FastDFS
cp conf/*  /etc/fdfs

# 安装Tracker服务 跟踪器服务
vim /etc/fdfs/tracker.conf

base_path=/usr/local/fastdfs/FastDFS/tracker

# 启动跟踪器
/user/bin/fdsf/trackrd /etc/fdfs/tracker.conf
# 重启跟踪器
/user/bin/fdsf/trackrd /etc/fdfs/tracker.conf restart

#开机启动
# 将以上命令加入到自启动的配置文件总
vim /etc/rc.d/rc.local
# 注意配置完毕之后 要将ip设置为静态ip


```


