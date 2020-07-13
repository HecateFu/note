> [Java Base64 Encoding and Decoding](https://www.baeldung.com/java-base64-encode-and-decode)

# 解密

## Url Base64 解密

```java
String a = "密文";
byte[] bytes = Base64.getUrlDecoder.decode(a);
try {
    String b = new String(a,"UTF-8");
} catch (UnsupportedEncodingException e) {
    e.printStackTrace();
}
```

