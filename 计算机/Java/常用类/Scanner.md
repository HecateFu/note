[菜鸟教程-Java Scanner 类](https://www.runoob.com/java/java-scanner-class.html)

| 方法名     | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| nextInt()  | 只会读取数值，剩下"\n"还没有读取，并将cursor放在本行中。如果想要在nextInt()后读取一行，就得在nextInt()之后额外加上scanner.nextLine()，否则下一次读取时会读取到前面剩下的“\n”。 |
| nextLine() | 以Enter为结束符,也就是说 nextLine()方法返回的是输入回车之前的所有字符。可以获得空白。 |
| next()     | 一定要读取到有效字符后才可以结束输入，对输入有效字符之前遇到的空格键、Tab键或Enter键等结束符，next()方法会自动将其去掉，只有在输入有效字符之后，next()方法才将其后输入的空格键、Tab键或Enter键等视为分隔符或结束符。next方法不能得到带空格的字符串。 |

