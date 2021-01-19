## Java爬虫实战项目

> 课程地址：https://www.bilibili.com/video/BV1r4411E7Qj

### P3、难点分析

#### 难点分析

- 网站采取反爬策略
- 网站模板定期变动
- 网站URL抓取失败
- 网站频繁抓取IP被封

#### 网站发反爬策略--解决方案

模拟浏览器访问

#### 网站模板定期变动--解决方案

- 不同配置文件配置不同完整的模板规则
- 数据库存储不同网站的模板规则

#### 网站URL抓取失败--解决方案

- HttpClient默认处理方式
- Storm实时解析失败日志，将失败URL重新加入抓取仓库，一般超过3次就放弃

#### 网站频繁抓取IP被封--解决方案

- 购买代理IP库，随机获取IP抓取数据
- 部署多个应用分别抓取，降低单点频繁访问
- 设置每个页面赚钱时间间隔，降低被封概率

### P4、架构设计

- 总体架构解析
- 数据流向
- 模块划分
- 各模块解读

##### 数据采集模块

- 页面下载
  - HttpClient
- 页面解析
  - HtmlCleaner+Xpath
  - Jsoup
  - 正则表达式
- 数据接入
  - 之间存储数据库
  - 存储到HDFS

### P5、技术选型

- 数据采集层
  - HtppClient
  - Html Cleaner
  - XPath
  - X
- A



### P6、部署方案

- 爬虫项目：多台服务器

- 网站爬虫分类URL定时项目：一台服务器

- Hbase数据库：集群

- Solr服务器：集群

- Redis服务器：集群

- 爬虫监控项目：一台服务器

- Web项目：多台服务器

- Zookeeper服务器：多台服务器

  

#### P7、爬虫代码实现

- 爬虫目标
- 抓取字段：
- 实现功能
- 采用技术：



1、创建项目

2、引入依赖

​	httpclient

​	htmlcleaner

3、页面下载工具类

​	util包 PageDownloadUtil

```java
package com.beyondsoft.crawler.poetry.util;

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.IOException;

public class PageDownloadUtil {
    public static String getPageContent(String url) throws IOException {
        HttpClientBuilder builder = HttpClients.custom();
        CloseableHttpClient client = builder.build();

        HttpGet request = new HttpGet(url);
        CloseableHttpResponse response = client.execute(request);
        HttpEntity entity = response.getEntity();
        String content = EntityUtils.toString(entity);
        return content;
    }

    public static void main(String[] args){
        String url="";
        try {
            String content = PageDownloadUtil.getPageContent(url);
            System.out.print(content);
        } catch (Exception e) {
            System.out.print(e.getMessage());
        }
    }
}
```

4、 将页面信息保存到实体类

​	Page

```java
@Data
public class Page{
    private String content;
    private String url;
    //作者 朝代 类型 形式
}
```

爬虫执行入口



解析页面 ProcessPage

```java
public interface ProcessPage{
    public void process(Page page);
}
```

解析接口的实现类

```java
public void process(Page page){
	HtmlCleaner htmlCleaner = new HtmlCleaner();
    TagNode = rootNode = htmlCleaner.clean(content);
    ObjectrootNode,evaluateXPath("")  ; //使用xpath 解析  找到需要提取的字段 网站源代码审查元素F12 有复制XPath工具可以复制到这个
}
```

