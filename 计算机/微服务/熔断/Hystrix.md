[Spring Cloud Feign 熔断配置的一些小坑](https://my.oschina.net/u/1758970/blog/1798279)

feign的hystix的开关

```
feign:
  hystrix:
    enabled: true
```



[feign hystrix 线程池伸缩控制](https://juejin.cn/post/6844903609390333960)

```
# 最小线程数 inventory-service 服务名称，所有服务通用配置用default
hystrix.threadpool.inventory-service.coreSize=5
# 最大线程数
hystrix.threadpool.inventory-service.maximumSize=20
# 线程在被释放之前将使用多长时间
hystrix.threadpool.inventory-service.keepAliveTimeMinutes=1
# 设为 true ，允许设置最小线程数 和 最大线程数
hystrix.threadpool.inventory-service.allowMaximumSizeToDivergeFromCoreSize=true
```



[maximumSize有效果的正确使用方式](https://zhuanlan.zhihu.com/p/161522189)



[用法及常用配置](https://www.jianshu.com/p/ea9dc730022c)

[常用配置](https://zhuanlan.zhihu.com/p/79891347)

