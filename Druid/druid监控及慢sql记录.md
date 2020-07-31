> 参考文档

https://www.cnblogs.com/telwanggs/p/7484854.html

#### druid sql监控

因为已经做了整合，所以这一步较为简单，只需要在web.xml中做一下简单的Servlet配置即可。
```xml
<!--druid监控页面 -->
<servlet>
	<servlet-name>DruidStatView</servlet-name>
	<servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
	<init-param>
		<!-- 不允许清空统计数据 -->
		<param-name>resetEnable</param-name>
		<param-value>false</param-value>
	</init-param>
	<init-param>
		<!-- 用户名 -->
		<param-name>loginUsername</param-name>
		<param-value>yourname</param-value>
	</init-param>
	<init-param>
		<!-- 密码 -->
		<param-name>loginPassword</param-name>
		<param-value>yourpassword</param-value>
	</init-param>
</servlet>
<servlet-mapping>
	<servlet-name>DruidStatView</servlet-name>
	<url-pattern>/druid/*</url-pattern>
</servlet-mapping>
<!--druid监控页面 -->
```
重新构建工程并启动tomcat，在浏览器中输入druid即可进入到druid监控面板的登录页面。