# 概念

## WSDL

Web Services Description Language， Web 服务描述语言

## SOAP

Simple Object Access Protoco，简单对象访问协议

# 集成 CXF 实现 WebService

## 服务端

导入依赖

- spring-boot-starter-web-service
- cxf-spring-boot-starter

定义配置类

- org.apache.cxf.Bus

- org.apache.cxf.jaxws.EndpointImpl

定义服务接口和实现类

- @WebService

  实现类的 @WebService 需要指定 `serviceName` 和 `targetNamespace`，需要和接口注解的这两个属性保持一致。实现类如果不指定，`serviceName` 默认使用类型，`targetNamespace` 默认使用包名的翻转，可能导致找不到服务中的方法。

- @WebMethod

默认访问路径

`http://127.0.0.1:8080/services/` 查看服务端所有服务

`http://127.0.0.1:8080/services/[服务地址]` 指定服务访问地址，`[服务地址]` 对应 `endpoint.publish("/hello");` 中的路径

`http://127.0.0.1:8080/services/[服务地址]?wsdl` 指定服务wsdl地址

## 客户端