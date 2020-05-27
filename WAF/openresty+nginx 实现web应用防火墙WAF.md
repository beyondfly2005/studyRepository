> 参考文档

 	https://blog.csdn.net/chuanxincui/article/details/86089763
 	https://www.cnblogs.com/reblue520/p/6814072.html
 	https://blog.csdn.net/reblue520/article/details/76286892
 	https://github.com/unixhot/waf

```bash
#创建安装目录
mkdir -p /app/openresty/install
cd /app/openresty/install/

#解压
tar zxvf openresty-1.13.6.2.tar.gz

#进入目录
cd openresty-1.13.6.2

#安装基本包
yum install -y readline-devel pcre-devel openssl-devel gcc gcc-c++ perl.x86_64

#配置环境
./configure --prefix=/usr/local/openresty --with-luajit --with-http_v2_module --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module --with-ipv6 --with-http_sub_module --with-pcre --with-pcre-jit --with-file-aio --with-http_dav_module

#编译 
gmake

安装
gmake install

测试
修改nginx.conf，测试是否安装成功
cd /usr/local/openresty/nginx/conf/
编辑nginx.conf
vi nginx.conf

server {
	location /hello {
		default_type text/html;
		content_by_lua_block {
			ngx.say("HelloWorld")
		}
	}
}

启动nginx 一次执行下面代码段

cd ../sbin
ps -ef|grep nginx
./nginx -t
./nginx 
ps -ef|grep nginx

#关闭防火墙或者开放虚拟机防火墙端口，这里为了方便，直接就关闭防火墙了，生产中必须采用开放防火墙端口的方式
systemctl stop firewalld
systemctl status firewalld

#查看当前虚拟机ip地址
ifconfig
显示192.168.44.129

#浏览器访问nginx进行测试
http://192.168.44.129/hello

#浏览器显示HelloWorld 表明测试成功 nginx启动成功
```

部署waf
下载waf包  https://github.com/unixhot/waf
	

```bash
可以使用wget,也可以下载到本地，让后上传到虚拟机 /app/openresty/install
cd /app/openresty/install

--解压
tar zxvf waf.tar.gz

--将waf复制到 /usr/local/openresty/nginx/conf/
cp -r waf /usr/local/openresty/nginx/conf/

cd /usr/local/openresty/nginx/conf/
cd waf
```


```bash
#修改Nginx的配置文件，加入以下配置。注意路径，同时WAF日志默认存放在/tmp/日期_waf.log
#放到http节点之内，server节点之前  gzip之后
cd /usr/local/openresty/nginx/conf
vim nginx.conf

#WAF
lua_shared_dict limit 50m;
lua_package_path "/usr/local/openresty/nginx/conf/waf/?.lua";
init_by_lua_file "/usr/local/openresty/nginx/conf/waf/init.lua";
access_by_lua_file "/usr/local/openresty/nginx/conf/waf/access.lua";

#测试nginx配置文件
cd /usr/local/openresty/nginx/sbin
./nginx -t

#修改waf中的配置信息
pwd
/usr/local/openresty/nginx/conf
cd /waf
vim config.lua

#检查并配置如下项目
config_log_dir="/tmp"
config_rule_dir="/usr/local/openresty/nginx/conf/waf/rule-conf"
config_waf_redirect_url="http://www.vh.com"
```

```html
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta http-equiv="Content-Language" content="zh-cn" />
	<title>网站防火墙</title>
	</head>
	<body>
	<h1 align="center"> 欢迎白帽子进行授权安全测试，安全漏洞请联系QQ：1111111。
	</body>
	</html>
	改为
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta http-equiv="Content-Language" content="zh-cn" />
	<title>网站防火墙</title>
	</head>
	<body>
	<h1 align="center"> 您的行为已违反本网站相关规定，注意操作规范。
	</body>
	</html>
```


	waf目录：/usr/local/openresty/nginx/conf/waf
	lua配置文件：/usr/local/openresty/nginx/conf/waf/config.lua
	Waf的ip黑名单：/usr/local/openresty/nginx/conf/waf/rule-config/blackip.rule
	Waf的ip白名单：/usr/local/openresty/nginx/conf/waf/rule-config/whiteip.rule
	Waf的规则存放目录：/usr/local/openresty/nginx/conf/waf/rule-config


	测试WAF
	模拟sql注入,访问ip/test.sql,看是否出现拦截
	http://192.168.44.129/test.sql
	模拟参数检测，访问ip/?id=test,看是否出现拦截
	http://192.168.44.129/?id=test
	测试（一个注入攻击）：
	http://192.168.44.129/?name=test AND 1=1

```bash
./configure \
            --prefix=/data/server/openresty-1.15.8.2 \ 
			--with-pcre-jit \ 
			--with-ipv6 \  
            --with-http_stub_status_module \  
            --with-http_ssl_module \  
            --with-http_realip_module \  
            --with-http_sub_module  \  
            --with-http_geoip_module \  
            --with-openssl=/opt/openssl-1.1.1d \ 
            --with-pcre=/opt/pcre-8.43
	
```
```bash
./configure \ 
			--prefix=/usr/local/openresty \  
			--with-luajit \ 
			--with-http_v2_module \ 
			--with-http_stub_status_module \ 
			--with-http_ssl_module \ 
			--with-http_gzip_static_module \ 
			--with-ipv6 \ 
			--with-http_sub_module \ 
			--with-pcre \ 
			--with-pcre-jit \ 
			--with-file-aio \ 
			--with-http_dav_module

./configure --with-pcre-jit --with-ipv6 \  
            --with-http_stub_status_module \  
            --with-http_ssl_module \  
            --with-http_realip_module \  
            --with-http_sub_module  \  
            --with-http_geoip_module \  
            --with-openssl=/opt/openssl-1.0.2k \ 
            --with-pcre=/opt/pcre-8.40 \
```

```bash
# 如何停止openresty下的nginx
cd /usr/local/openresty/nginx/sbin
./nginx -s stop
# 如何启动openresty下的nginx
cd /usr/local/openresty/nginx/sbin
./nginx -t
./nginx 
ps -ef|grep nginx
```