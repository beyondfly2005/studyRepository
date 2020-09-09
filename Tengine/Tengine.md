## Tengine

Tengine和 OpenRegi



##### 1、官网下载

http://tengine.taobao.org

```
#安装到指定位置
./configure --prefix=/usr/local/tengine 

make && make install

cd 

./nginx

# 停止nginx
ps -ef | grep nginx
kill -9


```

nginx以服务方式启动

创建脚本

/etc/init.d/

vim 

chmod 777 ./nginx



#### 配置说明

```
worker_processes 4  # 通常与内核数相同  比如四核
events{
	worker_connections 1024 # 一个work_process 可以接收1024个连接  超过之后，不再连接
	# 并发总量是 4X1024
	worker_connections 10240 改为10240
}

http{
	include mine.type; # 文件类型
	sendfile  on
	tcp_nopush on;  #
}
```



每个进程最多打开多少个文件 97320

流处理 ：微批处理

gzip  服务器端压缩和 浏览器端解压缩

gzip_types 压缩类型

gzip_disable 

