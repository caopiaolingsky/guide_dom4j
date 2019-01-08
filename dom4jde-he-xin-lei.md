如果想要了解Dom4j的大致原理，我想，搞清楚Dom4j的一些列接口应该是第一步。Dom4j是面向对象实现的，它同时也大量使用了接口的技术，这给Dom4j带来了非常大的灵活性。

#### 用接口描述对象

Dom4j既然是采用面向对象的编程哲学来解决XML的解析问题的，那么Dom4j的首要工作就是建立起XML文档树的抽象对象模型。这样说的理由我前面已经说过，XML文件的内容会形成一个XML文档树结构，而每个元素、属性、文本、注释等都成为相应的节点，对XML文档的DOM解析操作就是对这棵抽象文档树上各个节点的作用。Dom4j为了建立XML文档树对象，首先它定义了一系列节点接口，用这些接口来约束规范每个节点对象所应该有的属性和方法。

#### Node接口

Node接口是一个节点接口原型，XML文档树上所有的节点接口都继承自Node接口，如下图所示：

![](/assets/nodetree1.png)Node接口的部分实现代码如下：

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

可以看到

