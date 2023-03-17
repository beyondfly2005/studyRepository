# API优先的设计
> https://www.bilibili.com/video/BV1x341137De

## API优先的设计-理论篇
API优先的设计 在应用开发之前 先把
类比：接口优先，接口优先是指在接口设计时 ，不用担心

API优先 

#### 微服务脚骨中的API
首先把API相关的

#### API优先的优势
- 避免不必要的API改动
- 提升开发效率
  - API的提供者和使用者 并行开发
  

## API优先的设计-工具篇

#### OpenAPI文档编辑器
详情参考 http://openapi.tools/网站
##### 文档编辑器
  - Stoplight Studio
##### Mock服务器
  - Prism
##### 代码生成工具
  - OpenAPI Generator
- 解析文档 安全工具

## API优先的设计-OpenAP文档篇

API的传输方式 与语言无关的方式

API的调动方式

OpenAPI文档规范
- Swagger
- OpenAPI Specification
- 使用yaml和json描述

OpenAPI规范的内容
- 路径 HTTP操作
  - 请求路径
  - 操作
  - 方法
  - 请求内容格式
  - 响应内容格式
- 辅助信息  
  - 基本信息
  - 联系方式
  - 授权协议
  - 访问控制
  - 服务器

#### 使用Stoplight Studio 创建OpenAPI文档

操作步骤
- 创建项目payment
- 创新新的OpenAPI 
- 填写OpenAPI的基本信息
- 查询支付状态接口
- 查看生成的API文档

## API优先的设计-Generator （1）

提提供者开发

消费者开发

使用工具 OpenAPI Generator
- 使用java开发
- 支持多种语言课框架

#### 开发方式
- 使用命令行方式
- 使用maven或gradle插件
- 使用在线服务生成

#### 命令行工具
- 直接下载Jar文件
- 使用Homebrew等包管理工具下载

命令行工具
- generate
- 辅助子命令
  - validate
  - list

生成器类别
- 


config-help

查看生成器的配置项
```shell
openapi-generator-cli config-help -g spring
```

spring生成器的配置项
```shell

```

查看generate 子命令

generate子命令
- -g 指定生成器
- -i 指定输入的OpenAPI文档
- -o 指定代码生成路径
- -p 参数指定

生成使用Spring的服务器端代码
```shell
open-generator-cli generate -g spring \
  -i payment-service.yaml \
  -o payment-service-server-psring
```

生成器特有的配置项
- 使用-p参数指定
- 通过 config-help查询


示例：使用spring生成器的额外参数
```shell
openapi-generator-cli generate -g spring
-i payment-service.yaml \
-o payment-service-serrver-spring
-p delegatePattern=true
```


集成maven或gradle
- 分服务器端与客户端两类场景

客户端

OpenAPI文档一般保存在src/main/resources目录下
使用OpenAPI Generator的maven插件
```pom
    <build>
        <plugins>
    <plugin>
      <groupId>org.openapitools</groupId>
      <artifactId>openapi-generator-maven-plugin</artifactId>
      <version>6.0.</version>
    <executions>
      <execution>
      <goals>
      <goal>generate</goal>
      </goals>
      <configuration>
      <inputSpec>${project.basedir}/src/main/resources/api.yaml</inputSpec>
      <generatorName>java</generatorName><packageName>${openapi.clientpackage}</packageName><apiPackage>${openapi.client.package}.api</apiPackage><modelPackage>$(openapiclient.package}model</modelPackage><invokerPackage>S{openapi.client.package).invoker</invokerPackage><generateModeTests>false</generateModelTests><generateApiTests>false</generateApiTests>
      </configuration>
      </execution>
    </executions>
  </plugin>
<plugins>
```



客户端工具
- 


## API优先的设计-Generator （2）


Java客户端
- java
- java-micronaut-client
- jaxrs-cxf-client


构建工具
- maven
- gradle
- sbt

生成方式
- 离线生成
  - 优点
    - 构建
  - 缺点
    - 把生成的代码添加到了代码仓库
- 基础到构建过程
  - 使用
  - 优点
    - 简介清晰
  - 缺点 
    - 需要手动复制客户端工具的依赖


离线生成


使用Maven插件生成代码
- 独立模块


maven插件
- a


pom文件依赖


使用客户端


配置 ApiClient


## API优先的设计策略、OpenAPI规范与相关工具的使用

API
Application Programming Interface 应用编程接口


后端作为API的开发者  

前端后受限于 AP的开发


API契约先于具体实现


##### 规范

gRPC规范

Graph 规范

OpenAPI规范
- 从Swagger 发展而来


使用编辑器

##### 实用工具
- vscode 插件
- Insomnia Designer 
- Stoplight
  - Stoplight Studio 集成IDE
  - Spectral
  - Pris


