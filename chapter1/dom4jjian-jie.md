Dom4j的简介

* 本手册分析的dom4j版本：v2.1.1

* dom4j维护官网地址：[dom4j官网](http://dom4j.org)，gihub仓库地址：[gihub仓库](https://github.com/dom4j/dom4j)



* Dom4j的全称是Document Object Model for Java，故名思义，Dom4j是用于在Java平台实现Dom操作的。与一般的Java程序不同，Dom4j不是直接让用户来运行使用的（虽然它自带了使用案例程序）。Dom4j是一个Java类包，它里面提供了一系列类、对象、接口、方法来方便程序员在需要Dom操作时直接引用。某种意义上，Dom4j就相当于C语言里的一个函数库，相当于Python里的一个模块。众所周知，如果你想要用Python做一个爬虫程序，你肯定不会自己来写网络请求get\(\)，post\(\)方法的，当然如果你愿意的话。这时，引用别人写好的模块如request将会是你更好的选择，它可以帮助你节省开发时间并提高程序效率。同样地，如果你现在需要编写一个具有XML解析功能的程序、或者你的程序需要从一个XML文件中读取数据，那么你也千万不要自己从头来干：从Java的IO开始，因为你可以选择好用的轮子：它就是我将要介绍的Dom4j。

* 那么Dom4j到底是怎样的呢？别急，下面我们一步一步揭开它的神秘面纱。

> **先看一个dom4j的官方释义**：Dom4j is an easy to use, open source library for working with XML, XPath and XSLT on the Java platform using the Java Collections Framework and with full support for DOM, SAX and JAXP.

是的，dom4j是一个开源的、基于Java语言的类库。它专用于在Java平台上处理XML, XPath,XSLT等类型的文件（本手册仅关注于该类库在处理解析**XML**文件方面的作用）。这里不再赘述dom4j的各种优越性，我们只需要知道：**dom4j是一个使用简单，执行高效的XML解析包**，就连Sun公司的JAXM也在使用它解析XML文件。现今，dom4j已经成为jar包的一个标配。



