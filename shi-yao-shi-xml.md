### 什么是XML

上一节提到dom4j其实就是对XML格式文档进行解析的一个类包，它包含了XML文档的描述类、操作XML文档的许多方法。了解Python的人对这种类包形式将会很熟悉，Dom4j对于Java而言就相当于Python中的requests、beautifulSoup等模块包。在我们编写自己的程序时，我们可以很方便的引入Dom4j类包中的类与方法，来实现对XML文档的快捷高效操作。

##### HTML

XML是一种可拓展的标记语言。在说XML之前，先介绍一个大家非常熟悉的而且与XML非常类似的标记语言HTML。如果说你对HTML也感到非常陌生，那你接着看下去就会恍然大悟：哦，这就是HTML啊！事实上，你每天浏览的各种网页就是HTML文档，神奇吧？只不过，我们看到的网页中还包括有CCS代码、Js代码部分，它们分别负责对网页进行样式渲染和动态响应。但网页的基本框架依靠HTML来描述就够了，你完全可以只用HTML来制作一个网页。下面来看一个这样简单的网页：

![](/assets/dhsh_index.png)

这样的网页你可能会经常看到吧？



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









dom4j，顾名思义，它就是支持对XML文件进行**DOM操作**的一个类库。那么什么是DOM操作呢？事实上，DOM操作就在我们每个人的日常生活中。你每天浏览网页（静态网页除外）其实就是在执行DOM操作。

* 首先，我们先来看看XML文件的结构。众所周知，浏览器中的网页大多数都是由HTML语言（XML语法和HTML语法非常类似）描述的，例如下面就是我喜欢的一个wap网页游戏的部分首页**HTML**源代码：

  \`\`\`js  
  &lt;  
  !--HTML文件结构--  
  &gt;

&lt;  
  ?xml  
  version="1.0" encoding="utf-8"?  
  &gt;

&lt;  
  !DOCTYPE html PUBLIC "-//WAPFORUM//DTD XHTML Mobile 1.0//EN" "[http://www.wapforum.org/DTD/xhtml-mobile10.dtd](http://www.wapforum.org/DTD/xhtml-mobile10.dtd)"  
  &gt;

&lt;  
  html  
  &gt;

&lt;  
  head  
  &gt;

&lt;  
  title  
  &gt;  
  大话水浒  
  &lt;  
  /  
  title  
  &gt;

&lt;  
  script  
  src  
  =  
  "[http://203.195.129.90/html/js/wml2html.js](http://203.195.129.90/html/js/wml2html.js)"  
  &gt;  
  &lt;  
  /  
  script  
  &gt;

&lt;  
  link  
  href  
  =  
  "[http://203.195.129.90/html/css/gamestyle.css](http://203.195.129.90/html/css/gamestyle.css)"  
  type  
  =  
  "text/css"  
  rel  
  =  
  "stylesheet"  
  /  
  &gt;

&lt;  
  /  
  head  
  &gt;

&lt;  
  body  
  &gt;

&lt;  
  div  
  id  
  =  
  "main\_body"  
  &gt;

&lt;  
  p  
  &gt;

&lt;  
  img  
  src  
  =  
  "[http://203.195.129.90/image/app/21/waterth/logo.png](http://203.195.129.90/image/app/21/waterth/logo.png)"  
  /  
  &gt;

&lt;  
  br  
  /  
  &gt;

帐号:

&lt;  
  br  
  /  
  &gt;

&lt;  
  input  
  name  
  =  
  "username"  
  type  
  =  
  "text"  
  value  
  =  
  ""  
  /  
  &gt;

&lt;  
  /  
  div  
  &gt;

&lt;  
  /  
  body  
  &gt;

&lt;  
  /  
  html

    再来看看dom4j自带的一个示例**XML**文件`\dom4j-master\xml\test\journal.xml`：

&lt;  
?xml  
version="1.0"?  
&gt;

​

&lt;  
series  
xmlns:xsi  
=  
"[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"

xsi:noNamespaceSchemaLocation  
=  
"journal.xsd"  
&gt;

​

&lt;  
entry  
&gt;

&lt;  
date  
&gt;  
May 19, 2002  
&lt;  
/  
date  
&gt;

&lt;  
text  
&gt;

Sample text. We should support some markup here.

&lt;  
/  
text  
&gt;

&lt;  
/  
entry  
&gt;

​

&lt;  
entry  
&gt;

&lt;  
date  
&gt;  
May 19, 2002 10:27  
&lt;  
/  
date  
&gt;

&lt;  
text  
&gt;

Second batch.

&lt;  
/  
text  
&gt;

&lt;  
/  
entry  
&gt;

​

&lt;  
/  
series  
&gt;  
\`\`\`

可以看到，XML，HTML它们都由层次化的标签（tag）+内容（content）组成。例如在journal.xml中文档开头是`<?xml version="1.0"?>`，意在声明这是一个XML格式的文件。紧接着就是嵌套的多个元素（元素=tag+content）。在一个元素中，tag名描述了其中内容的含义、类型。例如其中的`<date>May 19, 2002 10:27</date>`，标签名就是date（以&lt;&gt;包装为开始标签，以&lt;/&gt;包装为结束标签），它表示这个元素的内容19,2002 10:27是一个日期（date）。

