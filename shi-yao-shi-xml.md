### 什么是XML

上一节提到dom4j其实就是对XML格式文档进行解析的一个类包，它包含了XML文档的描述类、操作XML文档的许多方法。了解Python的人对这种类包形式将会很熟悉，Dom4j对于Java而言就相当于Python中的requests、beautifulSoup等模块包。在我们编写自己的程序时，我们可以很方便的引入Dom4j类包中的类与方法，来实现对XML文档的快捷高效操作。

##### HTML

XML是一种可拓展的标记语言。在说XML之前，先介绍一个大家非常熟悉的而且与XML非常类似的标记语言HTML。如果说你对HTML也感到非常陌生，那你接着看下去就会恍然大悟：哦，这就是HTML啊！事实上，你每天浏览的各种网页就是HTML文档，神奇吧？只不过，我们看到的网页中还包括有CCS代码、Js代码部分，它们分别负责对网页进行样式渲染和动态响应。但网页的基本框架依靠HTML来描述就够了，你完全可以只用HTML来制作一个网页。下面来看一个这样简单的网页：

![](/assets/dhsh_index.png)

这样的网页你可能会经常看到吧？它是我从我非常喜欢的一个网页游戏中截取出来的，这里它正提醒我登录的书签失效了，需要重新登录。那这个文档对应的源代码是怎样的呢？很简单，单击鼠标右键就可以看到它的源代码了。上面的网页所对应的源代码如下：

```html
<?xml version="1.0" encoding="utf-8"?><!DOCTYPE html PUBLIC "-//WAPFORUM//DTD XHTML Mobile 1.0//EN" "http://www.wapforum.org/DTD/xhtml-mobile10.dtd"><html>
<head>
    <title>大话水浒_书签失效</title>
    <script src="http://203.195.129.90/html/js/wml2html.js"></script>
    <link href="http://203.195.129.90/html/css/gamestyle.css" type="text/css" rel="stylesheet"/>
</head>
<body>
<div id="main_body">


<p>
你的书签已失效，请重新登录.
<br />
<a href="http://3d941a-0.gz.1251001071.clb.myqcloud.com/">重新登录</a>
</p>

<p>
小Q报时_(21:17)
</p>
<img src="http://c.cnzz.com/wapstat.php?siteid=1000128905&amp;r=&amp;rnd=311916666" width="0" height="0"/><!-- <p>DB 0/0, MC 1/0, Time 0.016s</p> -->
</div>
</body></html>
```

这个文档给你一种结构化的感觉对吧？可以看到它的最外层是一个`<html>`标签，表示包含在其中的内容是一个HTML文档。在html标签中的第一个标签是`head`，在`<head>`和`</head>`之间的内容就表示这是这个文档的头部。在这个头部标签里，又内嵌了一个`<title>`标签，表示大话水浒\_书签失效这部分内容是这个网页的标题。总结来看，在HTML中，每个标签都由`<tagName>`和`</tagName>`以及它们之间的内容组成。内容传递具体信息，tagName定义内容的含义或者属性。这样做可以方便其他读者或者程序（例如我们要将的Dom4j）来阅读解析这个文档中的内容。

#### XML

XML和HTML非常类似，应用也非常广泛，据我所知，很多程序的配置文件都会写成XML格式的，例如在做安卓程序开发时，如果你要做APP的界面布局，那么你将大量使用到XML语句。使用XML语句可以很方便地来描述你的程序的界面有哪些元素，这些元素的属性是怎样的，它们的布局关系是怎样的。XML与HTML相比具有以下特点：

* XML 指可扩展标记语言（EXtensible Markup Language）

* XML是不作为的语言，仅仅是一种纯文本。

* XML 的设计宗旨是传输数据，而非显示数据

* XML 标签没有被预定义。您需要自行定义标签。

* XML 被设计为具有自我描述性。

其中指得指出的是XML标签没有被预定义，需要自定义标签。而在HTML中，很多标签都被定义了具体含义，例如上面提到的`<html>`标签专指这是一个html文档，`<head>`标签专指这是html文档的头部。还有一点就是XML是一种不作为的语言，意思是说它本身不会产生行为也不描述行为，只是单纯的用来存储、传输结构化的数据。

