# Docker安装GitLab

> Docker 配置国内源

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://jxus37ad.mirror.aliyuncs.com"]
}
EOF
#或者
vim /etc/docker/daemon.json
#写入
{
  "registry-mirrors": ["https://jxus37ad.mirror.aliyuncs.com"]
}
#保存退出
:wq

# 重新加载daemon 
sudo systemctl daemon-reload
# 重启docker
sudo systemctl restart docker
```



> 搜索可用的源

```bash
docker search gitlab-ce-zh  #ce社区版 zh中文版
#选择如下
twang2218/gitlab-ce-zh
```

> 安装gitlab

```bash
docker pull twang2218/gitlab-ce-zh
```



> 查看镜像

```bash
docker images

# 创建gitlab yml存放目录
mkdir /usr/local/docker/gitlab
cd /usr/local/docker/gitlab

# 添加yml文件
vim docker-compose.yml
```

> 编辑docker-componse.yml  将以下内容写入

```yml
version: '3'
services:
    web:
      image: 'twang2218/gitlab-ce-zh'
      restart: always
      container_name: 'gitlab'
      hostname: '192.168.44.129'
      environment:
        TZ: 'Asia/Shanghai'
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://192.168.44.129:8085'  #外部访问地址  与nginx端口一致
          gitlab_rails['gitlab_shell_ssh_port'] = 220  # ssh访问用的端口
          unicorn['port'] = 8888     #gitlab 内部端口
          nginx['listen_port'] = 8085
      ports:
        - '8085:8085'
        - '8443:443'
        - '220:22'
      volumes:
        - /uer/local/docker/gitlab/config:/etc/gitlab
        - /uer/local/docker/gitlab/data:/var/opt/gitlab
        - /uer/local/docker/gitlab/logs:/var/log/gitlab
```



> 启动docker-compose

```bash
docker-compose up
```

> 新建一个ssh连接 查看运行的镜像

```bash
docker ps
```

> 访问首页 需要漫长等待后

```
http://192.168.44.129:8085/
```







