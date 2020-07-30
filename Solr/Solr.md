### 搜索实现方案的分析

#### 1、SQL模糊查询实现搜索

​	模块查询select * from t_product where name like '%关键词%'

​	**存在的问题**

##### **导致索引失效**

​	没有索引时，搜索会导致全表扫描

​	为提高索引的速度 为数据库表添加索引，类似书籍的目录，本质是一种数据结构

​	1>、哈希数据结构：方便定值数据查找

​	2>、树形数据结构：方便范围查找、提高搜索速度

​	3>、增加索引导致的副作用：更新操作会变慢，比起没有索引 要多维护了这棵树

​	4>、like ‘%A%’ 会导致索引失效，没有起始参照，会走权标扫描

​			like ‘A%’ 这样可以走索引，以A为参照，但是不能实现全文搜索 模糊查询等 不符合业务需求

​	BTREE索引结构 

##### 无法实现 自动分词搜索

​	比如“Nginx启动失败” 应该 匹配 Nginx-启动 -失败 三个关键词

#### 2、Mysql在5.6之后提供了一个全文索引

​	不使用MySQL全文索引的原因，是因为减轻数据库压力，让数据库少干点活；

​	走的是磁盘IO方式，不方面扩展；

#### 3、成熟的搜索引擎技术来踢打数据库搜索

- solr  Apache产品

- ES(elasticSearche )大数据分析

  适用范围：站内搜索


### 安装Solr服务器

#### 1、什么是Solr

Solr是用Java编写、运行在Servlet容器（Apache Tomcat 或Jetty）的一个独立的全文搜索服务器。Solr采用了Lucene Java 搜索库为核心的全文索引和搜索，并具有类似REST的HTTP/XML和Json的API。Solr强大的外部配置功能使得无需进行Java编码，便可以对其进行调整以适应多种类型的应用程序。

#### 2、Solr的运行环境

以 版本 Solr 4.10.4为例

JDK 1.7 以上
Servlet容器：Tomcat
Java平台：Windows或Linux

#### 3、下载JDK

```html
https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
```

#### 4、安装JDK

```bash
cd /usr/local/jdk
#解压jdk压缩包
tar -zxvf jdk
vim /etc/profile
#增加环境变量JAVA_HOME 修改PATH
source /etc/profile
# 验证
java -version 
```

#### 5、安装tomcat

```bash
cd /usr/local/tomcat
#解压
tar -zxvf tomcat.tar.gz
```

#### 6、下载Solr

```bash
https://lucene.apache.org/solr/downloads.html
##全部版本
http://archive.apache.org/dist/lucene/solr/

# 7.7.3/8.5.2版本可以从清华大学网站下载
https://mirrors.tuna.tsinghua.edu.cn/apache/lucene/solr/
```

#### 7、Solr安装 及目录介绍

```bash
# 上传solr压缩包 解压
tar -zxvf solr-4.10.4.tgz

# 目录介绍
bin：soklr的运行脚本
dist：包含一个可以连通tomcat和solrhome的可运行的war包
docs：solr的API文档
example：solr工程的示例目录
example/solr：该目录是一个标准的SolrHome，它包含一个默认的SolrCoare
```

#### 8、创建SolrHome(SolrCore)

```bash
解压后的solr-4.10.4/example/solr 文件夹就是一个标准的SolrHome；
将其拷贝到自己的某个文件夹下（/usr/local/），改名为solrhome 
SolrHome是Solr运行的主目录，该目录中国可以包含多个solrcore目录，solrcore目录中包含了运行solr实例的所有的配置文件和数据文件，solrcore就表示一个solr实例
关系如下：
--solrHome
	--solrCore (solr实例 单独提供搜索服务) 如商品搜索服务
		--conf 配置文件
		--data 数据文件 (启动服务的时候自动生成)
```

```bash
# SolrHome 可以包含多个SolrCore
cd example
cp -r solr /usr/local/solrhome
cd /usr/local
cp apache-tomcat-7.0.99.tar.gz /usr/local/solr
cd /usr/local/solr
tar -zxvf apache-tomcat-7.0.99.tar.gz
mv apache-tomcat-7.0.99 tomcat7
# rm -rf apache-tomcat-7.0.99.tar.gz
cd /usr/local/solr/solr-4.10.4/dist
cp solr-4.10.4.war /usr/local/solr/tomcat7/webapps/
# 启动tomcat
cd /usr/local/solr/tomcat7/bin
./startup.sh
tail -f ../logs/catalina.out
cd ../webapps
ll
mv solr-4.10.4 solr
rm -rf solr-.10.4.war

```
#### 9、 Tomcat部署solr.war，连接solrhome

