常用的处理 json 的类库 [Google 的 Gson](https://github.com/google/gson) 、[Alibaba 的 Fastjson ](https://github.com/alibaba/fastjson/wiki/Quick-Start-CN)、[FastXML Jackson](https://github.com/FasterXML/jackson-docs) ，spring-boot-start-web 中集成的是 Jackson。

# Jackson

> [Jackson in N minutes](https://github.com/FasterXML/jackson-databind/)

## Json字符串转List

```java
String jsonStr = "[\"123\",\"abc\",\"ddd\"]";
ObjectMapper jsonMapper = new ObjectMapper();
List<String> strList = jsonMapper.readValue(jsonStr, List.class);
```