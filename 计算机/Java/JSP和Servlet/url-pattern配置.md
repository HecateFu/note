[web.xml中的url-pattern 写法小结(附源码分析)](https://blog.csdn.net/farawaywl/article/details/52902902)
# url-pattern详解

在web.xml文件中，以下语法用于定义映射：

1. 以”/’开头和以”/*”结尾的是用来做路径映射的。
2. 以前缀”*.”开头的是用来做扩展映射的。
3. “/” 是用来定义default servlet映射的。
4. 剩下的都是用来定义详细映射的。比如： /aa/bb/cc.action

**注意事项** 

1. 容器会首先查找精确路径匹配，如果找不到，再查找目录匹配，如果也找不到，就查找扩展名匹配。 
2. 如果一个请求匹配多个“目录匹配”，容器会选择最长的匹配(也就是更为精确的路径)。
3. 定义`/*.action`这样一个看起来很正常的匹配会报错？因为这个匹配即属于路径映射，也属于扩展映射，导致容器无法判断。

# 匹配规则和顺序如下

## 精确路径匹配

比如servletA 的url-pattern为 `/test`，servletB的url-pattern为 `/*` ，这个时候，如果我访问的url为`http://localhost/test` ，这个时候容器就会先进行精确路径匹配，发现`/test`正好被servletA精确匹配，那么就去调用servletA，也不会去理会其他的 servlet了

## 最长路径匹配

例子：servletA的url-pattern为`/test/*`，而servletB的url-pattern为`/test/a/*`，此时访问`http://localhost/test/a`时，容器会选择路径最长的servlet来匹配，也就是这里的servletB

## 扩展匹配

如果url最后一段包含扩展(如`*.do`)，容器将会根据扩展选择合适的servlet。

## 默认匹配

如果前面三条规则都没有找到一个servlet，容器会根据url选择对应的请求资源。如果应用定义了一个default servlet，则容器会将请求丢给default servlet。

# default servlet

1. web.xml中如果某个Servlet的映射路径仅仅为一个正斜杠（/），那么这个Servlet就成为当前Web应用程序的缺省Servlet。
2. 凡是在web.xml文件中找不到匹配的元素的URL，它们的访问请求都将交给缺省Servlet处理，也就是说，缺省Servlet用于处理所有其他Servlet都不处理的访问请求。
3. 在tomcat的安装目录\conf\web.xml文件中，注册了一个名称为org.apache.catalina.servlets.DefaultServlet的Servlet，并将这个Servlet设置为了缺省Servlet。(\conf\web.xml文件所有发布到tomcat的web应用所共享的)
4. 当访问Tomcat服务器中的某个静态HTML文件和图片时，实际上是在访问这个缺省Servlet，由DefaultServlet类寻找，当寻找到了请求的html或图片时，则返回其资源文件，如果没有寻找到则报出404错误。
5. 如果在web应用的web.xml文件中的中配置了”/”，如：
```xml
<servlet>
	<servlet-name>ServletDemo3</servlet-name>
	<servlet-class>edu.servlet.ServletDemo3</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>ServletDemo3</servlet-name>
	<url-pattern>/</url-pattern>
</servlet-mapping>
```
则当请求的url和上面其他的均不匹配时，则会交给ServletDemo3.Java处理,而不在交给DefaultServlet.java处理，也就是说，当请求web应用中的静态资源等时，则全部进入了ServletDemo3.java,而不会正常返回页面资源。

# servlet容器对url的匹配过程

当一个请求发送到servlet容器的时候，容器先会将请求的url减去当前应用上下文的路径作为servlet的映射url，比如我访问的是 `http://localhost/test/aaa.html`，我的应用上下文是test，容器会将`http://localhost/test`去掉， 剩下的`/aaa.html`部分拿来做servlet的映射匹配。这个映射匹配过程是有顺序的(按照servlet-mapping在web.xml中声明的先后顺序)，而且当有一个servlet匹配成功以后，就不会去理会剩下 的servlet了（filter不同，将符合条件都进行过滤）。

# 源码分析

想要了解url-pattern的大致配置必须了解`org.apache.tomcat.util.http.mapper.Mapper`这个类。

> 这个类上的源码注释：Mapper, which implements the servlet API mapping rules (which are derived from the HTTP rules).  意思也就是说  “Mapper是一个衍生自HTTP规则并实现了servlet API映射规则的类”。

tomcat在启动的时候会扫描web.xml文件, `org.apache.tomcat.util.descriptor.web.WebXml`这个类是扫描web.xml文件的, 然后得到的servlet的映射数据`servletMappings(HashMap)`, map的key是url-pattern，value是 servlet-name。

然后会调用`Context`(实现类为`StandardContext`)的`addServletMapping`方法。 这个方法会调用本文上面提到的`Mapper的addWrapper方法`. 
这个方法进行if-else判断 

1.  以 /* 结尾的。 path.endsWith(“/*”) 
2.  以 \*. 开头的。 path.startsWith(“*.”) 
3.  是否是 /,      path.equals(“/”) 
4.  以上3种之外 的。 

各种对应的处理完成之后，会存入context的各种wrapper中。这里的context是ContextVersion，这是一个定义在Mapper内部的静态类。 

它有4种wrapper。 defaultWrapper，exactWrapper， wildcardWrappers，extensionWrappers。  

**这里的Wrapper概念**：Wrapper 代表一个 Servlet，它负责管理一个 Servlet，包括的 Servlet 的装载、初始化、执行以及资源回收。

回过头来看mapper的addWrapper方法： 
1. 我们看到  /* 对应的Servlet会被丢到wildcardWrappers中 
2. *. 会被丢到extensionWrappers中 
3. / 会被丢到defaultWrapper中 
4. 其他的映射都被丢到exactWrappers中

用户请求过来的时候会调用mapper的internalMapWrapper方法,用户请求这里进行URL匹配的时候是有优先级的.

> 规则1：精确匹配，使用contextVersion的exactWrappers 
> 
> 规则2：前缀匹配，使用contextVersion的wildcardWrappers 
> 
> 规则3：扩展名匹配，使用contextVersion的extensionWrappers 
> 
> 规则4：使用资源文件来处理servlet，使用contextVersion的welcomeResources属性，这个属性是个字符串数组(如默认首页index.jsp) 
> 
> 规则7：使用默认的servlet，使用contextVersion的defaultWrapper