### 什么是Dom操作

前面我们介绍了Dom4j是一个解析XML文档的Java类包，也简要介绍了什么是XML：XML是一种标签化语言，被用来描述结构化的数据，从而便于数据的存储和传输。现在还有一个紧迫的问题，Dom4j对XML文档的解析是指什么呢？其实，Dom4j对XML文档进行解析的操作就被称为Dom操作。

#### XML文档的树结构

与HTML文档一样，XML文档也表现树状结构。例如下面一段XML代码：

```xml
<bookstore>
<book category="children">
<title lang="en">Harry Potter</title>
<author>J K. Rowling</author>
<year>2005</year>
<price>29.99</price>
</book>
<book category="cooking">
<title lang="en">Everyday Italian</title>
<author>Giada De Laurentiis</author>
<year>2005</year>
<price>30.00</price>
</book>
<book category="web" cover="paperback">
<title lang="en">Learning XML</title>
<author>Erik T. Ray</author>
<year>2003</year>
<price>39.95</price>
</book>
<book category="web">
<title lang="en">XQuery Kick Start</title>
<author>James McGovern</author>
<author>Per Bothner</author>
<author>Kurt Cagle</author>
<author>James Linn</author>
<author>Vaidyanathan Nagarajan</author>
<year>2003</year>
<price>49.99</price>
</book>
</bookstore>
```

它就描述了这样一个树状结构：

![](/assets/xml_tree.png)

在树根处，也即文档的最外层是一个`<bookstore>`标签，表示这个文档描述了一个“书店”。在这个“书店”里有些什么呢？我们看到正如现实世界那样是一本一本的书，每个`<book>`标签就描述了一本书这样一个数据结构。在每本书里，又有相应标签描述了这本书的各种属性，例如`<title>`标签描述书的标题，`<author>`标签描述书的作者。如果你细心，你会发现这个深入解析Dom4j文档本身的结构也呈现为一种树状结构，因此我们可以用XML语言来描述它：

```xml
<guide_dom4j>
<authour>赵飞</author>
<introduction/>
<part1>
<title>Dom4j的功能与使用</title>
<article>Dom4j简介</article>
<article>什么是XML</article>
<article>什么是Dom操作</article>
...
</part1>
<part2>
...
</part2>
<part3>
...
</part3>

</guide_dom4j>
```

上面的XML文档就是对本文档组成结构的一个较为全面的描述，事实上，我所使用的编写此文档的工具git book正是基于这样的技术来组织我的文档内容并实现对其排版的。























