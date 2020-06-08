## Registry (镜像管理平台)

#### 安装 Docker Registry私服

##### 简介



##### 安装

```yaml
version: 3.1
services:
  registry:
  image: registry
  restart: always
  container_name: registry
  ports:
    - 5000:5000
  volumns:
    - /usr/local/docker/registry/data:/var/lib/registry
```



##### 测试

启动成功后需要测试服务器端是否能够正常提供服务，有两种方式测试：

- 浏览器访问

  http://ip:5000/v2/

- 终端访问

```bash
curl http://ip:5000/v2/
```



#### 配置Docker Registry客户端

#### 部署Docker Registry WebUI

