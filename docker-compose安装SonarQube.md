#　Docker 搭建代码质量检测中文平台 SonarQube



####　下载编排文件



```bash
$ git clone https://github.com/Hello-Nemo/docker-SonarQube.git
$ cd docker-SonarQube
```

#### 使用



```bash
ifconfig # 查看ip
192.168.0.109

http://192.168.0.109:9000

$ docker-compose ps
$ docker logs docker-sonarqube_sonarqube_1

```





```bash
$ docker-compose build
$ docker-compose up -d 
```



#### 访问

浏览器访问　

http://localhost:9000

> username/password：admin/admin


参考文档：https://www.jianshu.com/p/eb891c37ffba
