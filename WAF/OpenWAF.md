### OpenWAF安装

https://www.cnblogs.com/ocean-boy/p/openwaf.html

### 安装所需依赖
```bash
yum install gcc gcc-c++ wget GeoIP-devel git swig make perl perl-ExtUtils-Embed readline-devel zlib-devel -y
```

	安装报错
		Cannot retrieve repository metadata (repomd.xml) for repository: docker-ce-stable. Please verify its path and try again
	处理方法
	    参考 https://blog.csdn.net/zhang_xinxiu/article/details/51763623
	解决办法如下：
	a. 打开/etc/yum.repos.d/xxxxx.repo，对于本例来说就是
	b. 将项[xxx]中的enabled=1改为enabled=0
	
	vim /etc/yum.repos.d/docker-ce.repo

```bash
$ #逐一安装 看下到底是哪个安装包依赖了docker
$ yum install gcc -y
$ yum install gcc-c++ -y
$ yum install wget -y
$ yum install GeoIP-devel -y
$ yum install git -y
$ yum install swig -y
$ yum install make -y
$ yum install perl -y
$ yum install perl-ExtUtils-Embed -y
$ yum install readline-devel -y
$ yum install zlib-devel -y
$ 
$ #安装报错
$ yum install GeoIP-devel -y 安装报错：   No package geoip-devel available.
$ 处理方法
$ $ 参考 http://www.foooy.me/linux/105.html
$ 处理方法
$ rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
$ rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
$ 安装完以上两个后，再次安装：
$ yum install GeoIP-devel -y
```
# 安装openwaf

    cd /opt
    git clone https://github.com/titansec/OpenWAF.git
    mv /opt/OpenWAF/lib/openresty/ngx_openwaf.conf /etc
    mv /opt/OpenWAF/lib/openresty/configure /usr/local/src/openresty-1.15.8.2/ # 这个configure文件要使用openwaf维护者的文件
    cp -RP /opt/OpenWAF/lib/openresty/* /usr/local/src/openresty-1.15.8.2/bundle/
    cp -RP /opt/OpenWAF/lib/openresty/* /opt/openresty-1.15.8.2/bundle/
    cd /opt/OpenWAF
    make install


git官网安装指南
    https://github.com/titansec/OpenWAF/blob/master/doc/%E8%BD%BB%E6%9D%BE%E7%8E%A9%E8%BD%ACOpenWAF%E4%B9%8B%E5%AE%89%E8%A3%85%E7%AF%87.md#docker%E5%AE%B9%E5%99%A8%E5%AE%89%E8%A3%85

基于CentOS系统安装

1、安装OpenWAF依赖
    cd /opt
    #yum install gcc wget git swig make perl build-essential zlib1g-dev libgeoip-dev libncurses5-dev libreadline-dev -y #这是在Ubuntu & Debian Linux操作系统下安装的命令
    yum install gcc gcc-c++ wget GeoIP-devel git swig make perl perl-ExtUtils-Embed readline-devel zlib-devel -y    #这是在CentOS系统安装的命令
    wget http://www.over-yonder.net/~fullermd/projects/libcidr/libcidr-1.2.3.tar.xz
    wget https://ftp.pcre.org/pub/pcre/pcre-8.43.tar.gz
    wget https://www.openssl.org/source/openssl-1.1.1d.tar.gz
    wget https://openresty.org/download/openresty-1.15.8.2.tar.gz
    tar -xvf libcidr-1.2.3.tar.xz
    tar -zxvf pcre-8.43.tar.gz
    tar -zxvf openssl-1.1.1d.tar.gz
    tar -zxvf openresty-1.15.8.2.tar.gz
    rm -rf pcre-8.43.tar.gz \
           openssl-1.1.1d.tar.gz \
           openresty-1.15.8.2.tar.gz
    cd /opt/libcidr-1.2.3
    make && make install

    cd /opt/pcre-8.43
    ./configure --enable-utf8
    make && make intall


    安装报错 No package build-essential available
    是因为当前是CentOS 使用了Ubuntu 的安装包

```bash

#安装 OpenWAF
    cd /opt
    git clone https://github.com/titansec/OpenWAF.git
    mv /opt/OpenWAF/lib/openresty/ngx_openwaf.conf /etc
    mv /opt/OpenWAF/lib/openresty/configure /opt/openresty-1.15.8.2
    cp -RP /opt/OpenWAF/lib/openresty/* /opt/openresty-1.15.8.2/bundle/
    cd /opt/OpenWAF
    make clean
    make install
    ln -s /usr/local/lib/libcidr.so /opt/OpenWAF/lib/resty/libcidr.so
```
```bash
编译 openresty
    cd /opt/openresty-1.15.8.2/
    ./configure --with-pcre-jit --with-ipv6 \
                --with-http_stub_status_module \
                --with-http_ssl_module \
                --with-http_realip_module \
                --with-http_sub_module  \
                --with-http_geoip_module \
                --with-openssl=/opt/openssl-1.1.1d \
                --with-pcre=/opt/pcre-8.43
    make && make install

    gmake && gmake install

```


    编译出现错误
    pwd
    /opt/openresty-1.15.8.2
    cd build/nginx-1.15.8/objs/autoconf.err