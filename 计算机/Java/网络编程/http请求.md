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
>
> [OkHttpClient单例和长连接Connection Keep-Alive](https://blog.csdn.net/sinat_36553913/article/details/104054028)

支持 HTTP/2

***注意：每个client对象都有自己的线程池和连接池，如果为每个请求都创建一个client对象，自然会出现内存溢出。所以官方建议OkHttpClient应该单例化，重用连接和线程能降低延迟和减少内存消耗**。*

### Get请求

```java
OkHttpClient client = new OkHttpClient();

String run(String url) throws IOException {
  Request request = new Request.Builder()
      .url(url)
      .build();

  try (Response response = client.newCall(request).execute()) {
    return response.body().string();
  }
}
```



## Apache HttpComponents 

> [官方文档](https://hc.apache.org/httpcomponents-client-4.5.x/quickstart.html)

### HttpClient

#### Get请求

```java
HttpGet httpGet = new HttpGet("http://127.0.0.1:8803");
try(CloseableHttpClient httpclient = HttpClients.createDefault();
	CloseableHttpResponse response = httpclient.execute(httpGet)) {
    System.out.println(response.getStatusLine());
    HttpEntity entity = response.getEntity();
    String resp = EntityUtils.toString(entity);
    System.out.println(resp);
} catch (IOException e) {
	e.printStackTrace();
}
```

### Fluent HC

#### Get请求

```
Request.Get("http://targethost/homepage").execute().returnContent();
```



# Java 类库

## URL+HttpURLConnection



## JDK11 HttpClient