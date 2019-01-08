可以从这里 获得Dom4j的源代码：[gihub仓库](https://github.com/dom4j/dom4j)

#### Dom4j项目的文件目录结构

* 下载好后Dom4j的源代码并解压，可以看到它的文件目录布局如下：

![](/assets/djcatalog.png)

* src文件夹下存放Dom4j的源代码，其中example文件下存有使用Dom4j的一些示例代码；main文件下为Dom4的核心功能代码，也将是我们的分析重点；test文件下为Dom4j开发过程的一些测试代码。

* 在main文件夹下存放Java代码，并按Java的包命名规则给定相应目录main\java\org\dom4j：

![](/assets/djindex.png)

* 下面对dom4j文件夹的内容逐一介绍

##### 文件夹：

**bean**：从Dom4j的XML DOM操作核心类继承而来，为方便生成支持转为Java Bean对象的XML文档对象。

**dtatype**：XML Schema Data Types

**dom**：XML文档树上各节点的DOM操作方法实现类

dtd：定义XML文档中document type域，使得支持自定义风格的XML文档

io：XML解析器的读写文件类，主要与硬盘文件打交道

jaxb：jaxb的全称是

Java Architecture for XML Binding

相当于一层适配器，使得Dom4j支持jaxb操作，即让Dom4j支持将XML文档元素绑定生成为Java类。

rule：包含XML解析的一些模式Pattern、过滤器filter、规则集rule set

swing：一层适配器，实现Dom4j里的XML 里文档树节点和Java Swing组件树节点接口的绑定

tree：定义了XML文档树中各个节点类，形成文档树结构

util：Dom操作方法实现需要用到的工具类

##### 其他文件：

dom4j目录下的其他文件是描述XML文档树节点的接口定义

* 从下一节开始将对上面提到的文件中的核心文件进行详细介绍。



