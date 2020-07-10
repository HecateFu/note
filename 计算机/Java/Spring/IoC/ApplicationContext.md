获取 `spring.profiles.active` 的值

```java
// ac 是 ApplicationContext 接口的实例
String[] activeProfiles = ac.getEnvironment().getActiveProfiles();
```

> 参考资料
>
> [SpringBoot中获取spring.profiles.active的值](https://www.cnblogs.com/linzhanfly/p/9056722.html)