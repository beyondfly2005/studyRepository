#### Nginx常见问题处理

##### Nginx上传文件大小限制(请求报文过大)
https://blog.csdn.net/zhuchunyan_aijia/article/details/80744558

将client_max_body_size 从location节点 上移至 http节点内

