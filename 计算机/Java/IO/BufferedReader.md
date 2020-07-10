###### readLine() 整行读取

```java
InputStream is = this.getClass().getResourceAsStream("/rankhandler.js");
try (BufferedReader br = new BufferedReader(new InputStreamReader(is,"UTF-8"))) {
	String raw = br.readLine();
} catch (IOException e){
    //TODO 异常信息记录日志
    e.printStackTrace();
}
```

