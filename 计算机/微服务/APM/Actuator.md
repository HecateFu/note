# Spring Boot Actuator 2.x

## http访问路径

默认 : `ip:port/actuator/[endpoint名称]` , eg: 健康检查 `ip:port/actuator/health`

配置：`management.endpoints.web.base-path` 

配置示例: 

````properties
# 访问路径变为 ip:port/[endpoint名称],健康检查的访问路径变为ip:port/health
management.endpoints.web.base-path=/
````



## 可通过http访问的endpoint

默认：info，health可访问，其他不可访问

配置（对1.x无效）：`management.endpoints.web.exposure.include`可通过http访问的endpoint、`management.endpoints.web.exposure.exclude`不可通过http访问的endpoint

配置示例：

```properties
# 所有启用的endpoint都可以通过 http 访问
management.endpoints.web.exposure.include=*
# 所有启用的endpoint都不可通过 http 访问
management.endpoints.web.exposure.exclude=*
# 所有启用的endpoints都可以通过 http 访问，除了 env 和 beans
management.endpoints.web.exposure.include=*
management.endpoints.web.exposure.exclude=env,beans
```



## 启用的endpoint

说明：启用是指是否将endpoint加载到spring boot的环境中，如果一个endpoint只启用了，但是没有通过http或者jmx的方式暴露，那么它也无法访问，如果一个endpoint没有启用，它也无法使用任何方式暴露。

默认：所有内置endpoint除了 shutdown

配置：`management.endpoint.[endpoint名称].enabled` 启用或关闭指定endpoint，`management.endpoints.enabled-by-default` 启用或关闭所有endpoint

配置示例：

```properties
# 关闭所有endpoint除了健康检查
management.endpoints.enabled-by-default=false
management.endpoint.health.enabled=true
```



# Spring Boot Actuator 1.x

## http访问路径

默认：`ip:port/`

配置：`management.context-path`

## 可通过http访问的endpoint

默认：非敏感endpoint可以直接访问，敏感endpoint无法直接访问

配置（对2.x无效）：`management.security.enabled` 是否对敏感endpoint进行权限验证（没有 Spring Security直接禁止访问，有spring security使用其进行权限验证），`endpoints.[endpoint名称].sensitive` 是否敏感endpoint
