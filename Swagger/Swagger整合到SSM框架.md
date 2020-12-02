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

