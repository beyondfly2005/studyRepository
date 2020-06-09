# docker-SonarQube

## Docker 搭建代码质量检测中文平台 SonarQube

[![img](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1591347309764&di=6a370afcbeca26318076aeebbb42d462&imgtype=0&src=http%3A%2F%2Fimg3.dns4.cn%2Fpic%2F192713%2Fp13%2F20170811125224_5418_zs_sy.jpg)](https://camo.githubusercontent.com/e6d5e03bd3a55cb584cab014210ba756401c6525/687474703a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f333238303238392d633332373138303565653130616238352e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### 下载编排文件

```
$ git clone https://github.com/Hello-Nemo/docker-SonarQube.git
$ cd docker-SonarQube
```

### 使用

```
$ docker-compose build
$ docker-compose up -d 
```

访问：localhost:9000

> username/password：admin/admin

**访问会很慢很慢  耐心等待。。。 **



```bash
# 进入到容器内
docker exec -it 88343b33744c bash
docker exec -it sonarqube_postgres_1 bash

# 查看日志
dockerlogs sonarqube_postgres_1
```





>参考文档
>
>https://github.com/Hello-Nemo/docker-SonarQube
>
>https://www.jianshu.com/p/eb891c37ffba





## 另一个版本的安装

> 参考文档

https://www.jianshu.com/p/194a86df8e17

https://hub.docker.com/r/jamesz2011/sonarqube6.7/

https://github.com/jamesz2011/sonarqube/blob/master/postgres_sonarqube.yml

#### 版本特点

- 将内嵌的数据库改为了关联外部数据库PostgreSQL数据库
- 系统默认语言设置为中文
- 安装了一些常用工具wget curl vim lrzsz
- 将镜像系统Debian9改为阿里Debian加速 并将默认时区改为Aisa/Shanghai



#### 获取compose yaml文件

```bash
cd /usr/local/docker
mkdir sonarqube
cd sonarqube
wget https://github.com/jamesz2011/sonarqube/blob/master/postgres_sonarqube.yml
mv postgres_sonarqube.yml docker-compose.yml

# 如果下载不到 直接 编辑
vim docker-compose.yml

# 添加一下内容
```

```yaml
#这是一个利用docker-compose来构建【sonarqube6.7+PostgreSQL】环境的yml文件

#sonarqube6.7的登录用户和密码均为admin,登录页面port为9000。

#PostgreSQL数据库的用户和密码均为sonar[可以在浏览器输入ip+8088或navicat工具访问数据库]。
version: "3"
services:
  db:
    image: postgres
    container_name: postgres
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar

    adminer:
      image: adminer
      restart: always
      ports:
        - 8088:8080

    sonarqube6.7:
      image: jamesz2011/sonarqube6.7:latest
      container_name: sonarqube
      ports:
        - "9000:9000"
        - "9092:9092"
      volumes:
        - /etc/localtime:/etc/localtime:ro
      links:
        - db
      environment:
        - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar
```

### 运行Docker

```bash
docker-compose up 
docker-compose up -d

# 如果yaml文件不改名的情况下 可以使用如下命令启动
docker-compose f postgres_sonarqube.yml up
```



**sonar**

