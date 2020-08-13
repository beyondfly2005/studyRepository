#### Nginx常见问题处理

##### Nginx上传文件大小限制

###### 请求报文过大

https://blog.csdn.net/zhuchunyan_aijia/article/details/80744558

将client_max_body_size 从location节点 上移至 http节点内



#### Nginx常用命令   

```bash
#优雅停止nginx，有连接时会等连接请求完成再杀死worker进程  
nginx -s quit     

#优雅重启，并重新载入配置文件nginx.conf
nginx -s reload

#重新打开日志文件，一般用于切割日志
nginx -s reopen   

#查看版本
nginx -v      

#检查nginx的配置文件
nginx -t       

#查看帮助信息
nginx -h      

#详细版本信息，包括编译参数 
nginx -V    

#指定配置文件
nginx  -c filename  
```

