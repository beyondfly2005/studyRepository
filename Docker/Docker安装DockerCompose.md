# Docker 安装Docker Compose

> 安装docker-compose

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

