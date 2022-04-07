# RestTemplate 用法

> [Calling REST Service](https://docs.spring.io/spring-boot/docs/current/reference/html/io.html#io.rest-client)

线程安全，因为需要定制的场景比较多，所以Spring Boot 不提供自动注入的单例 RestTemplate 对象，但是提供自动注入的 RestTemplateBuilder 对象可用来创建 RestTemplate 对象。RestTemplateBuilder对象为创建的 RestTemplate 对象提供合适的 HttpMessageConverters，RestTemplateBuilder 还包含很多常用的方法来快速配置 RestTemplate。

通常可以这么用:

```java
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class MyService {

    private final RestTemplate restTemplate;

    public MyService(RestTemplateBuilder restTemplateBuilder) {
        this.restTemplate = restTemplateBuilder.build();
    }

    public Details someRestCall(String name) {
        return this.restTemplate.getForObject("/{name}/details", Details.class, name);
    }

}
```

如果想用 @Autowired 注入，需要先用配置类定义一个Bean：

```java
@Configuration
public class MyConfig {
	@Bean
    public RestTemplate createRestTemplate (RestTemplateBuilder rtb) {
        return rtb.build();
    }
}
```

然后，再在用到的类中使用 @Autowired 注入：

```java
@Service
public class MyService {
	@Autowired
    private RestTemplate restTemplate;

    public Details someRestCall(String name) {
        return this.restTemplate.getForObject("/{name}/details", Details.class, name);
    }

}
```

