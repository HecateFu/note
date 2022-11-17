[mybatis output sql](https://www.cnblogs.com/huangjinyong/p/11209753.html)

在mybatis.config.xml中增加如下配置：

```
mybatis.config.xml
<setting name="logImpl" value="STDOUT_LOGGING" />
```

或者，在SpringBoot中，修改application.yml文件

```yaml
mybatis:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

