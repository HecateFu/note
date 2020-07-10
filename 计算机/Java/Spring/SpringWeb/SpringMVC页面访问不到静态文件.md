[Spring mvc 项目中页面访问不到静态文件，如img ， js ， css 等](http://blog.csdn.net/responsecool/article/details/38821627 )  

起因：我们通常在springmvc项目中web.xml配置文件中的内容为：  
```xml
<servlet>
    <servlet-name>springmvc001-servlet</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:springmvc001-main.xml</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
	<servlet-name>springmvc001-servlet</servlet-name>
	<url-pattern>/</url-pattern>
</servlet-mapping>
```
这就导致用户的所用请求都被拦截给了DispatcherServlet    

解决办法有三种：  

# Tomcat DefaultServlet

第一种：在web.xml文件中激活Tomcat的defaultServlet来处理静态文件  

参见 [url-pattern配置 default servlet 章节](../JSP和Servlet/url-pattern配置.md#default-servlet)

```xml
<!-- 直接添加即可达到效果   有些文章中说要放在dispatcherservlet的后面，我测试了一下，在web.xml文件中没有不存在这个问题-->
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.jpg</url-pattern>
    <url-pattern>*.gif</url-pattern>
    <url-pattern>*.png</url-pattern>
    <url-pattern>*.js</url-pattern>
    <url-pattern>*.css</url-pattern>
</servlet-mapping>
```
# mvc:resources

第二种：spring3.0.4以后版本提供了mvc:resources ，在springmvc的主配置文件中添加  

```xml
<!-- 对静态资源文件的访问 -->       
<mvc:annotation-driven />  
<mvc:resources mapping="/images/**" location="/images/" />
```
`/images/**`映射到 `ResourceHttpRequestHandler`进行处理，`location`指定静态资源的位置.可以是web应用根目录下、jar包里面，这样可以把静态资源压缩到jar包中。cache-period 可以使得静态资源进行web cache 

再例如：
```xml
<mvc:resources location="/,classpath:/META-INF/publicResources/" mapping="/resources/**"/>  
```
以上配置将Web根路径`/`及类路径下 `/META-INF/publicResources/` 的目录映射为`/resources`路径。假设Web根路径下拥有images、js这两个资源目录,在images下面有bg.gif图片，在js下面有test.js文件，则可以通过 `/resources/images/bg.gif` 和 `/resources/js/test.js` 访问这二个静态资源。 

# mvc:default-servlet-handler

第三种：使用`<mvc:default-servlet-handler/>`，在springmvc主配置文件中加上这个标签就搞定了，不需要其他的额外的配置。 