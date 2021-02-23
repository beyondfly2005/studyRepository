>
>
>https://www.bilibili.com/video/BV1FK4y1y7qh?seid=3943250021786021496



#### 1、添加pom依赖

```xml
        <!--swagger-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.6.1</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.6.1</version>
        </dependency>
```

#### 2、添加config 配置类

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean Docket userApi(){
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any())
                .build();
    }
    
    @Bean
    public RequestMappingInfoHandlerMapping requestMapping(){
        return new RequestMappingHandlerMapping();
    }
}
```

#### 3、在spring中配置

```xml
    <bean class="com.ac.intellsecurity.config.SwaggerConfig"/>
    <!-- 配置swagger -->
    <mvc:resources mapping="swagger-ui.html"location="classpath:/META-INF/resources/" />
    <mvc:resources mapping="/webjars/**" location="classpath:/META-INF/resources/webjars/" />
	<bean id="swagger2Config" class="springfox.documentation.swagger2.configuration.Swagger2DocumentationConfiguration" />
```

#### 4、Swagger常用属性

##### @Api (value="xxx模块")

标记在类上，标识这个类是swagger资源  value  tags 

##### @ApiOperation(value="xx方法")

标记在方法上，表示一个http请求的操作  value  tags 

##### @ApiParam

标记在方法上法，表示对参数添加元数据（说明或是否必填）参数：字段说明  

@ApiParam(hidden=true)

##### @ApiModel()

标记在类上，标示对类进行说明，用于参数用实体类接收

