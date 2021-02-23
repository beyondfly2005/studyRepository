#### 1、获取阿里云云加速地址

##### 1.1 登录阿里云

https://cr.console.aliyun.com/#/accelerator

登录之后 左侧菜单栏 镜像中心--镜像加速服务

获取加速器地址

#### 2、配置Docker加速

```bash
cd /etc/docker
ll
vim daemon.json 
```

将地址改为  刚才获取的阿里云镜像地址

```json
{
  "registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
}
```

修改后保存 

#### 3、重启Docker

```bash
# 
systemctl daemon-reload
# 
systemctl restart docker.service
```

#### 4、验证加速

```bash
docker info
```

结果

```bash
Registry Mirrors:
 https://xxxx.mirror.aliyuncs.com  ####### 这是是不是你配置的阿里镜像加速地址
Live Restore Enabled: false
Registries: docker.io (secure)
```

