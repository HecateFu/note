> 参考资料
>
> [java中文件复制的4种方式](https://www.cnblogs.com/xxjcai/p/11581987.html)
>
> [Java读取文本文件](https://www.yiibai.com/java/java-read-text-file.html)

FileChannel transferFrom 复制文件
```
  private static void copyFileUsingFileChannels(File source, File dest)
      throws IOException {
    FileChannel inputChannel = null;
    FileChannel outputChannel = null;
    try {
      inputChannel = new FileInputStream(source).getChannel();
      outputChannel = new FileOutputStream(dest).getChannel();
      outputChannel.transferFrom(inputChannel, 0, inputChannel.size());
    } finally {
      inputChannel.close();
      outputChannel.close();
    }
  }
```