```bash
1、从solr解压包下的solr-4.10.4/dist目录中拷贝solr-4.10.4.war,复制到tomcat安装目录下的webapps目录下
2、启动tomcat解压war文件，然后关闭tomcat 删除solr-4.10.4.war 重命名解压后的的工程目录为solr
3、在slor工程目录下的WEB-INFO里面创建classes目录

添加扩展日志包
1、将solr解压包下的solr-4.10.4/example/lib.ext 目录下的所有jar包 复制到解压后的solr工程目录下的WEB-INFO/lib目录
2、把solr解压熬下的solr-4.10.4/example/resources/log4j.properties文件夹复制到解压后的solr工程目录下的WEB-INFO/classes目录

到此solr工程环境准备就绪
启动tomcat
```
```bash

cd solr
ll
# 增加日志
cd WEB-INF/lib
cd WEB_INF
mkdir classes #用于存放log4j配置文件

cd /usr/loca/solr/solr-4.10.4/
# 将sor-4.10.4/example/lib/ext目录下的所有jar包复制到soolr工程的WEB-INF/lib目录下
cd example/lib/ext
ll
cp -r * /usr/local/solr/tomcat7/webapps/solr/WEB-INF/lib/
# 将log4j配置文件拷贝到classes中

cd /usr/local/solr/solr-4.10.4/example/resources/
cp log4j.properties  /usr/local/solr/tomcat7/webapps/solr/WEB-INF/classes

#在solr应用中的web.xml文件中 加载SolrHome
cd /usr/local/solr/tomcat7/webapps/solr/WEB-INF/

vim web.xml

# 增加如下内容：
```

```xml
# 取消文档的注释
<env-entry>
    <env-entry-name>sor/home</env-entry-name>
    <evn-entry-value>/put/your/sor/home/here</evn-entry-value>
    <env-entry-type>java.lang.String</env-entry-type>
</env-entry>

# solrhome 目录 # /usr/local/sorhome

<env-entry>
    <env-entry-name>sor/home</env-entry-name>
    <evn-entry-value>/usr/local/solrhome</evn-entry-value>
    <env-entry-type>java.lang.String</env-entry-type>
</env-entry>

```

```bash
# 启动tomcat
cd /usr/local/solr/tomcat7/bin
./startup.sh
tail -f ../logs/catalina.out

# 验证安装是否成功 浏览器访问
http://192.168.198.128:8080/solr
```

#### 10、安装中文分词器

IKAnalyzer分词器

下载地址： https://pan.baidu.com/s/1nlgxWkne_LsCQtjE4SHHzg  提取码: dais

- 将jar包上传到服务器

- 将IKAnalyzer依赖的jar包添加到slor工程中

  将dist/IKAnalyzer2012FF_u1.jar 上传到 /usr/local/solr/tomcat7/webapps/slor/WEB-INF/lib

- 将分析器使用额配置文件添加到classes中

  将ILAnalyzer.cfg.xml  和 stopword.dic 上传到/usr/local/solr/tomcat7/webapps/slor/WEB-INF/classes

- 修改schema.xml文件增加如下内容
```bash
cd /usr/local/solrhome/collection1/
cd conf
vim schema.xml

# 增加如下内容
```

```xml
<!--IKAnalyzer-->
<fieldType name="text_ik" class="solr.TextField">
  <analyzer class="org.wltea.analyzer.lucene.IKAnalyzer"/>
</fieldType>

```

```bash
# 重启taomcat
cd /usr/local/solr/tomcat7/bin
./shutdown.sh
# 测试分词器作用
```

词典 停词 扩展新词 自定新词

```bash
cd /usr/local/solr/tomcat7/webapps/solr/WEB-INF/classes/
vim IKAnalyzer.cfg.xml
# 取消注释
# <entry key="ext_dict">ext.dic<entry>

vim ext.dic
# 增加 需要的新词 如 科比 
# 增加多个新词 每个新词 独占一行 \r\n方式换行
# 文件为无BOM的UTF-8格式
# <entry key="ext_dict">ext.dic<entry> 可以配置多个分词文件，分词文件直接以分号分割 例如<entry key="ext_dict">ext.dic;ext2.dic;ext3.dic<entry>
```

Mmseg4j 另外一款中文分词器

#### 11、搭建搜索库

```bas
接下来，我们要搭建索引库
主要有两个步骤：
1、自定义域，主要作用是描述索引信息
2、通过solr将数据库表的数据导入索引库，设置表字段和索引库域的映射关系，从而建立索引库
```



#### 12、建立索引库第一步—自定义域

​	修改schema.xml

```xml

加入如下代码：
<!-- 商品名称 -->
<field name="product_name" type="text_ik" indexed="true" stored="true"/>
<!-- 商品价格 -->
<field name="product_price" type="double" indexed="false" stored="true"/>
<!-- 商品图片 -->
<field name="product_image" type="text_ik" indexed="true" stored="true"/>
<!-- 商品卖点 -->
<field name="product_sale_point" type="text_ik" indexed="true" stored="true"/>
<!-- 商品介绍 -->
<field name="product_desc" type="text_ik" indexed="ftrue" stored="true"/>
<!-- 目标域-->
<field name="product_goods" type="text_ik" indexed="true" stored="true" multiValued="true"/>
<!-- 将商品名称添加到目标域 -->
<copyField source="product_name" dest="product_goods"/>
<!-- 将商品介绍添加到目标域 -->
<copyField source="product_desc" dest="product_goods"/>

```



### SpringBOot整合Solr 操作solr数据

#### 1、目标：搭建搜索服务接口及实现

#### 2、SpringBoot整合Solr

- 创建工程 添加solr依赖

- 配置solr服务器地址

- 使用solr提供的客户端操作API

  ​	@Autowired

  ​	private SolrClient solrClient;

#### 3、单元测试，掌握对索引库的增删改查操作



### 开发搜索系统，实现商品信息的搜索及展示

### 实现关键词高亮显示

### 实现搜索结果的分页效果

### Solr原理，为什么快

### Solr服务器多个solr-core的情况

### SolrCloud 多个服务器部署同一个soolr服务