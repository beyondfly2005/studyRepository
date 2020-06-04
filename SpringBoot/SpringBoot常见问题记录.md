### SpringBoot 常见问题



##### SpringBoot如何集成Jsp



##### SpringBoot如何集成mybatis



##### SpringBoot @MapperScan扫描的是接口还是实现

扫描的是接口



##### SpringBoot Mapper文件不在resources目录下应如何配置

application.properties中增加

```properties
mybatis.mapper-locations=classpath*:com/ac/intellsecurity/dao/*/impl/*.xml
```

或者

application.yml中增加

```yaml
mybatis: 
  mapper-locations: classpath*:com/ac/intellsecurity/dao/*/impl/*.xml
```

还需要在pom.xml文件中增加  （目的：让idea识别，让maven识别）

```xml
<!-- 编译 src/main/java 目录下的 mapper 文件 -->
<resource>
    <directory>src/main/java</directory>
    <includes>
        <include>**/*.xml</include>
    </includes>
    <filtering>false</filtering>
</resource>
```

##### SpringBoot如何集成tk.mybatis



##### SpringBoot如何集成mybatis



##### MyBatis 报错无效的列类型

**解决方法**

**方法一：指定插入值得jdbcType，将sql改成 **

```sql
insert into user(id,name) values(#{id,jdbcType=VARCHAR},#{name,jdbcType=VARCHAR}) 
```

**方法二 在mybatis-config.xml文件中配置一下，添加settings配置，如下：(推荐)**

```html
<configuration>
<settings>
    <setting name="jdbcTypeForNull" value="NULL" />
</settings>
</configuration>
```

**springboot集成mybatis 修改 application.properties**

```properties
mybatis.configuration.jdbc-type-for-null=null
```

**springboot集成mybatis 修改application.yml**

```yaml
mybatis: 
  configuration:
    jdbc-type-for-null: null
```



springboot 集成 mybatis-plus时

```yaml
mybatis-plus:
  configuration:
    jdbc-type-for-null: ‘null‘
```

##### SpringBoot如何集成druid数据源



##### SpringBoot集成druid数据源时如何检查进行密码加密







