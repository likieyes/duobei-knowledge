Overview

Jetty作为嵌入式的Servlet服务器，用途非常广泛：

在开发时，我们用它在Eclipse里直接启动应用，而不是使用诸如Eclipse的Tomcat插件，先把几十兆应用打包，然后再copy到某个目录后再启动。在quickstart，showcase的测试目录里，都放着一个QuickstartServer.java，ShowcaseService.java之类的文件。
在功能测试时，我们用它实现容器外的测试，直接在用例里启动Jetty， 而不是先将应用打包部署到容器。在各个项目的BaseFunctionalTest中有演示。
甚至很多项目在运行期，也可以直接使用Jetty， 实现Micro-Service，详见Executable War章节。
改动代码后，按个回车快速重载应用

这都是被PlayFramework刺激的。 之前在Eclipse里用Main函数启动Jetty Server， 改动了代码后需要停止再启动Jetty，虽然已经免去了Tomcat插件那种重新打包几十兆的消耗，但比起php这种完全不用重启的来说还是慢，特别是关闭，启动一个新的JVM，消耗不小。

因此改动了Main函数，捕捉到回车后，重新reload context，并重新载入Class文件。感觉速度快了很多，而且不会像以前一样搞到很多个窗口。
但如果看到有class cast的错误，就还是重启一下数据库了。

while (true) {
	char c = (char) System.in.read();
	if (c == '\n') {
	        context.stop();

		WebAppClassLoader classLoader = new WebAppClassLoader(context);
		classLoader.addClassPath("target/classes");
		classLoader.addClassPath("target/test-classes");
		context.setClassLoader(classLoader);

		context.start();
	}
}
JettyFactory in SpringSide Core

JettyFactory 提供createServerInSource(int port, String contextPath)的函数，以src/main/webapp为Web应用目录创建Web　Server.

函数里有三个关键的设置：

一个是JVM退出时关闭Jetty的钩子，这样子就可以在整个功能测试时启动一次Jetty，然后让它在JVM退出时自动关闭。再次鄙视Junit没有在整次测试时期的setup/teardown控制函数。

二是connector.setReuseAddress(false)，在Windows下有个Windows + Sun的connector实现的问题，reuseAddress=true的话，重复启动同一个端口的Jetty不会报错。 所以必须设为false，代价是有时候上一次退出不干净，比如有TIME_WAIT时，会导致不能新的Jetty不能启动。但权衡之下还是设为False。

三同样是Windows下的问题，像javascript，css文件，会被Jetty锁住，不能修改保存，需要将webdefault.xml里的useFileMappedBuffer改为false

jsp taglib的tld文件不能找到的问题

这是Jetty一个很麻烦的设计，如果tld文件在当前的classloader里面就可以顺利载入，如果在parent的class loader里就会有因为安全原因而拒绝扫描，除非你像Jetty文档教的那样做了特别的设置。

在Eclipse中运行Jetty时， jetty会创建WebAppClassLoader， 然后把Eclipse那个带着项目Build path中所有Class的ClassLoader 设为parent....问题就来了.....

因此JettyFactory封装了一个setTldJarNames(String...)的方法，供你设定项目中用到的tld文件的jar包，详见QuickStartServer.java中的使用。

但是还是有最悲剧的时候，比如showcase项目用了springside-core中的tld文件，而且Eclipse里sprignside-core是以项目依赖而不是jar包依赖方式存在的，这时候就完全没办法了，只好把springside-core中的tld文件copy一份到showcase的WEB-INF目录。

在maven运行时，则需要设置surefire插件的属性

<useSystemClassLoader>false</useSystemClassLoader>
就会 这时候悲剧来了，jetty用的那个Jasper，只会扫描当前classloader里的jar，不会扫描parent的classloader里的，除非你org.eclipse.jetty.server.webapp.ContainerIncludeJarPattern去扫描某些jar。感觉还是直接设classloader简单点。

在Maven运行的Test里，则需要如下设置，原因懒得深究了。

<useSystemClassLoader>false</useSystemClassLoader>,
像Tomcat一样用Jetty

下载Jetty7 回来，解开发现bin里面居然没有windows下的bat文件，还好，自己打命令也一样

java -jar start.jar
然后把想运行的应用丢进它的webapps目录就可以了。如果要改变默认端口，命令行加上 -Djetty.port=8082 或者修改etc/jetty.xml
参考资料

Jetty 的工作原理以及与 Tomcat 的比较(IBM DW)