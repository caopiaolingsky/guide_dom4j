如果想要了解Dom4j的大致原理，我想，搞清楚Dom4j的一些列接口应该是第一步。Dom4j是面向对象实现的，它同时也大量使用了接口的技术，这给Dom4j带来了非常大的灵活性。

#### 用接口描述对象

Dom4j既然是采用面向对象的编程哲学来解决XML的解析问题的，那么Dom4j的首要工作就是建立起XML文档树的抽象对象模型。这样说的理由我前面已经说过，XML文件的内容会形成一个XML文档树结构，而每个元素、属性、文本、注释等都成为相应的节点，对XML文档的DOM解析操作就是对这棵抽象文档树上各个节点的作用。Dom4j为了建立XML文档树对象，首先它定义了一系列节点接口，用这些接口来约束规范每个节点对象所应该有的属性和方法。

#### Node接口

Node接口是一个节点接口原型，XML文档树上所有的节点接口都继承自Node接口，如下图所示：

![](/assets/nodetree1.png)

Node接口的部分实现代码如下：

```java
public interface Node extends Cloneable {
    // W3C DOM complient node type codes

    /** Matches Element nodes */
    short ANY_NODE = 0;

    /** Matches Element nodes */
    short ELEMENT_NODE = 1;

    /** Matches elements nodes */
    short ATTRIBUTE_NODE = 2;

    /** Matches elements nodes */
    short TEXT_NODE = 3;
    ...
     * @return true if this node supports the parent relationship or false it is
     *         not supported
     */
    boolean supportsParent();

    Element getParent();

    void setParent(Element parent);

    Document getDocument();

    void setDocument(Document document);

    boolean isReadOnly();

    boolean hasContent();

    String getName();
    ...
```

可以看到Node接口继承了Java.lang包中的Cloneable接口，使得所有节点支持继承自Object类的克隆方法clone\(\)，因为显然文档树中的节点都是可复制的。紧接着，在Node接口内部定义了一些所有节点的共性行为。

* **节点类型常量挂载**

在Dom4j中节点类型由short型常量表示，例如ANY\_NODE代表任意节点，ELEMENT\_NODE代表元素节点，ATTRIBUTE\_NODE代表属性节点，TEXT\_NODE代表文本节点。在Dom4j中一共定义了14中节点，当然其中部分节点还没有被实现，但预留了位置。同时，使用UNKNOWN\_NODE代表未知节点，它不属于XML文档树中的节点类型。

Dom4j中实现的节点类型有：

ANY\_NODE：代表任意节点

ELEMENT\_NODE：代表元素节点

ATTRIBUTE\_NODE：代表属性节点

TEXT\_NODE：代表文本节点

CDATA\_SECTION\_NODE：代表CDATA域节点，CDATA是一种特殊节点，它里面的内容不会被解析

ENTITY\_REFERNCE\_NODE：代表一个实体引用节点

PROCESSING\_INSTRUCTION\_NODE：代表一个处理指令节点

COMMENT\_NODE：代表一个注释节点，例如&lt;--我是注释--!&gt;

DOCUMENT\_TYPE\_NODE：代表一个Document type型节点，即文档类型声明节点

NAMESPACE\_NODE：名字空间节点

* **节点共性行为定义**

supportsParent\(\)方法：调用该方法可以知道当前节点是否支持向父级节点索引访问；

getParent\(\)方法：获取节点的父节点，如果该节点是root节点或不支持父级节点索引，就返回null；

setParent\(\)方法：指定当前节点的父类节点

getDocument\(\)方法：获取当前节点所在的文档节点

isReadOnly\(\)方法：判断当前节点是否是只读、不可更改的

hasContent\(\)方法：判断当前节点是否是Branch型节点，即它是否有子节点

getName\(\)方法：返回当前节点的名字

...



