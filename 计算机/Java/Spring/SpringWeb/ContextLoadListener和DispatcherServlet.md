# ContextLoaderListener
[ContextLoaderListener作用详解](http://blog.csdn.net/ysughw/article/details/8992322)

[关于ContextLoaderListener那点事](http://blog.csdn.net/gzu_imis/article/details/18989679)

[【spring】源码分析 一 从ContextLoaderListener开始](http://blog.csdn.net/lihuapiao/article/details/51919283) 源码分析，还没看完

[【Spring】浅谈ContextLoaderListener及其上下文与DispatcherServlet的区别](http://www.cnblogs.com/weknow619/p/6341395.html)

ContextLoaderListener是引导性监听器，用于 启动 和 关闭 Spring 的  根 WebApplicationContext，委托（delegate）给 ContextLoader 和 ContextCleanupListener 执行。
如果配置了 org.Springframework.web.util.Log4jConfigListener（已过时） ，则 应该配置在 Log4jConfigListener 的后面；Spring 3.1 之后，ContextLoaderListener支持 通过构造函数直接注入根 WebApplicationContext ，因此允许 编程式的配置 Servlet 3.0+ 的环境。

对于一个web应用，**web容器提供**了一个全局的上下文环境，这个上下文就是**ServletContext**，其**为后面Spring IOC容器提供宿主环境**。在web容器启动时会触发容器初始化事件，**ContextLoaderListener监听器**的作用，就是**启动Web容器时自动装配ApplicationContext的配置信息**。

ContextLoaderListener继承自ContextLoader，实现了ServletContextListener这个接口，ServletContextListener是servlet 中 八个 分门别类的监听器之一，负责监听 Servlet Context 的生命周期 (该生命周期其实就对应着应用的生命周期)。

在web.xml配置这个监听器，启动容器或销毁容器时，就会默认执行它实现 ServletContextListener的contextInitialized和contextDestroyed方法。contextInitialized和contextDestroyed 方法分别调用了 ContextLoader里面的initWebApplicationContext和closeWebApplicationContext方法，所以核心最终还是 ContextLoader 实现了这个监听器。