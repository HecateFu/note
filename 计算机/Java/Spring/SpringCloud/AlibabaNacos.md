# 注册中心

## 配置

`spring.cloud.nacos.discovery.enabled` 开启或关闭注册中心功能，默认true 开启，设置为false时不会连接nacos注册或获取服务

# 配置中心

## Spring Boot集成

1. pom配置

   必须的依赖 `spring-cloud-starter-alibaba-nacos-discovery` 、`spring-cloud-starter-bootstrap` ,配套的依赖管理 `spring-cloud-dependencies`、  `spring-cloud-alibaba-dependencies`

   完整pom如下:

   ```
   ```

2. 配置文件

3. 启动类

4. 使用

## 配置

`spring.cloud.nacos.config.enabled` 开启或关闭注册中心功能，默认true 开启，设置为false时不会连接nacos获取配置