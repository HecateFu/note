# Spring 类库

## RestTemplate

> [The Guide To RestTemplate](https://www.baeldung.com/rest-template)
>
> [RestTemplate 403 forbidden](https://stackoverflow.com/questions/44922261/why-do-i-always-get-403-when-fetching-data-with-resttemplate)
>
> [Spring RestTemplate处理gzip压缩问题](https://www.jianshu.com/p/2ed17552d0c3)

spring-boot-starter-web

### 最简单的Get请求

```java
import org.springframework.http.*;
import org.springframework.web.client.RestTemplate;

RestTemplate rt = new RestTemplate();
String link = "http://baidu.com";
// 设置请求头，不设请求头容易出现 403
HttpHeaders header = new HttpHeaders();
header.setAccept(Arrays.asList(MediaType.TEXT_PLAIN));
header.add("user-agent","Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36");
HttpEntity<String> entity = new HttpEntity<>("parameters",header);
ResponseEntity<String> resp = rt.exchange(link,HttpMethod.GET,entity,String.class);
if(HttpStatus.OK.equals(resp.getStatusCode())){
    // 请求成功
    String raw = resp.getBody();
} else {
    System.out.println(resp.getStatusCode());
}
```



## WebClient

spring-boot-starter-webflux

# 第三方类库

## OkHttp

> [官方文档](https://square.github.io/okhttp/)

支持 HTTP/2

### 最简单的Get请求

```java

```



## Apache HttpComponents

HTTP 1.0 和 1.1

# Java 类库

## URL+HttpURLConnection



## JDK11 HttpClient