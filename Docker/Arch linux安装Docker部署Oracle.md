## 基于Arch linux系统

#### 安装docker
```bash
## --archlinux/manjaro
sudo pacman -S docker

## --ubuntu/debian
apt install docker

## --centos/red hat/fedora
yum install docker

## 启动docker
## --arch linux
sudo systemctl start docker

## --ubantu
sudo service docker start
/etc/init.d/docker
```

#### 切换国内镜像源

```
vim /etc/docker/daemon.json
{
  "registry-mirrors": ["http://hub.mirror.c.163.com"]
}
或者
{
  "registry-mirrors": ["https://registry.docker.cn.com"]
}
```

#### 搜索可用的oracle 镜像

```
docker search oracle

--选用alexeiled/docker-oracle-xe-11g  已经没有了
--选用jaspeen/oracle-11g
```
#### 拉取docker镜像

```
--docker pull alexeiled/docker-oracle-xe-11g
docker pull jaspeen/oracle-11g
```

#### 查看镜像

```
docker image ls
```

#### 启动oracle

```bash
sudo systemctl start docker
```

#### 开机启动

```bash
sudo systemctl enable docker
```

#### 关掉开机启动docker

```bash
sudo systemclt disable docker
```



#### 启动报错

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



> 参考文档

https://www.docker.org.cn/book/install/arch-install-docker-36.html

https://www.cnblogs.com/littlesuns/p/11227992.html

https://www.docker.org.cn/book/install/arch-install-docker-36.html