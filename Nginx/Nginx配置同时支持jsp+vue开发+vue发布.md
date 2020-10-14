#### 项目背景

项目从传统jsp页面转型到vue单页面开发，采用分部实施方案，新的需求开发用vue，原有的功能采用jsp暂时组作不动，逐步分时/分批将原有jps逐渐替代为vue实现。

既要保证原有的jsp页面访问正常；又要保证vue开发调试、发布上线 ；能vue项目和 jsp项目能同时访问一个共同的 API接口服务器。

从jsp到vue页面互相调用时，又需要不出现跨域或 二次登录等问题；还要满足vue的开发和部署 不能互相影响；部署工作简单灵活。



项目中先以App开始做部分页面的vue转化； App采用了混合开发模式；安卓做应用外壳，具体功能由jsp页面或vue页面实现



前端遇到的主要问题；

1、跨域问题

2、Cookie丢失问题

3、页面404、503等错误



为了实现项目不出现跨域问题；后端不需要跨域配置 ，采用Nginx反向代理方式

采用了基于一个项目Context目录，接口服务层 /api 和 vue页面 /view 都部署在这个目录下



```properties
http://IP:port/context/page   ## jsp页面
http://IP:port/context/view   ## view页面
http://IP:port/context/api    ## 接口文档

#具体事例
http://192.168.1.254:80/context/page   ## jsp页面
http://192.168.1.254:80/context/view   ## view页面
http://192.168.1.254:80/context/api    ## 接口文档
```



#### 各项目配置

vue项目 开发时使用8081端口  打包后的dist 发布到 nginx指定的目录 配置为root ；发布后不再占用端口

api项目 端口为8080 部署到一个tomcat服务器

jsp项目 端口为8080  同api接口部署到同一个tomcat服务器

nginx监听http的80端口，重定向到https的443端口 





#### Vue项目配置

配置vue.config.js

```javascript
devServer: {
    open: true,
    host: '0.0.0.0',
    port: 8081,
    https: false,
    hotOnly: false,
    proxy: { // 配置跨域

      '/IntellSecurity-App/api': {
        target: 'https://192.168.1.146/IntellSecurity-App/api',
        changeOrigin: true,
        pathRewrite: {
          '^/IntellSecurity-App/api': '/'
        }
      },
      '/files': {
        target: 'http://192.168.1.146/files/',
        changeOrigin: true,
        pathRewrite: {
          '^/files': '/'
        }
      }
     
    },

  }
```



#### Nginx配置

配置nginx安装目录下的conf/nginx.conf

```

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    upstream slb_server{

        server 192.168.1.146:8080;
    }

    server {
        listen 80;
        return 301 https://$host$request_uri;
    }

    server {
        listen       443 ssl;
        server_name  192.168.1.146;
        ssl_certificate cert/dtest.crt; #修改证书文件
        ssl_certificate_key cert/dtest.key;  #修改证书文件

        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;   #使用该协议进行配置
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;  #加密套件
        ssl_prefer_server_ciphers on;

	#vue项目的打包后的dist
        root        E:/myapp/dist;

        location /PushMessage/websocket/ {
			proxy_pass http://192.168.1.220:8088/PushMessage/websocket/ ;
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "Upgrade";
			proxy_set_header X-real-ip $remote_addr;
			proxy_set_header X-Forwarded-For $remote_addr;
		}

		# Jsp page
		location /IntellSecurity-App/ {
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header Host $http_host;
                    proxy_set_header X-Forwarded-Proto https;
                    proxy_cookie_path /IntellSecurity-App /IntellSecurity-App;
                    proxy_intercept_errors on;
                    proxy_redirect off;
                    proxy_connect_timeout      10;
                    proxy_send_timeout         240;
                    proxy_read_timeout         240;
            proxy_pass http://192.168.1.146:8080/IntellSecurity-App/;
        }
	
	# Vue page
	location /IntellSecurity-App/view {
                    #proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                    #proxy_set_header Host $http_host;
                    #proxy_set_header X-Forwarded-Proto https;
                    #proxy_cookie_path /IntellSecurity-App /IntellSecurity-App;
                    #proxy_intercept_errors on;
                    #proxy_redirect off;
                    #proxy_connect_timeout      10;
                    #proxy_send_timeout         240;
                    #proxy_read_timeout         240;
                   #proxy_pass http://192.168.1.146:8081/IntellSecurity-View;
            try_files $uri $uri/ @router;#需要指向下面的@router否则会出现vue的路由在nginx中刷新出现404
            index  index.html index.htm;
        }

        location @router {
            rewrite ^.*$ /index.html last;
        }
	
	# api接口
        location /IntellSecurity-App/api {
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header Host $http_host;
                    proxy_set_header X-Forwarded-Proto https;
                    proxy_cookie_path /IntellSecurity-App /api;
                    proxy_intercept_errors on;
                    proxy_redirect off;
                    proxy_connect_timeout      10;
                    proxy_send_timeout         240;
                    proxy_read_timeout         240;
            proxy_pass http://192.168.1.146:8080/IntellSecurity-App;
        }

        location /files {
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header Host $http_host;
                    proxy_set_header X-Forwarded-Proto https;
                    proxy_cookie_path /files /files;
                    proxy_intercept_errors on;
                    proxy_redirect off;
                    proxy_connect_timeout      10;
                    proxy_send_timeout         240;
                    proxy_read_timeout         240;
            proxy_pass http://192.168.1.146:8080/files;
        }

        #location / {
        #        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #        proxy_set_header Host $http_host;
        #        proxy_set_header X-Forwarded-Proto https;
        #        proxy_intercept_errors on;
        #        proxy_redirect off;
        #        proxy_connect_timeout      10;
        #        proxy_send_timeout         240;
        #        proxy_read_timeout         240;
        #        # note, there is not SSL here! plain HTTP is used
        #        proxy_pass http://slb_server;
        #}
        #charset koi8-r;

        #access_log  logs/host.access.log  main;


        error_page  502 503 504 /wh.html;
        location = /wh.html {
            root  html/wh/;
        }

        # redirect server error pages to the static page /50x.html
        #
        #error_page   500 502 503 504  /50x.html;
        #location = /50x.html {
        #    root   html;
        #}

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```

