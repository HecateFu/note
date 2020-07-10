###### Files.copy 复制文件

> [java中文件复制的4种方式](https://www.cnblogs.com/xxjcai/p/11581987.html)

```java
import java.nio.file.Files;

public class CopyFile{
    public static void copyFileUsingJava7Files(File source, File dest)
        throws IOException {
          Files.copy(source.toPath(), dest.toPath());
    }
}
```
