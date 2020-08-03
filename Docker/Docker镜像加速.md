#  Docker镜像加速

> 修改/etc/docker/daemon.json

```bash
cd /etc/docker
vim daemon.json

#写入如下json
#阿里云为加速为自行注册的阿里云加速 不需要任何字符
```

```yaml
{
  "registry-mirrors": ["https://gntmfrqc.mirror.aliyuncs.com"]
}
```

> 重启

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

