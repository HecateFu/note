[Vuejs发送Ajax请求](https://www.cnblogs.com/EricZLin/p/9380406.html)

1. vuejs中没有内置任何ajax请求方法。
2. 在vue1.0版本，使用的插件 vue resource 来发送请求，支持promise
3. 在vue2.0版本，使用社区的一个第三方库 [axios](https://github.com/axios/axios) ，也支持promise
4. 在HTML5时代，浏览器增加了一个特殊的异步请求方法 fetch，原生支持promise，由于兼容性问题，一般用于移动端
5. 还有的项目会使用vue和jquery混用，借助jQuery的ajax方法

# axios的使用

##  安装和引用

> [use axios API with Vue CLI](https://dev.to/ljnce/use-axios-api-with-vue-cli-54i2)

1. 安装

   ```shell
   npm install --save axios vue-axios
   ```

2. 导入main.js

   ```javascript
   import Vue from 'vue'
   import axios from 'axios'
   import VueAxios from 'vue-axios'
   
   Vue.use(VueAxios, axios)
   ```

3. 使用

   ```
   this.axios.get()....
   ```

   

## 发送POST请求

```javascript
// 请求路径，请求参数，请求头等配置
this.axios.post("/fund/trans", this.trans, {
    headers: {
        "Content-Type": "application/json"
    }
}).then(function(response) { // 处理响应结果
    console.log(response);
}).catch(function(error) { // 处理异常信息
    console.log(error);
});
```



# 跨域配置

> [vue-cli 3.0 vue.config.js devServer](https://cli.vuejs.org/zh/config/#devserver-proxy)
>
> [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware#proxycontext-config)
>
> [Vue-cli3配置代理转发devServer.proxy](https://blog.csdn.net/mouday/article/details/106315236)

需求：生产环境前端资源集成在 spring boot fat jar 中，请求不跨域；开发环境前端后端分离，前端是vue-cli项目使用node运行，后端使用 spring boot 运行。

解决方案：在项目根目录增加 `vue.config.js` ，在其中添加开发环境代理配置 `devServer` ，具体配置如下：

```javascript
module.exports = {
	devServer: {
		proxy: {
			'/': { // key 是该路由规则适配的路径
				target: 'http://127.0.0.1/' // 请求转发到的地址，即服务的地址
			}
		}
	}
}
// 简化版
module.exports = {
  devServer: {
    proxy: 'http://127.0.0.1/'
  }
}
```



