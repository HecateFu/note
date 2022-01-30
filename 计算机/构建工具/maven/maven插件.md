# 简介 

mvn archetype:generate 命令来生成一个项目。那么这里的 archetype:generate 是什么意思呢？archetype 是一个插件的名字，generate是目标(goal)的名字。这个命令的意思是告诉 maven 执行 archetype 插件的 generate 目标。插件目标通常会写成 pluginId:goalId

 一个目标是一个工作单元，而插件则是一个或者多个目标的集合。比如说Jar插件，Compiler插件，Surefire插件等。从看名字就能知道，Jar 插件包含建立Jar文件的目标， Compiler 插件包含编译源代码和单元测试代码的目标。Surefire 插件的话，则是运行单元测试。

mvn本身不会做太多的事情，它不知道怎么样编译或者怎么样打包。它把构建的任务交给插件去做。插件定义了常用的构建逻辑，能够被重复利用。这样做的好处是，一旦插件有了更新，那么所有的 maven 用户都能得到更新。

# 常用插件

## maven-compiler-plugin

 默认情况下，用来编译Maven项目下的.java文件的工具是"maven-compiler-plugin"插件

## maven-resources-plugin

为了使项目结构更为清晰，Maven区别对待Java代码文件和资源文件，maven-compiler-plugin用来编译Java代码，maven-resources-plugin则用来处理资源文件。默认的主资源文件目录是src/main/resources，很多用户会需要添加额外的资源文件目录，这个时候就可以通过配置maven-resources-plugin来实现。此外，资源文件过滤也是Maven的一大特性，你可以在资源文件中使用${propertyName}形式的Maven属性，然后配置maven-resources-plugin开启对资源文件的过滤，之后就可以针对不同环境通过命令行或者Profile传入属性的值，以实现更为灵活的构建。

指定和排除参与构建的resoures  [maven资源文件的相关配置](https://www.cnblogs.com/pixy/p/4798089.html)

```xml
<build>
...
<resources>
    <resource>
        <directory>src/main/resources</directory>
        <excludes>
            <exclude>**/*.properties</exclude>
            <exclude>**/*.xml</exclude>
         </excludes>
        <filtering>false</filtering>
    </resource>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
</resources>
...
</build>
```



## maven-surefire-plugin

surefire 插件用来在maven构建生命周期的test phase执行一个应用的单元测试。它会产生两种不同形式的测试结果报告：1)纯文本 2)xml文件格式的。默认情况下，这些文件生成在工程的${basedir}/target/surefire-reports，目录下（basedir指的是pom文件所在的目录）。它可以运行任何testNG,Junit,pojo写的单元测试。参考：http://blog.sina.com.cn/s/blog_59ae3b350100yjwi.html

```xml
<plugins>
    <plugin>
    	<groupId>org.apache.maven.plugins</groupId>
    	<artifactId>maven-surefire-plugin</artifactId>
        <configuration>
        	<skip>true</skip>
        </configuration>
    </plugin>
    <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                     <source>1.7</source>
                     <target>1.7</target>
                     <encoding>UTF-8</encoding>
            </configuration>
      </plugin>
</plugins>
```



## build-helper-maven-plugin

http://blog.csdn.net/wangjunjun2008/article/details/17553503

http://blog.csdn.net/xinluke/article/details/51569237

把原有项目添加到Maven管理时，总会出现很多莫名奇妙的问题，其中之一便是Maven默认的项目结构和自己的项目结构不一致，导致无法编译源代码，更不用说部署、运行项目了。

Java程序开发，一般使用Eclipse、MyEclipse等工具，其源码目录为src，这与Maven默认的src/main/java不同。因此，在没有额外配置的情况下，使用Maven命令无法完成代码的编译。

针对这种情况，codehaus提供了build-helper-maven-plugin插件来支持自定义的项目目录结构(相对于Maven默认目录结构来说)。

Maven默认只允许指定一个主Java代码目录和一个测试Java代码目录。

虽然这其实是个应当尽量遵守的约定，但偶尔你还是会希望能够指定多个源码目录（例如为了应对遗留项目），build-helper-maven-plugin的add-source目标就是服务于这个目的，通常它被绑定到默认生命周期的generate-sources阶段以添加额外的源码目录。

需要强调的是，这种做法还是不推荐的，因为它破坏了 Maven的约定，而且可能会遇到其他严格遵守约定的插件工具无法正确识别额外的源码目录。

build-helper-maven-plugin的另一个非常有用的目标是attach-artifact，使用该目标你可以以classifier的形式选取部分项目文件生成附属构件，并同时install到本地仓库，也可以deploy到远程仓库。

## maven-war-plugin

http://blog.csdn.net/taiyangdao/article/details/51308579

WAR插件提供了4个goal：

war:war，对于POM中打包类型为war的项目，Maven的package 阶段默认执行该goal，从而构建出一个war文件。

war:exploded，通常用于开发阶段，创建一个包含所有war文件内容的解压缩的webapp目录（默认位于target/目录），以提高测试的效率。

war:inplace，类似于war:explode，区别只在于生成的webapp目录位于Web应用的源代码目录，即默认的src/main/webapp。

war:manifest，生成Manifest文件，默认位于Web应用的源代码目录。

插件属性：

encoding：强制字符集编码

warName：war包名字——platform.war

webappDirectory：产生war前，用于存放构建war包的目录——target/platform（可以把这个参数值设为本地tomcat的webapp目录，这样就可以直接把项目放到tomcat上了）。

warSourceDirectory：指定项目中哪个文件时web项目根目录，eg：eclipse的web项目中是webapp，myeclipse中是webRoot

## exec-maven-plugin

> http://www.mojohaus.org/exec-maven-plugin/index.html

使用maven执行系统命令或者java程序

eg:

```sh
mvn -q -Dexec.executable="echo" -Dexec.args='${project.artifactId}' --non-recursive org.codehaus.mojo:exec-maven-plugin:1.6.0:exec
```

作用：输出当前项目名称

参数说明：

`-q` 静默模式，只输出 mvn 执行中的 ERROR 日志，不输出默认的 INFO 日志

`-D<参数名>` 通过命令行给插件传递参数，该方式为java程序命令行传参的通用方式

`exec.executable` 插件执行的命令

`exec.args` 插件执行命令的参数

`--non-recursive` 简写`-N` 不对子模块递归执行，eg：一个父项目下Father面有3个子项目A、B、C，都生成jar包，则有Father.jar、A.jar、B.jar、C.jar，此时如果使用mvn clean install -N，则只会把Father.jar安装到本地仓库(~/.m2/repository)， 而不会安装其他三个包