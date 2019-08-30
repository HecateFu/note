# 一、配置pom文件

[Using Spring Boot - Build System - Maven](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/using-boot-build-systems.html#using-boot-maven)

## 继承starter-parent

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

说明：

1. springboot官方推荐我们使用spring-boot-starter-parent，spring-boot-starter-parent 提供以下功能：
	1. 使用java8编译级别。
	2. 使用utf-8编码。
	3. 继承spring-boot-dependencies pom，对常用依赖进行版本管理。使用依赖管理模块可以让你在配置这些依赖时省略<version>标签。
	4. <pluginManagement>中定义了 spring-boot-maven-plugin 默认配置，配置了一个<execution>，<id>是repackage，仅有一个<goal>repackage。
	5. <resource>配置，支持资源过滤。
	6. <pluginManagement>中还定义了其他常用插件的默认配置(exec plugin, surefire, Git commit ID, shade)。
	7. 配置文件application.properties和application.yml的智能过滤，包括特定环境的偏好配置文件，e.g. application-dev.properties和application-dev.yml)

	注意`application.properties`和`application.yml`配置文件中接受spring风格的占位符，maven的占位符变为@...@(可以通过配置maven属性resource.delimiter来改变maven占位符)

2. 改变java的编译级别，添加属性：
```xml
<properties>
	<java.version>1.8</java.version>
</properties>
```

