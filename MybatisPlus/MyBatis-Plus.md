>  视频地址：https://www.bilibili.com/video/BV1KV411U7pH?p=3



对于自增主键，数据插入成功后，主键ID自动会设置到对象上，直接使用.getId()  就可以获取



#### 驼峰式命名规则的处理





最佳实践：

自动映射会对 表名 做处理，牺牲一些性能

建议全部使用驼峰命名：



#### @TableField字段 

```
@TableField(value="name")
private String table_name;

```



```
@TableField(exist=false)
private String status;
```







https://www.bilibili.com/video/BV12t411B7e9?p=2

PigX多租户实现

```
com.baomidou.mybatisePlus.extends.tenant.TenantHander



```

##### 数据权限

在角色上  四种权限类型

DataScope

入参 newDataScope

```
@Inte
```

