--- 
layout: post
title: Running Maven Scala Application on Eclipse WTP
tags: 
- Eclipse
- Maven
- Scala
status: draft
type: post
published: false
---

Let's build a simple <a href="http://www.scalatra.org/">Scalatra</a> application for which I use <a href="http://maven.apache.org/">Maven</a> to build. And since the <a href="http://www.scala-ide.org/">Scala support for Eclipse</a> is quite decent nowadays, I decided to use that too.

So, what you need is: Eclipse (3.7), WTP (3.3.0), Scala IDE (2.0.0beta10), Maven integration for Eclipse (1.0.0), Maven integration for Scala IDE (0.3.1) and Maven integration for WTP (0.14.0).

Now when you import your Maven project everything should go without much problems. Until... you want to run it from Eclipse. You set up your servers, run and <em>BOOM!</em> end up with:

[sourcecode]
java.lang.NoClassDefFoundError: scala/ScalaObject
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:634)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:277)
	at java.net.URLClassLoader.access$000(URLClassLoader.java:73)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:212)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:205)
	at org.mortbay.jetty.webapp.WebAppClassLoader.loadClass(WebAppClassLoader.java:392)
	at org.mortbay.jetty.webapp.WebAppClassLoader.loadClass(WebAppClassLoader.java:363)
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:634)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:277)
	at java.net.URLClassLoader.access$000(URLClassLoader.java:73)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:212)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:205)
	at org.mortbay.jetty.webapp.WebAppClassLoader.loadClass(WebAppClassLoader.java:392)
	at org.mortbay.jetty.webapp.WebAppClassLoader.loadClass(WebAppClassLoader.java:363)
	at org.mortbay.util.Loader.loadClass(Loader.java:91)
	at org.mortbay.util.Loader.loadClass(Loader.java:71)
	at org.mortbay.jetty.servlet.Holder.doStart(Holder.java:73)
	at org.mortbay.jetty.servlet.ServletHolder.doStart(ServletHolder.java:242)
	at org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:50)
	at org.mortbay.jetty.servlet.ServletHandler.initialize(ServletHandler.java:736)
	at org.mortbay.jetty.servlet.Context.startContext(Context.java:140)
	at org.mortbay.jetty.webapp.WebAppContext.startContext(WebAppContext.java:1282)
	at org.mortbay.jetty.handler.ContextHandler.doStart(ContextHandler.java:518)
	at org.mortbay.jetty.webapp.WebAppContext.doStart(WebAppContext.java:499)
	at org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:50)
	at org.mortbay.jetty.handler.HandlerCollection.doStart(HandlerCollection.java:152)
	at org.mortbay.jetty.handler.ContextHandlerCollection.doStart(ContextHandlerCollection.java:156)
	at org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:50)
	at org.mortbay.jetty.handler.HandlerCollection.doStart(HandlerCollection.java:152)
	at org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:50)
	at org.mortbay.jetty.handler.HandlerWrapper.doStart(HandlerWrapper.java:130)
	at org.mortbay.jetty.Server.doStart(Server.java:224)
	at org.mortbay.component.AbstractLifeCycle.start(AbstractLifeCycle.java:50)
	at org.mortbay.xml.XmlConfiguration.main(XmlConfiguration.java:985)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:616)
	at org.mortbay.start.Main.invokeMain(Main.java:194)
	at org.mortbay.start.Main.start(Main.java:534)
	at org.mortbay.start.Main.start(Main.java:441)
	at org.mortbay.start.Main.main(Main.java:119)
[/sourcecode]

What you may notice is that's because the Scala library can not be found on the classpath; even though you see the Scala container in your project!

I found it weird that only adding the JAR from the local Maven repository gets it right.
