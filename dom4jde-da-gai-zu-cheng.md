可以从这里 获得Dom4j的源代码：[gihub仓库](https://github.com/dom4j/dom4j)

下载好后Dom4j的源代码并解压，可以看到它的文件目录布局如下：

![](/assets/djcatalog.png)

前面做了很多铺垫性的介绍，现在我们终于可以进入到Dom4j的内部了。Dom4j的源代码在`dom4j-master\src\main\java\org\dom4j\*`

其主要文件结构如下:

\|-java\org\dom4j

\|-----bean

```
  \|---BeanAttribute.java

  \|---BeanAttributeList.java

  \|---BeanDocumentFactory.java

  \|---...
```

\|----datatype

```
  \|---DatatypeAttribute.java

  \|----DatatypeDocumentFactory.java

  \|---...
```

\|----dom

```
  \|---DomAttribute.java

  \|---DomAttributeNodeMap.java

  \|---DOMCDATA.java
```



