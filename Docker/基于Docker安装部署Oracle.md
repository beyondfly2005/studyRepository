## 基于docker安装部署Oracle

#### 安装docker
```bash
## --archlinux/manjaro
sudo pacman -S docker

## --ubuntu/debian
apt install docker

## --centos/red hat/fedora
yum install docker
```

#### 启动docker
```bash
## -- centOS
sudo systemctl start docker

## --arch linux
sudo systemctl start docker

## --ubantu
sudo service docker start
/etc/init.d/docker
```

#### 启动docker

```bash
sudo systemctl start docker
```
#### 开机启动 docker

```bash
sudo systemctl enable docker
```

#### 禁止开机启动docker

```bash
sudo systemclt disable docker
```

#### 切换国内镜像源

```bash
vim /etc/docker/daemon.json
写入
{
  "registry-mirrors": ["http://hub.mirror.c.163.com"]
}
或者
{
  "registry-mirrors": ["https://registry.docker.cn.com"]
}

# 重新启动服务生效
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

#### 搜索可用的oracle 镜像

```
docker search oracle

--选用alexeiled/docker-oracle-xe-11g  已经没有了
--选用jaspeen/oracle-11g
```
#### 拉取docker镜像

```
docker pull jaspeen/oracle-11g
```

#### 查看镜像

```
docker image ls
#或
docker images
```





#### 启动报错处理

```bash
$ sudo systemctl start docker

#提示
A dependency job for docker.service failed. See 'journalctl -xe' for details.

# 执行
$ journalctl -xe

# 提示
8月 03 19:42:49 acpc systemd[1010]: Failed to chown socket at step GROUP: No such process
8月 03 19:42:49 acpc systemd[1]: docker.socket: Control process exited, code=exited status=216
8月 03 19:42:49 acpc systemd[1]: Failed to listen on Docker Socket for the API.

# 执行
groupadd docker

# 再次启动
$ sudo systemctl start docker
# 提示
Job for docker.service failed because the control process exited with error code.
See "systemctl status docker.service" and "journalctl -xe" for details.

# 执行
systemctl status docker.service

# 提示
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: failed (Result: start-limit-hit) since 一 2020-08-03 19:50:14 CST; 32s ago
     Docs: https://docs.docker.com
  Process: 1216 ExecStart=/usr/bin/dockerd -H fd:// (code=exited, status=1/FAILURE)
 Main PID: 1216 (code=exited, status=1/FAILURE)

8月 03 19:50:14 acpc systemd[1]: Failed to start Docker Application Container Engine.
8月 03 19:50:14 acpc systemd[1]: docker.service: Unit entered failed state.
8月 03 19:50:14 acpc systemd[1]: docker.service: Failed with result 'exit-code'.
8月 03 19:50:14 acpc systemd[1]: docker.service: Service hold-off time over, scheduling restart.
8月 03 19:50:14 acpc systemd[1]: Stopped Docker Application Container Engine.
8月 03 19:50:15 acpc systemd[1]: docker.service: Start request repeated too quickly.
8月 03 19:50:15 acpc systemd[1]: Failed to start Docker Application Container Engine.
8月 03 19:50:15 acpc systemd[1]: docker.service: Unit entered failed state.
8月 03 19:50:15 acpc systemd[1]: docker.service: Failed with result 'start-limit-hit'.

touch /etc/docker/daemon.json
vim daemon.json

输入
{
	"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}

wq保存退出

systemctl daemon-reload
systemctl restart docker
systemctl start docker.service
systemctl restart docker.service

```

#### 创建oracle 安装目录以及数据目录

```bash
# 用于存放安装文件
mkdir -p /oracle/install
# 用于存放
mkdir -p /oracle/dpdump
```

#### 下载 oracle 

存放于服务器中 下载地址 https://www.oracle.com/database/technologies/oracle-database-software-downloads.html

也可以使用wget下载；如果下载慢 可以使用迅雷下载后，上传服务器

```
wget -c -b https://download.oracle.com/otn/linux/oracle11g/R2/linux.x64_11gR2_database_1of2.zip?AuthParam=1561175324_b6f19c00413750b39fbe89d3a7dce441
wget -c -b https://download.oracle.com/otn/linux/oracle11g/R2/linux.x64_11gR2_database_2of2.zip?AuthParam=1561175324_b6f19c00413750b39fbe89d3a7dce441
```

#### 解压Oracle安装文件

```bash
unzip linux.x64_11gR2_database_1of2.zip -d /oracle/install
unzip linux.x64_11gR2_database_2of2.zip -d /oracle/install
```

##### 安装upzip

```bash
yum install -y unzip zip
```



#### 解压后删除Oracle安装包

```
rm -rf linux.x64_11gR2_database_1of2.zip
rm -rf linux.x64_11gR2_database_2of2.zip
```

#### 运行Docker-Oracle

```bash
docker run -d --privileged -p 1521:1521 -v /oracle/install:/install -v /oracle/dpdump:/opt/oracle/dpdump --name=oracle11g jaspeen/oracle-11g
```
或者
```shell
docker run -d --privileged -p 1521:1521 -v /oracle/install:/install jaspeen/oracle-11g --name oracle11g 
```



#### 查看Oracle 是否运行

```shell
docker ps
```

#### 如未启动  查看日志

```bash
docker logs oracle11g
```



#### 进入oracle11g容器

```shell
docker exec -it oracle11g /bin/bash
```



#### 切换到image的oracle用户

```shell
su - oracle
```



#### 连接oracle数据库

```csharp
sqlplus / as sysdba
```



#### 解锁scott用户

```sql
SQL> alter user scott account unlock;
User altered.
SQL> commit;
Commit complete.
SQL> conn scott/tiger
ERROR:
ORA-28001: the password has expired
Changing password for scott
New password:
Retype new password:
Password changed
Connected.
SQL> 
```



#### 数据库管理工具连接oracle数据库

如Toad for Oracle、Navcat for Oracle、PL/SQL、**datagrip**

![](https://upload-images.jianshu.io/upload_images/18157369-cd77621affb5666e.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/853/format/webp)



> 参考文档

https://www.cnblogs.com/murry/p/11905355.html

https://www.jianshu.com/p/4ede7dcc1d86

https://www.docker.org.cn/book/install/arch-install-docker-36.html

https://www.cnblogs.com/littlesuns/p/11227992.html
