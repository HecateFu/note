> 参考资料
>
> [Java8如何将Array转换为Stream的实现代码](https://segmentfault.com/a/1190000020247886)
>
> [Java8新特性：Stream流详解](https://blog.csdn.net/weixin_38294999/article/details/89277697)
>
> [Collectors收集器](https://www.jianshu.com/p/6ee7e4cd5314)
>
> [JAVA8 中的flatmap](https://blog.csdn.net/liyantianmin/article/details/96178586)
> 
> [Java8中利用stream对map集合进行过滤的方法](https://www.jb51.net/article/144709.htm)

# 创建 Stream

把数组、集合变成 Stream

- 数组

  数组工具类 Arrays.stream 或是 Stream.of
  ```
  String[] array = {"a", "b", "c", "d", "e"};
  //Arrays.stream
  Stream<String> stream = Arrays.stream(array);
  stream.forEach(x-> System.out.println(x));

  int[] intArray = {1, 2, 3, 4, 5};
  // 1. Arrays.stream -> IntStream
  IntStream stream = Arrays.stream(intArray);
  stream.forEach(x->System.out.println(x));
  ```

- Map

  先获取 Map 的 Entry 集合，再调用 Set 的 stream 方法获取 Stream
  ```
  Map<String,Object> m = new HashMap<>();
  Stream<Entry<String, Object>> mapStream = m.entrySet().stream();
  ```

# map

  把 Stream 中元素原有的 A 类型的对象，根据指定方法转换为 B 类型

# collect & Collectors

把 Stream 转成集合、数组

toSet
```
Stream.of(1,2,3,4,5,6,8,9,0).collect(Collectors.toSet());
```

toMap，Function.identity() 获取这个对象本身。
```
Stream.of(studentA,studentB,studentC).collect(Collectors.toMap(Student::getId,Function.identity()));
```