3. 继承 spring-boot-starter-parent 的方式使用 spring-boot-dependencies ，可以通过重写特定的`<properties>`来修改某个默认的依赖版本，eg：
```xml
<properties>
	<spring-data-releasetrain.version>Fowler-SR2</spring-data-releasetrain.version>
</properties>
```
其他可覆盖的properties详见，[spring-boot-dependencies](https://github.com/spring-projects/spring-boot/tree/v2.1.7.RELEASE/spring-boot-project/spring-boot-dependencies/pom.xml)的pom文件中。

4. 使用了 spring-boot-dependencies 后，再引入其他的starter时就可以省略版本了，如上demo中的spring-boot-starter-web。

## 不继承starter-parent

我们也可以不继承spring-boot-starter-parent，同时还能使用springboot的依赖管理模块，通过在<dependencyManagement>中引用一个`scope=import`的spring-boot-dependencies依赖实现，配置如下：

```xml
<dependencyManagement>
     <dependencies>
        <dependency>
            <!-- Import dependency management from Spring Boot -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.5.9.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
使用这种方式来利用springboot默认依赖配置时，不能通过覆盖properties的方式来修改某个依赖的版本，如果想修改默认依赖版本，需要在<dependencyManagement>中**spring-boot-dependencies配置的前面**加入改变版本的依赖配置，eg：
```xml
<dependencyManagement>
    <dependencies>
        <!-- Override Spring Data release train provided by Spring Boot -->
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-releasetrain</artifactId>
            <version>Fowler-SR2</version>
            <scope>import</scope>
            <type>pom</type>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.5.9.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
## Spring Boot Maven Plugin 插件

[Creating an Executable Jar](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/getting-started-first-application.html#getting-started-first-application-executable-jar)

spring-boot-maven-plugin可以直接将spring boot 项目打包为包含所有依赖的可执行jar，继承spring-boot- starter-parent后，使用时如果不需要对spring-boot-maven-plugin进行特殊配置，只需如下配置即可：

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>
```

执行完`mvn package`会发现`target`目录中生成了一个比较大的 `myproject-0.0.1-SNAPSHOT.jar`和一个比较小的 `myproject-0.0.1-SNAPSHOT.jar.original` ，较大的这个jar就是 spring-boot-maven-plugin 生成的可执行jar。使用以下命令可以查看这个jar包的具体内容：

```bash
$ jar tvf target/myproject-0.0.1-SNAPSHOT.jar
```

较小的 jar.original 是在 spring boot 执行 repackage 之前，由 maven 打包生成的 jar。

使用以下命令执行 spring boot 项目：

```bash
$ java -jar target/myproject-0.0.1-SNAPSHOT.jar
```

如果未继承 spring-boot-starter-parent 需要使用如下配置：

[spring-boot-maven-plugin Usage](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/maven-plugin/usage.html)

```xml
<build>
  ...
  <plugins>
    ...
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
      <version>2.1.7.RELEASE</version>
      <executions>
        <execution>
          <goals>
            <goal>repackage</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
    ...
  </plugins>
  ...
</build>
```

## Starter介绍

[从零开始开发一个Spring Boot Starter](https://www.jianshu.com/p/bbf439c8a203)

Starter是Spring Boot中的一个非常重要的概念，Starter相当于模块，它能将模块所需的依赖整合起来并对模块内的Bean根据环境（ 条件）进行自动配置。使用者只需要依赖相应功能的Starter，无需做过多的配置和依赖，Spring Boot就能自动扫描并加载相应的模块。

1. 它整合了这个模块需要的依赖库；
2. 提供对模块的配置项给使用者；
3. 提供自动配置类对模块内的Bean进行自动装配；

[Spring Boot（二）：Spring Boot中的Starter介绍](https://blog.csdn.net/jdfk423/article/details/82940924)

在使用SpringBoot做Web开发时，我们会在pom中加入spring-boot-starter-web的依赖。Starter的jar中没有一句java代码，有一个pom文件。引入启动器依赖的作用有两个：

1. 引入该场景的组件的所有需要的依赖
   如spring-boot-starter-web会引入spring-webmvc等模块的依赖，引入自己所需要的依赖。
2. 引入jar包的自动配置
starter都会引入spring-boot-starter的依赖，spring-boot-starter会引入spring-boot-autoconfigure的依赖，autoconfigure模块会根据XXXAutoConfiguration完成容器默认配置。

常用Starter依赖合集

| artifactId                             | 说明                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| spring-boot-starter                    | 核心模块，包含自动配置支持、日志库和对 YAML配置文件的支持。已经添加了其他功能的starter之后不需要再单独配置这个模块的<dependency> |
| spring-boot-starter-activemq           | 通过ActiveMQ支持JMS                                          |
| spring-boot-starter-amqp               | 支持Spring AMQP和Ribbit MQ                                   |
| spring-boot-starter-aop                | 包含 spring-aop和 AspectJ 来支持面向切面编程（AOP）。        |
| spring-boot-starter-artemis            | 通过Apache Artemis支持JMS                                    |
| spring-boot-starter-batch              | 支持 Spring Batch，包含 HSQLDB。                             |
| spring-boot-starter-cache              | 支持spring框架的缓存                                         |
| spring-boot-starter-cloud-connectors   | 支持使用spring云连接来简单的连接云平台服务，例如Cloud Foundry，Heroku |
| spring-boot-starter-data-cassandra     | 包含Spring Data Cassandra来支持Cassandra分布式数据库         |
| spring-boot-starter-data-couchbase     | 包含 Spring Data Couchbase来支持Couchbase数据库              |
| spring-boot-starter-data-elasticsearch | 包含Spring Data Elasticsearch来支持Elasticsearch搜索和分析引擎 |
| spring-boot-starter-data-gemfire       | 包含 Spring Data GemFire 支持GemFire分布式数据存储           |
| spring-boot-starter-data-jpa           | 包含 spring-data-jpa、spring-orm和 Hibernate来支持 JPA。     |
| spring-boot-starter-data-ldap          | 支持Spring Data LDAP                                         |
| spring-boot-starter-data-mongodb       | 包含 spring-data-mongodb来支持 MongoDB。                     |
| spring-boot-starter-data-neo4j         | 包含 Spring Data Neo4j来支持neo4j数据库                      |
| spring-boot-starter-data-redis         | 包含 Spring Data Redis 和 the Jedis client来支持redis数据库  |
| spring-boot-starter-data-rest          | 通过 spring-data-rest-webmvc支持以 REST 方式暴露 Spring Data 仓库。 |
| spring-boot-starter-data-solr          | 通过Spring Data Solr支持Solr搜索平台                         |
| spring-boot-starter-freemarker         | 支持使用FreeMarker views作为MVC框架构建web应用               |
| spring-boot-starter-groovy-templates   | 支持使用Groovy Templates views作为MVC框架构建web应用         |
| spring-boot-starter-hateoas            | 通过Spring MVC和Spring HATEOAS支持构建hypermedia-based RESTfulweb应用 |
| spring-boot-starter-integration        | 支持Spring Integration                                       |
| spring-boot-starter-jdbc               | 支持使用 JDBC访问数据库。                                    |
| spring-boot-starter-security           | 包含 spring-security。                                       |
| spring-boot-starter-test               | 包含常用的测试所需的依赖，如 JUnit、Hamcrest、Mockito和 spring-test等。 |
| spring-boot-starter-velocity           | 支持使用 Velocity作为模板引擎。                              |
| **spring-boot-starter-web**            | 支持 Web应用开发，包含 Tomcat和 spring-mvc。                 |
| spring-boot-starter-websocket          | 支持使用 Tomcat开发 WebSocket应用。                          |
| spring-boot-starter-ws                 | 支持 Spring Web Services。                                   |
| spring-boot-starter-actuator           | 添加适用于生产环境的功能，如性能指标和监测等功能。           |
| spring-boot-starter-remote-shell       | 添加远程 SSH支持。                                           |
| spring-boot-starter-jetty              | 使用 Jetty而不是默认的 Tomcat作为应用服务器。                |
| spring-boot-starter-log4j              | 添加 Log4j的支持。                                           |
| spring-boot-starter-logging            | 使用 Spring Boot默认的日志框架 Logback。不需要单独配置<dependency>, spring-boot-starter已经依赖了该模块。 |
| spring-boot-starter-tomcat             | 使用 Spring Boot默认的 Tomcat 作为应用服务器。               |

详见 <https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#using-boot-starter>

# 二、Hello World

[Writing the Code](https://docs.spring.io/spring-boot/docs/2.1.7.RELEASE/reference/html/getting-started-first-application.html#getting-started-first-application-code)

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Example.class, args);
    }

}
```
说明：
1. `@RestController`是表示这个类可用于处理HTTP请求， `@RestController`是`@Controller`和`@ResponseBody`的结合体，两个标注合并起来的作用。

2. `@RequestMapping("/")`指定controller的访问路径。它告诉spring所有路径是`/`的http请求都映射到`home()`方法处理。

3. `@EnableAutoConfiguration`告诉SpringBoot基于你已经添加jar依赖项，“猜”你将如何想配置Spring。如果spring-boot-starter-web已经添加Tomcat和Spring MVC,这个注释自动将假设您正在开发一个web应用程序并添加相应的spring设置。

   自动配置被设计用来和“Starters”一起更好的工作,但这两个概念并不直接相关。您可以自由挑选starter依赖项以外的jar包，springboot仍将尽力自动配置您的应用程序。

   spring通常建议我们将main方法所在的类放到一个root包下，@EnableAutoConfiguration（开启自动配置）注解通常都放到main所在类的上面，下面是一个典型的结构布局：

```
com
 +- example
     +- myproject
         +- Application.java
         |
         +- domain
         |   +- Customer.java
         |   +- CustomerRepository.java
         |
         +- service
         |   +- CustomerService.java
         |
         +- web
             +- CustomerController.java
```

这样@EnableAutoConfiguration可以从逐层的往下搜索各个加注解的类，例如，你正在编写一个JPA程序（如果你的pom里进行了配置的话），spring会自动去搜索加了@Entity注解的类，并进行调用。

4. 主程序中调用`SpringApplication.run`方法启动应用，运行spring容器，依次运行自动配置的 Tomcat Web 服务。`Example.class`作为`run()`方法的参数，告诉`SpringApplication`哪块是主要的spring组件，`args`数组用于将命令行参数传递给`SpringApplication`。

# 三、客户端访问路径和服务器资源的映射

1. 独立的tomcat中应用的访问路径是`http://host:port/contextPath/资源路径`，spring boot内嵌的tomcat中默认没有contextPath，并且DispatcherServlet默认映射`/`，直接拦截所有请求，所以`http://host:port/资源路径`就可进入springMVC组件。
2. springboot项目中公开静态资源可放在以下4个目录中，客户端访问时使用`http://host:port/资源相对静态资源目录的路径`，eg：a.html在static目录下，它的访问路径是`http://host:port/a.html`；b.html位于static/folder目录下，它的访问路径是`http://host:port/folder/b.html`。(详见 [SpringBoot之静态资源访问](https://blog.csdn.net/qq_34797335/article/details/80194137))
	1. 在src/main/resources/目录下创建static文件夹 
	2. 在src/main/resources/目录下创建resources文件夹 
	3. 在src/main/resources/目录下创建public文件夹 
	4. 在src/main/resources/目录下创建META-INF/resources文件夹 
3. application配置文件中`server.servlet.context-path`来指定contextPath，`spring.mvc.servlet.path`（2.1.5之前是 `server.servlet.path`）来指定DispatcherServlet映射的路径，完整的请求路径就是`http://host:port/contextPath/DispatcherServlet路径/controller路径或静态资源路径`。
4. springMVC组件（controller、handler等）的路径都是相对于DispatcherServlet来说的，如果一个controller的@RequestMapping定义为`/`，不设置contextPath：
	1. DispatcherServlet绑定`/`，访问`http://host:port/`进入这个controller；
	2. DispatcherServlet绑定`/spring`，访问`http://host:port/spring`进入这个controller。

	公开静态资源的访问类似。

参考 [url-pattern配置](../../JSP和Servlet/url-pattern配置.md)、[Servlet获取URL地址](../../JSP和Servlet/Servlet获取URL地址.md)、[SpringMVC页面访问不到静态文件](../SpringMVC页面访问不到静态文件.md)、[ContextLoaderListener和DispatcherServlet](../ContextLoadListener和DispatcherServlet.md)

# 四、配置文件

