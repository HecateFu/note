# spring boot 依赖

```
mybatis-plus-boot-starter
```

# 日志配置

> [mybatis-plus配置控制台打印完整带参数SQL语句](https://blog.csdn.net/moshowgame/article/details/90641657)

SpringBoot中，修改application.yml文件

```
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

