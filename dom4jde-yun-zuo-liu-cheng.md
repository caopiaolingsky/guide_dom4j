这里只简单阐述以下Dom4j的运作流程，如果要详细给出，还需要费些力气，仅仅靠文字是不够的。

前面已经较为详细的说明了Dom4是如何通过接口、类的定义来建立起XML文档树的对象模型了。在XML文档树对象模型建立好之后，Dom4j通过DocumentHelper这个帮助类来实现对单例类DocumentFactory的引用，并相关工厂方法做了引用。util文件夹提供了一系列工具类：

![](/assets/util.png)

这里面的类提供了一些XML处理中的常用方法，例如元素的索引、节点的比较等，方便其他类在进一步功能封装时调用。

在dom文件夹下实现了XML的DOM操作方法，这里面的的DOM类继承自相应的节点类，并且实现W3C标准规定的XML DOM接口：

![](/assets/dom.png)

io文件下暴露出了一些类和方法，如SAXReader、SAXWriter、DOMWriter、XMLWriter等，这些都是对dom文件夹下的dom类和方法的一些聚合、组合、封装，供我们编写程序时调用：

![](/assets/io.png)

