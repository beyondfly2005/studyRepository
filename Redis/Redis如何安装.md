#### 一、源码方式安装redis

##### 1、下载安装文件

```bash
cd /usr/local/src
wget http://download.redis.io/releases/redis-3.2.3.tar.gz
```

##### 2、解压文件

```bash
tar -zxvf redis-3.2.3.tar.gz
mv redis-3.2.3 redis
```

3

```bash
cd redis
make && make install
```

