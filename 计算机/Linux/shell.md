[定时取内存保存到文件](https://blog.csdn.net/zhongweidu3/article/details/88785580)

```shell
echo "%CPU,%MEM" > cpu_test.csv
pid=1    #Can be change by yourself
while true              
  do
        top -bn1 -n 1 -p $pid | tail -1 | awk '{ print $9,$10 }' | sed 's/ /,/' >> cpu_test.csv
    sleep 2    #delay time
done
```



[nohup、输出流、重定向](https://blog.csdn.net/ianly123/article/details/85113539)



[Shell字符串拼接（连接、合并）](http://c.biancheng.net/view/1114.html)