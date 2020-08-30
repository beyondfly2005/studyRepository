1、什么是jwt，jwt优缺点有哪些

2、hwt有哪些组成部分

3、jwt与token之间存在哪些区别、

4、如何纯手写一个jwt

5、jwt如何设计过期时间

6、jwt与oauth2.0之间的区别

7、基于jwt+oauth2.0实现单点登录



Jwt （json web token）

oauth2.0开发协议



Token特征：临时



Jwt主要由三部分组成

- head
- PayLoad
- sign

##### jwt优缺点：

1、减轻服务器压力，jwt放到客户端，服务端不需要保存用户会话，只做验证

2、查询效率比从redis中查询token 效率高

3、不容易被客户端篡改数据

##### JWT缺点：

1、一旦生成好一个jwt，后期时候可以销毁；session可以被设置无效；可以加过期时间进行加固

2、jwt payload数据过多的话，会占用服务器端带宽资源，比token和session更长 