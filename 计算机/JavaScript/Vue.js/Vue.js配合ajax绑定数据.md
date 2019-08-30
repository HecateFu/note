[Vuejs发送Ajax请求](https://www.cnblogs.com/EricZLin/p/9380406.html)

1. vuejs中没有内置任何ajax请求方法。
2. 在vue1.0版本，使用的插件 vue resource 来发送请求，支持promise
3. 在vue2.0版本，使用社区的一个第三方库 [axios](https://github.com/axios/axios) ，也支持promise
4. 在HTML5时代，浏览器增加了一个特殊的异步请求方法 fetch，原生支持promise，由于兼容性问题，一般用于移动端
5. 还有的项目会使用vue和jquery混用，借助jQuery的ajax方法

# axios的使用

##  安装和引用

1. npm直接下载axios组件，下载完毕后axios.js就存放在node_modules\axios\dist中

   ```shell
   npm install axios
   ```

2. 网上直接下载axios.min.js文件，或者使用cdn，通过script src的方式进行文件的引入

   ```html
   <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
   ```

## 发送get请求

