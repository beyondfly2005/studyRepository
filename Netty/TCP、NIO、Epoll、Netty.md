> https://www.bilibili.com/video/BV1Af4y117ZK?seid=9198776770668101058

###### OSI 7层参考模型

- 应用层

- 表示层

- 会话层

- 传输控制层

- 网络层

- 链路层

- 物理层

###### TCP/IP协议

- 应用层
- 传输控制层
- 网络层
- 链路层
- 物理层

减少了表示层、和会话层

开发者实现应用层

```
exec 8<> /dev/tcp/www.baidu.com/80
echo "GET / HTTP/1.0\n"
echo "GET / HTTP/1.0\n" 1>& 8
cat 0<& 8
```

