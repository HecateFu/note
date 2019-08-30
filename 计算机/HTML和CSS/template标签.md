[Html5中的<template>标签](https://www.imooc.com/article/9415)

# 简介

HTML5中的标签，2013年出现的，用来声明是“模板元素”。

在html中如果有很多重复的结构，就可以把重复部分写在这个标签内部供整个文档调用。

# 特点

1. 根script标签一样是不可见的
2. 拥有content属性（通过这个也可以判断浏览器是否支持template标签）
3. 标签内的节点虽然不可见但是支持DOM操作

# 使用方法

使用之前需要了解一下两个方法：

1. importNode()

   importNode() 方法把一个节点从另一个文档**复制**到该文档以便应用。

2. DocumentFragment

   DocumentFragment是一个节点类型，代表轻量级的 Document 对象，能够容纳文档的某个部分，DocumentFragment 节点不属于文档树。


eg:

html

```html
<div id="box">
	<h3>链接支持</h3>
</div>

<template id="temp">
	<dl>
		<dt></dt>
		<dd><a href=""></a></dd>
	</dl>
</template>
```
js
```js
//这里通过content来判断浏览器是否支持template标签
if ('content' in document.createElement('template')) {
    var temp = document.getElementById('temp'),
        dt = temp.content.querySelector("dt"),
        a = temp.content.querySelector("a"),
        dt.textContent = "慕课网",
        a.textContent = "去学习",
        a.href = "http://www.imooc.com";

    var box = document.getElementById("box");
    var clone = document.importNode(temp.content, true);
    box.appendChild(clone);

    dt.textContent = "百度一下";
    a.textContent = "去搜索";
    a.href = "http://www.baidu.com";
    var clone2 = document.importNode(temp.content, true);
    box.appendChild(clone2);
}
```

最后HTML结构就是这样：

```html
<div id="box">
    <h3>链接支持</h3>
    <dl>
        <dt>慕课网</dt>
        <dd><a href="http://www.imooc.com">去学习</a></dd>
    </dl>
    <dl>
        <dt>百度一下</dt>
        <dd><a href="http://www.baidu.com">去搜索</a></dd>
    </dl>
</div>
```

代码说明：

`temp.content.querySelector("dt")；`要对template标签内的节点进行操作必须通过temp.content返回的节点来操作。

解释一下为什么不能直接temp.querySelector("dt")，这里的temp并不是一个正常的文档结构，通过consloe.log(temp)来看，temp返回的是`<template>`中的一个节点片段，而不是直接的节点，在chrome中后台打印的是：

```html
<template id="temp">
    #document-fragment  
//这就是DocumentFragment节点，这里面才包含了template中的HTML结构
</template>
```
所以这里不能用temp.querySelector("dt")来操作。当然通过`temp.content`得到就是`#document-fragment`，通过它就能读取到节点。

`document.importNode(temp.content, true);`这个是导入节点，理解上面DocumentFragment特点后，
就明白为什么不使用innerHTML来写入节点

# vue.js中的应用

[Vue中的template标签的使用和在template标签上使用v-for](https://www.cnblogs.com/taohuaya/p/10686420.html)

[条件渲染](https://cn.vuejs.org/v2/guide/conditional.html)

[列表渲染](https://cn.vuejs.org/v2/guide/list.htm)

## 条件渲染和列表渲染分组

`v-if`、`v-else`、`v-for`等条件控制指令必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 `v-if`。最终的渲染结果将不包含 `<template>` 元素。

eg:

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

[单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)

## 单文件组件

复杂的js项目中使用js模块化开发。vue.js框架中使用文件扩展名为 `.vue` 的 **single-file components(单文件组件)** 来定义组件，构建工具（webpack 、Browserify 等配合vue-loader）来生成最终浏览器解析的文件。

vue单文件组件就是把一个页面拆分成为多个、多层次的组件。通过多层引用，大大缩小vue文件的长度和页面复杂度。

单文件组件的html部分定义在`<template>`标签中。构建工具打包后，使用到相应组件的地方会替换为template中定义的内容。

eg:

```vue
<template>
  <p>{{ greeting }} World!</p>
</template>

<script>
module.exports = {
  data: function () {
    return {
      greeting: 'Hello'
    }
  }
}
</script>

<style scoped>
p {
  font-size: 2em;
  text-align: center;
}
</style>
```



