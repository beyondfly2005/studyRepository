#### docker version出现 Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

有时候输入任何docker的命令会报如下错误：

```bash
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. ...
```

```bash
# 原因可能是上一次没有正常退出docker，所以docker没有正常启动，在相应的/var/run/路径下找不到docker进程。
# 解决方法
sudo service docker restart
## 或者
sudo systemctl start docker
```

docker安装之后，已经安装了开机启动service文件，但还需要在设置下开机启动，才能在服务器重启时自动启动

```bash
systemctl enable docker
```